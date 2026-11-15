# Architecture design note — The Illusionists

## 1. Target architecture

![Target architecture](target-architecture.png)

OptiView is the system our team builds for optical stores. It manages patients (the `customers` domain), frames and lenses (the `products` domain), work orders, quotations and payments (the `sales` domain), and the users and roles of the store (`auth`). After MVP 2 the code is split into bounded contexts, each with its own `-db`, `-api` and `-portal` repository, plus a gateway, a saga orchestrator (`opti-workflow`), a scheduler (`opti-worker`) and a shared infrastructure repository (`opti-infra`).

The way it is deployed has not caught up with that split. According to `05-architecture/deployment.md`, every environment still runs on a single Docker host with Docker Compose, with one replica per service and no autoscaling, and the observability stack (OpenTelemetry collector, Prometheus and Grafana from `opti-infra/compose.yml`) runs on the same machine it is supposed to watch. The deploy strategy described in `10-devops/environments.md` is closer to "Recreate": pull the new image, run `docker compose up -d` and accept a short downtime.

The target design moves this to Huawei Cloud without rewriting the services:

- **Entry:** users reach the portals through CDN, and the API through WAF and a dedicated ELB with an EIP. The ELB distributes requests between two copies of `opti-api-gateway`, one in each availability zone.
- **Compute:** one CCE cluster with node pools in AZ1 and AZ2 inside the VPC `optiview-vpc`. The gateway, the four domain APIs (`opti-auth-api`, `opti-customers-api`, `opti-products-api`, `opti-sales-api`) and `opti-workflow` run in both zones. `opti-worker` runs as a single scheduled workload, because its jobs are idempotent and running them twice would only repeat work.
- **Data:** following ADR-003 (database-per-service), each `-db` repository becomes its own RDS for PostgreSQL instance, primary in AZ1 and synchronous standby in AZ2. Redis, which today holds the saga state of `opti-workflow` and is the candidate cache of the gateway, moves to DCS in master/standby mode.
- **Static front ends:** the five portals are builds of static files, so they are stored in OBS and served through CDN instead of running as containers.
- **Operations:** images go to SWR, telemetry goes to AOM, APM and LTS, and database backups are kept in OBS. A NAT Gateway gives the nodes outbound access without exposing them.

