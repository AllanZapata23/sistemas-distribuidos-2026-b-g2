# ADR-C3-02: CCE across two availability zones with CPU-based autoscaling

## Status

Proposed

## Context

All OptiView services run as containers on a single Docker host per environment, with one replica per service and no autoscaling (`05-architecture/deployment.md`, sections 1 and 5). This has three effects. The host is a single point of failure, its size must cover the peak load all day even when the store is closed, and every deploy follows a Recreate strategy with a short downtime, because there is no second replica behind a load balancer (`10-devops/environments.md`, "Production deploy strategy").

Our scalability requirement NFR-003 in `04-requirements/non-functional.md` asks to scale horizontally when CPU passes 70%, to react to a spike in less than two minutes, to scale down without interrupting active traffic, and to stay at a maximum of three instances per service. It also states that the strategy is stateless horizontal scaling. Sections 2 (Reliability and Cost), 3 (Scalability) and 5 (rolling deploys) of the design note depend on how the services are run.

## Decision

The services (`opti-api-gateway`, `opti-auth-api`, `opti-customers-api`, `opti-products-api`, `opti-sales-api`, `opti-workflow` and `opti-worker`) run as workloads in one CCE cluster with node pools in AZ1 and AZ2. Each API and the gateway keep at least 2 pods, spread across both zones with anti-affinity rules, and a Horizontal Pod Autoscaler adds pods when average CPU is above 70% for 2 minutes, up to 3 pods, as NFR-003 requires. `opti-worker` stays at one pod because its jobs are scheduled and idempotent. The Cluster Autoscaler adds an ECS node when there is no room for new pods and removes it when it is empty. Each workload keeps the health endpoint it already exposes for readiness and liveness probes, so the ELB only sends traffic to healthy pods. Images are pulled from SWR, built from the same Dockerfiles that the team already has in each `deploy/` folder, and new versions are deployed with rolling updates.

## Alternatives considered

- One large ECS with Docker Compose, sized for the peak: rejected because it keeps the single point of failure, we pay for peak capacity even at night when the store is closed, and deploys still need a short downtime.
- A fixed group of ECS instances behind the ELB with manual scaling: rejected because a person has to notice the load and add machines, which cannot meet the 2-minute reaction time of NFR-003, and each instance would still need its own Compose setup and updates.
- Serverless containers (CCI): rejected for now because `opti-workflow` and the APIs need stable connections to RDS and DCS inside the VPC, and the team already knows the Docker images and Compose services, which map more directly to CCE workloads.

## Consequences

- Positive: losing one node or one availability zone leaves the pods of the other zone serving traffic through the ELB, the cluster grows only when the load needs it, and rolling updates remove the downtime of the current Recreate deploys.
- Negative: the team has to learn Kubernetes concepts (deployments, HPA, probes, anti-affinity) and maintain manifests in addition to the existing Compose files that are still used for local development.
- Negative: the CCE control plane and the minimum of two pods per service cost more than one small host at low traffic; we accept it because availability for the store is the main goal of this design.
