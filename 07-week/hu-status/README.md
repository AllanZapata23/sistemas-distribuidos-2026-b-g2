<!-- HU-STATUS TEMPLATE - do NOT remove the <!-- ... --> markers or the table headers.
     Your weekly grade is read AUTOMATICALLY from this file:
       07-week/hu-status/README.md  (inside YOUR fork). English. -->

# Weekly Status - Week 07

<!-- CONFIG-START - must match your profile repo (username/username) CONFIG -->
- FULL_NAME: Allan Zapata Torres
- GITHUB_USER: AllanZapata23
- TEAM: The Illusionists
- SPRINT_GOAL: Document inter-service communication (REST, gRPC, messaging) and versioned API contracts / contract testing for OptiView's evolutionary microservices path; produce visual summaries for both topics.
<!-- CONFIG-END -->

> **Week window:** Monday **2026-09-14** → Sunday **2026-09-20** (each course week is Mon–Sun).
> **Week 07 deliverable — communication + contracts.** Study notes and visuals on **how services talk** (REST / gRPC / messaging) and **how contracts evolve safely** (versioning + contract testing).
> **Course fork:** https://github.com/AllanZapata23/sistemas-distribuidos-2026-b-g2
> **Author email:** `azapatat-2021a@corhuila.edu.co`

## 1. User stories worked this week

| HU ID | Title | Status (todo/doing/done) | Evidence (PR or commit URL) |
|---|---|---|---|
| HU-OPT-061 | Document inter-service communication — REST, gRPC and messaging | done | [`inter-service-communication-rest-grpc-messaging Allan.md`](./inter-service-communication-rest-grpc-messaging%20Allan.md) |
| HU-OPT-062 | Produce visual summary of REST / gRPC / messaging | done | [`Inter-Service comunication-Rest,-gRPC-and Messaging.png`](./Inter-Service%20comunication-Rest,-gRPC-and%20Messaging.png) |
| HU-OPT-063 | Document versioned contracts and contract testing | done | [`Api Versioned Contracts and Contract Testing.md`](./Api%20Versioned%20Contracts%20and%20Contract%20Testing.md) |
| HU-OPT-064 | Produce visual summary of versioned contracts / contract testing | done | [`Api Versioned Contracts and Contract Testing.png`](./Api%20Versioned%20Contracts%20and%20Contract%20Testing.png) |

## 2. My individual contribution

> **Author:** Allan Zapata Torres / `AllanZapata23` (`azapatat-2021a@corhuila.edu.co`).  
> **Week 07 scope (Mon–Sun):** study notes + visuals prepared within **2026-09-14 … 2026-09-20**; target course-fork commit date **2026-09-20** (end of week).
> **Scope this week:** distributed-systems documentation + visuals — **not** new application feature code.

### Study notes and visuals

| Artifact | What it covers |
|----------|----------------|
| [`inter-service-communication-rest-grpc-messaging Allan.md`](./inter-service-communication-rest-grpc-messaging%20Allan.md) | Sync vs async; REST; gRPC; messaging; trade-offs and when to use each |
| [`Inter-Service comunication-Rest,-gRPC-and Messaging.png`](./Inter-Service%20comunication-Rest,-gRPC-and%20Messaging.png) | Visual summary of REST / gRPC / messaging |
| [`Api Versioned Contracts and Contract Testing.md`](./Api%20Versioned%20Contracts%20and%20Contract%20Testing.md) | Contract definition, versioning strategies, breaking vs compatible changes, contract testing |
| [`Api Versioned Contracts and Contract Testing.png`](./Api%20Versioned%20Contracts%20and%20Contract%20Testing.png) | Visual summary of versioned contracts / contract testing |

### Mapping to OptiView

| Topic | How I apply it |
|-------|----------------|
| REST | Default sync style for BC APIs behind `opti-api-gateway` |
| gRPC | Optional later for internal high-throughput paths |
| Messaging | Async collaboration (worker / outbox-style flows) |
| Contracts | Keep OpenAPI / events versioned so portals and APIs can evolve independently |

## 3. Blockers and risks

- Artifacts are local in the fork until committed/pushed to `main`.
- Contract docs in this folder are study notes; they do not by themselves change `opti-docs` OpenAPI files.

## 4. Plan for next week

- Document Agile & DevOps for distributed teams and story mapping / estimation / MVP commitment.
- Produce matching visual summaries.

## 5. Compliance self-check

- [x] Conventional Commits - `type(scope): summary`
- [ ] Per-environment HU branch + PR to that environment (hu-xxx-dev -> develop, ...)
- [x] Testable acceptance criteria
- [ ] Tests added/updated (unit / integration)
- [ ] DDD / hexagonal boundaries respected (domain has no I/O)
- [x] No secrets; config via environment variables

Notes: Week window is Mon–Sun (**2026-09-14 → 2026-09-20**). Course-fork HU-status commits on `main` should use author/committer dates inside that window. Week 07 is documentation + visuals only. Unchecked items apply when I land code on `opti-*`.

## 6. Evidence links

- [`inter-service-communication-rest-grpc-messaging Allan.md`](./inter-service-communication-rest-grpc-messaging%20Allan.md)
- [`Inter-Service comunication-Rest,-gRPC-and Messaging.png`](./Inter-Service%20comunication-Rest,-gRPC-and%20Messaging.png)
- [`Api Versioned Contracts and Contract Testing.md`](./Api%20Versioned%20Contracts%20and%20Contract%20Testing.md)
- [`Api Versioned Contracts and Contract Testing.png`](./Api%20Versioned%20Contracts%20and%20Contract%20Testing.png)
- Course fork: https://github.com/AllanZapata23/sistemas-distribuidos-2026-b-g2