| Component in the diagram | Defined in (file of your team's -docs) |
|---|---|
| opti-api-gateway | `09-microservices/services/01-api-gateway/README.md` |
| opti-auth-api, opti-auth-db (RDS) | `09-microservices/services/02-auth-service/README.md` |
| opti-sales-api, opti-sales-db (RDS) | `09-microservices/services/03-sales-service/README.md` |
| opti-customers-api, opti-customers-db (RDS) | `05-architecture/overview.md` (section 4, service ↔ repository mapping) and `06-data/models.md` (`patients_schema`) |
| opti-products-api, opti-products-db (RDS) | `05-architecture/overview.md` (section 4) and `06-data/models.md` (`inventory_schema`) |
| opti-workflow (saga orchestrator) | `05-architecture/decisions/records/ADR-004-place-order-saga.md` |
| opti-worker (scheduled jobs) | `05-architecture/overview.md` (section 4) |
| One database per service | `05-architecture/decisions/records/ADR-003-database-per-service.md` |
| DCS for Redis (gateway cache, saga state) | `09-microservices/service-catalog.md` |
| Portals on OBS + CDN (opti-front, opti-auth-portal, opti-customers-portal, opti-products-portal, opti-sales-portal) | `05-architecture/overview.md` (section 2, clients) |
| ELB, CCE node pools, topology per environment | `05-architecture/deployment.md` |
| AOM, APM and LTS (observability) | `13-operations/observability.md` |

## 2. Five principles

| Principle | Current weakness | Evidence in -docs | Design decision | Huawei Cloud service |
|---|---|---|---|---|
| Reliability | Every environment runs on one Docker host with one replica per service, so if the host fails the whole store stops, and each deploy causes a short downtime. | `05-architecture/deployment.md` (sections 1 and 5) | Run at least two replicas of every API, one per AZ, behind a load balancer, and put every database in a primary/standby pair across AZs. | ELB, CCE (node pools in AZ1 and AZ2), RDS for PostgreSQL primary/standby |
| Security | The secrets store for staging and production is a pending team decision, and the identity provider (Keycloak) is not deployed in any environment yet. | `05-architecture/deployment.md` (section 5) and `07-api/authentication.md` | Keep all secrets in a managed secret store injected into the pods at runtime, and filter public traffic before it reaches the gateway. | DEW (CSMS + KMS), WAF |
| Performance efficiency | The gateway has no cache decided; Redis is only a candidate for rate-limit counters, and reports compete with the write path of Sales. | `09-microservices/service-catalog.md` and `05-architecture/decisions/records/ADR-006-cqrs-sales-reports.md` | Use a managed Redis for the gateway cache and the rate-limit counters, and keep the CQRS read models of ADR-006 so report queries do not load the sales write model. | DCS for Redis |
| Cost | Scalability is still a pending decision, so the only safe option today is a single host sized for the peak, which is paid for even when the store is closed. | `04-requirements/non-functional.md` (NFR-003) | Scale the APIs between 2 and 3 pods by CPU, let the cluster add nodes only when needed, and serve the portals as static files instead of containers. | CCE with HPA and Cluster Autoscaler, OBS + CDN |
| Operations | Logs, metrics and traces depend on containers we operate ourselves (otel-collector, Prometheus, Grafana) on the same host they monitor. | `13-operations/observability.md` | Send telemetry to managed services that do not fail together with the application, and keep the existing trace ids and correlation ids. | AOM, APM, LTS |

The most important row is the first one. Our documents already describe a clean split by domain: one database per bounded context (ADR-003), services that integrate only through REST, sagas (ADR-004) and asynchronous side effects (ADR-005). All of that still depends on one machine, so a hardware failure would take down every context at the same time. Moving to two availability zones removes that single point of failure without changing the code of the services, because the APIs keep their state in PostgreSQL and Redis, not in memory, and they read their configuration from environment variables (`10-devops/environments.md`).

The security row follows the same idea. The variables such as `DATABASE_URL`, `REDIS_URL` and `JWT_SECRET` are already separated per environment, so the missing piece is a real place to keep them, not a change in the code. For cost, I preferred managed services over running our own PostgreSQL, Redis or Grafana: a student team cannot be on call for them, and paying for a managed standby is cheaper than losing a day of sales. Finally, `opti-worker` already sends one correlation id per run on every call, so moving the telemetry to AOM and LTS keeps the ability to follow one operation across services.

## 3. Scalability

Our requirement NFR-003 in `04-requirements/non-functional.md` already defines the rules: scale horizontally when CPU goes above 70%, react to a sudden spike in less than two minutes, scale down without cutting active traffic, and keep a maximum of three instances per service, because OptiView serves one optical store per deployment. The strategy written there is stateless horizontal scaling, which fits our services: the saga state of `opti-workflow` is saved in Redis after every step, and the domain data is in PostgreSQL.

| Service | Scaling metric | Threshold (number and unit) | Min instances | Max instances |
|---|---|---|---|---|
| opti-api-gateway | Average CPU utilization of the pods | 70 % for 2 minutes | 2 | 3 |
| opti-sales-api | Average CPU utilization of the pods | 70 % for 2 minutes | 2 | 3 |

I chose these two because every request passes through the gateway, and `opti-sales-api` handles the busiest part of the day: quotations, work orders, payments and the sales reports. The minimum is 2 so that each availability zone always has one pod and losing a zone does not stop the service; the maximum is 3 because that is the limit in NFR-003. Scale-down waits five minutes below the threshold so that pods are not removed and added again every minute. If there is no room for a new pod, the CCE Cluster Autoscaler adds an ECS node in the zone that needs it, and removes it later when it is empty.

## 4. Disaster recovery

- Critical data store: `opti-sales-db` (RDS for PostgreSQL), because it holds quotations, work orders and payments; losing it means losing money the store already charged and orders it already promised to patients.
- RTO: 5 minutes
- RPO: 0 minutes (NFR-008 asks for less than 1 second)
- DR pattern (chapter 11): active-standby across two availability zones in the same region (RDS primary in AZ1, synchronous standby in AZ2 with automatic failover), complemented by daily automated backups in OBS for logical errors.
- Why the pattern meets those numbers: Because the standby confirms every transaction synchronously before the primary answers the commit, no confirmed sale is lost when the primary fails, which gives an RPO of zero. RDS detects the failure and promotes the standby automatically, usually within one or two minutes, and the services reconnect to the same RDS address without manual steps, which keeps recovery inside the 5-minute RTO.

The numbers come from NFR-008 in `04-requirements/non-functional.md`, which sets RTO below 5 minutes and RPO below 1 second for the failure of the primary database. A daily backup alone could not meet them: it could lose up to 24 hours of sales, and restoring a dump takes much longer than five minutes. The backups are still useful, but for a different problem: if a bug or a person deletes data by mistake, the standby copies the mistake immediately, so only a backup from before the error can recover it. NFR-008 also accepts an RTO of 15 minutes and an RPO of 5 minutes when a whole availability zone is lost, and this design is better than that, because the standby already lives in the other zone. A full region disaster (RTO 4 hours, RPO 1 hour) is out of scope for this design and would need cross-region backup copies.

## 5. Automated deployment

Today every service repository has two GitHub Actions workflows: `ci.yml`, which runs on pull requests to `develop`, `qa` and `main`, and `publish.yml`, which builds the Docker image and pushes it to GitHub Container Registry with the tag `latest` when something is merged into `develop`. The `-db` repositories have `db-ci.yml` and a Flyway image that applies the migrations. There is no automated deploy job yet: deploying means running Docker Compose on the host. The table shows the pipeline I propose, using the files that already exist and adding SWR and CCE.

| Stage | Trigger | Tool or service | File in your team's repositories |
|---|---|---|---|
| Build | Pull request to `develop`, `qa` or `main` | GitHub Actions, Maven (`mvn -B verify`) | `opti-sales-api/.github/workflows/ci.yml` |
| Test | Same pull request (unit and integration tests in the verify phase); the migrations are validated in the `-db` repository | GitHub Actions, Maven, Flyway | `opti-sales-api/.github/workflows/ci.yml`, `opti-sales-db/.github/workflows/db-ci.yml` |
| Image | Merge to `develop` | Docker Buildx; today pushed to GHCR, target SWR with a version tag | `opti-sales-api/.github/workflows/publish.yml`, `opti-sales-api/deploy/Dockerfile` |
| Deploy to qa | Merge to `qa` | Flyway migration job, then update of the CCE workload with the new tag | `opti-sales-db/deploy/Dockerfile`, `opti-sales-api/deploy/compose.yml` |
| Deploy to production | Merge to `main` after manual approval of the Tech Lead | CCE rolling update across both AZs | `opti-infra/compose.yml` |
| Rollback | Failed health check after the deploy, or manual decision | Redeploy of the previous image tag in CCE | `opti-docs/05-architecture/deployment.md` (section 6) |

Two changes make this pipeline safe. First, images must be tagged with a version or the commit hash instead of only `latest`, because the rollback in `deployment.md` (section 6) is "redeploy the previous image tag", and that is impossible if every build overwrites the same tag. Second, migrations run before the new API version and must stay backward compatible, because `deployment.md` explains that Flyway undo needs the paid edition and the team's fallback is a hand-written compensating migration. With a rolling update and two pods per zone, the short downtime of the current Recreate strategy also disappears.

## 6. Decisions

- [ADR-C3-01](ADR-C3-01.md): Use RDS for PostgreSQL with a synchronous standby in a second availability zone for every OptiView database.
- [ADR-C3-02](ADR-C3-02.md): Run OptiView services on CCE across two availability zones with CPU-based horizontal autoscaling.
