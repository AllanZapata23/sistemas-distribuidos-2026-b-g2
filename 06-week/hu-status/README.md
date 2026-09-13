<!-- HU-STATUS TEMPLATE - do NOT remove the <!-- ... --> markers or the table headers.
     Your weekly grade is read AUTOMATICALLY from this file:
       06-week/hu-status/README.md  (inside YOUR fork). English. -->

# Weekly Status - Week 06

<!-- CONFIG-START - must match your profile repo (username/username) CONFIG -->
- FULL_NAME: Allan Zapata Torres
- GITHUB_USER: AllanZapata23
- TEAM: The Illusionists
- SPRINT_GOAL: Document Docker Compose and container orchestration basics; document environment planning, configuration strategy and orchestration for OptiView after MVP 1; and produce visual summaries for both study topics.
<!-- CONFIG-END -->

> **Week window:** Monday **2026-09-07** → Sunday **2026-09-13** (each course week is Mon–Sun).
> **Week 06 deliverable — post-MVP 1 ops foundations.** Study notes and visuals on **Docker Compose / orchestration** and **environment + configuration + orchestration planning** for the evolutionary OptiView path.
> **Course fork:** https://github.com/AllanZapata23/sistemas-distribuidos-2026-b-g2
> **Author email:** `azapatat-2021a@corhuila.edu.co`

## 1. User stories worked this week

| HU ID | Title | Status (todo/doing/done) | Evidence (PR or commit URL) |
|---|---|---|---|
| HU-OPT-055 | Document Docker Compose and container orchestration basics | done | [`docker_compose_orchestration_basics_Allan.md`](./docker_compose_orchestration_basics_Allan.md) |
| HU-OPT-056 | Produce visual summary of Docker Compose / orchestration | done | [`Docker compose and orchestration basics.png`](./Docker%20compose%20and%20orchestration%20basics.png) |
| HU-OPT-057 | Document environment planning, configuration strategy and orchestration | done | [`Software Environment Planning,Configuration Strategy & Orchestration.md`](./Software%20Environment%20Planning,Configuration%20Strategy%20%26%20Orchestration.md) |
| HU-OPT-058 | Produce visual summary of environments / config / orchestration planning | done | [`Software Environment Planning,Configuration Strategy & Orchestration.png`](./Software%20Environment%20Planning,Configuration%20Strategy%20%26%20Orchestration.png) |

## 2. My individual contribution

> **Author:** Allan Zapata Torres / `AllanZapata23` (`azapatat-2021a@corhuila.edu.co`).  
> **Week 06 scope (Mon–Sun):** study notes + visuals prepared within **2026-09-07 … 2026-09-13**; target course-fork commit date **2026-09-13** (end of week).
> **Scope this week:** DevOps / platform study notes and visuals — **not** new application feature code.

### Study notes and visuals

| Artifact | What it covers |
|----------|----------------|
| [`docker_compose_orchestration_basics_Allan.md`](./docker_compose_orchestration_basics_Allan.md) | Multi-container apps, Compose file, networking, volumes, env vars, Compose commands, Compose vs Kubernetes, practices |
| [`Docker compose and orchestration basics.png`](./Docker%20compose%20and%20orchestration%20basics.png) | Visual summary of Docker Compose / orchestration |
| [`Software Environment Planning,Configuration Strategy & Orchestration.md`](./Software%20Environment%20Planning,Configuration%20Strategy%20%26%20Orchestration.md) | Environment planning, configuration strategy, orchestration concepts for cloud-native delivery |
| [`Software Environment Planning,Configuration Strategy & Orchestration.png`](./Software%20Environment%20Planning,Configuration%20Strategy%20%26%20Orchestration.png) | Visual summary of environments / config / orchestration |

### Mapping to OptiView (after MVP 1)

| Topic | How I apply it |
|-------|----------------|
| Compose | Local multi-service stack while BCs are extracted |
| Environments | Align promotion with Git Flow: `develop` → `qa` → `main` |
| Configuration | No secrets in repo; env vars / secret store per environment |
| Orchestration | Compose for local/dev; Kubernetes-class orchestration as a later production target |

## 3. Blockers and risks

- Study artifacts live in this fork folder; they still need a commit + push to `main` to count as remote evidence.
- Evolutionary `opti-*` service scaffolds are not part of my Week 06 deliverable (docs only).

## 4. Plan for next week

- Document inter-service communication (REST, gRPC, messaging) and versioned contracts / contract testing.
- Produce matching visual summaries.

## 5. Compliance self-check

- [x] Conventional Commits - `type(scope): summary`
- [ ] Per-environment HU branch + PR to that environment (hu-xxx-dev -> develop, ...)
- [x] Testable acceptance criteria
- [ ] Tests added/updated (unit / integration)
- [ ] DDD / hexagonal boundaries respected (domain has no I/O)
- [x] No secrets; config via environment variables

Notes: Week window is Mon–Sun (**2026-09-07 → 2026-09-13**). Course-fork HU-status commits on `main` should use author/committer dates inside that window. Week 06 is documentation + visuals only. Unchecked items apply when I start landing code on `opti-*` repos. No secrets in the study notes.

## 6. Evidence links

- [`docker_compose_orchestration_basics_Allan.md`](./docker_compose_orchestration_basics_Allan.md)
- [`Docker compose and orchestration basics.png`](./Docker%20compose%20and%20orchestration%20basics.png)
- [`Software Environment Planning,Configuration Strategy & Orchestration.md`](./Software%20Environment%20Planning,Configuration%20Strategy%20%26%20Orchestration.md)
- [`Software Environment Planning,Configuration Strategy & Orchestration.png`](./Software%20Environment%20Planning,Configuration%20Strategy%20%26%20Orchestration.png)
- Course fork: https://github.com/AllanZapata23/sistemas-distribuidos-2026-b-g2
