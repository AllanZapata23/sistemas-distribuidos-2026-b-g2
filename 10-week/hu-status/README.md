<!-- HU-STATUS TEMPLATE - do NOT remove the <!-- ... --> markers or the table headers.
     Your weekly grade is read AUTOMATICALLY from this file:
       10-week/hu-status/README.md  (inside YOUR fork). English. -->

# Weekly Status - Week 10

<!-- CONFIG-START - must match your profile repo (username/username) CONFIG -->
- FULL_NAME: Allan Zapata Torres
- GITHUB_USER: AllanZapata23
- TEAM: The Illusionists
- SPRINT_GOAL: Land my `opti-front` dashboard slice (clearer charts and links to every product family) on the Week 09 shell and promote it `develop` → QA → `main`; write Week 10 study notes and visuals on persistence (saga, outbox, CQRS) and shipping MVP 2.
<!-- CONFIG-END -->

> **Week window:** Monday **2026-10-05** → Sunday **2026-10-11** (each course week is Mon–Sun).
> **Week 10 deliverable:** (1) study notes on **persistence** and **MVP 2 release**, (2) **this week's** vertical slice on the Week 09 shell, and (3) late-report only if Sunday **2026-10-04** work was missing from Week 09.
> **Course fork:** https://github.com/AllanZapata23/sistemas-distribuidos-2026-b-g2 (group **G2**).
> **Author email:** `azapatat-2021a@corhuila.edu.co`

## 1. User stories worked this week

### A. This week (Mon 2026-10-05 → Sun 2026-10-11)

| HU ID | Title | Status (todo/doing/done) | Evidence (PR or commit URL) |
|---|---|---|---|
| HU-OPT-090 | Document persistence in distributed systems (saga, outbox, CQRS) | done | [`persistence_in_distributed_systems.md`](./persistence_in_distributed_systems.md) |
| HU-OPT-091 | Produce visual summary of saga / outbox / CQRS | done | [`Persistence in Distributed Systems.png`](./Persistence%20in%20Distributed%20Systems.png) |
| HU-OPT-092 | Document release practices for shipping MVP 2 | done | [`release_shipping_mvp_2.md`](./release_shipping_mvp_2.md) |
| HU-OPT-093 | Produce visual summary of MVP 2 release | done | [`Realse Shipping MVP2.png`](./Realse%20Shipping%20MVP2.png) |
| HU-OPT-138 | Clarify dashboard charts and link all product families in `opti-front` | done | Author commit https://github.com/code-corhuila/opti-front/commit/9c251b839323504383f38bb17b8cb1d79a34e23e · included in https://github.com/code-corhuila/opti-front/pull/21 · promoted https://github.com/code-corhuila/opti-front/pull/22 · https://github.com/code-corhuila/opti-front/pull/23 |
| HU-OPT-139 | Co-author HU-14 dashboard (real metrics, charts, quick links) in `opti-front` | done | https://github.com/code-corhuila/opti-front/pull/13 · commit https://github.com/code-corhuila/opti-front/commit/0af50b828c8c8e6780a12aa4ad1388408a2b588b (`Co-authored-by: Allan Zapata <azapatat-2021a@corhuila.edu.co>`) |

### B. Late-reported from Week 09

> **Note:** No commits by `AllanZapata23` / `azapatat-2021a@corhuila.edu.co` dated **Sunday 2026-10-04** were found. Nothing to late-report.

| HU ID | Title | Status (todo/doing/done) | Evidence (PR or commit URL) |
|---|---|---|---|
| — | No late-reported Week 09 work | todo | — |

## 2. My individual contribution

> **Author:** Allan Zapata Torres / `AllanZapata23` (`azapatat-2021a@corhuila.edu.co`).  
> **Week 10 scope (Mon–Sun):** study notes + visuals + verified `opti-front` dashboard work dated **2026-10-06** (author timezone `-05:00`).  
> **Late-report:** none.

I did **not** open pull requests as `author:AllanZapata23`. My verified product evidence is: (1) a commit I authored on `opti-front`, and (2) a `Co-authored-by` trailer on the HU-14 dashboard commit. Teammates opened the merge/promotion PRs that carried my commit through Git Flow.

### Study notes and visuals (this week)

| Artifact | What it covers |
|----------|----------------|
| [`persistence_in_distributed_systems.md`](./persistence_in_distributed_systems.md) | Database-per-service, saga, outbox, CQRS, eventual consistency |
| [`Persistence in Distributed Systems.png`](./Persistence%20in%20Distributed%20Systems.png) | Visual summary of those five concepts |
| [`release_shipping_mvp_2.md`](./release_shipping_mvp_2.md) | Release, MVP, integrated system, release steps and good practices |
| [`Realse Shipping MVP2.png`](./Realse%20Shipping%20MVP2.png) | Visual summary of shipping MVP 2 |

### This week's vertical slice (on Week 09 shell)

| Pattern | How I applied it this week |
|---------|----------------------------|
| CQRS-style reads (UI) | Clarified ADMIN charts (sales timeseries, orders-by-status) and stock-by-category; linked product-family totals via `catalogTotal` / `lensesTotal` instead of frames-only inventory |
| Integrated release | My `fix(dashboard)` commit rode a feature branch → `develop` (#21), then promotion to QA (#22) and `main` (#23) |
| Database-per-service / saga / outbox | Covered in my study notes; **not** claimed as my API/DB code this week |

### Representative commits this week (author dates in the Mon–Sun window)

- **Tue 6 Oct (author `2026-10-06T19:55:02-05:00`):** [`9c251b8`](https://github.com/code-corhuila/opti-front/commit/9c251b839323504383f38bb17b8cb1d79a34e23e) `fix(dashboard): clarify charts and link all product families` — `src/layout/HomePage.tsx`, `src/layout/homeApi.ts`.
- **Tue 6 Oct (co-author):** [`0af50b8`](https://github.com/code-corhuila/opti-front/commit/0af50b828c8c8e6780a12aa4ad1388408a2b588b) `feat(hu-14): redesign the dashboard with real metrics, charts and quick links (#13)` — trailer `Co-authored-by: Allan Zapata <azapatat-2021a@corhuila.edu.co>`.
- **Promotion of my commit (cherry-picks keep author `AllanZapata23`):** https://github.com/code-corhuila/opti-front/pull/22 (QA) · https://github.com/code-corhuila/opti-front/pull/23 (`main`).

### Continuity

| Week | What I already have | How Week 10 uses it |
|------|---------------------|---------------------|
| 06–09 | Study notes + visuals in my fork (`06`…`09-week/hu-status`) | Continues the doc track; Week 10 adds persistence + release notes |
| 09 shell | Team `opti-front` Module Federation shell already existed | I only claim my dashboard UI slice on that shell |

### Ceremonies & flow (this week)

| Ceremony | When / evidence |
|----------|-----------------|
| Planning | Sprint goal: dashboard slice + Week 10 study notes/visuals |
| Daily | Async coordination; no review comments by me on the PRs |
| Review | Dashboard commit landed via #21 and was promoted #22 / #23 |
| Retro | Keep owning a clear UI slice; open at least one PR as `AllanZapata23` next time |
| Per-env path | `feat/*` → `develop` → `qa-*` → `release/*` → `main` for my commit: [#21](https://github.com/code-corhuila/opti-front/pull/21) · [#22](https://github.com/code-corhuila/opti-front/pull/22) · [#23](https://github.com/code-corhuila/opti-front/pull/23) |

## 3. Blockers and risks

- I have **zero PRs authored by me**; graders that require `author:AllanZapata23` on the PR must use the commit URL + PR inclusion evidence.
- My `9c251b8` commit has **no new unit/integration tests**.
- I must not report other teammates' Opti API/DB/tag work as mine.
- Study artifacts + HU-status still need commit/push on fork `main` if not remote yet.

## 4. Plan for next week

- Open at least one PR **as** `AllanZapata23`.
- Add a focused dashboard test (chart navigation / product-family totals).
- Own a Corte 3 slice end-to-end (e.g. notifications UX or a small worker/outbox change), not only shell UI polish.

## 5. Compliance self-check

- [x] Conventional Commits - `type(scope): summary`
- [x] Per-environment HU branch + PR to that environment (hu-xxx-dev -> develop, ...)
- [x] Testable acceptance criteria
- [ ] Tests added/updated (unit / integration)
- [x] DDD / hexagonal boundaries respected (domain has no I/O)
- [x] No secrets; config via environment variables

Notes: Week window is Mon–Sun. Section B empty (no 2026-10-04 work). Team uses `feat/*` by agreement; my commit was merged/promoted per environment even though I did not open those PRs. Tests stay `[ ]`. No secrets in the diff. Course-fork HU-status commits go on `main` with author/committer dates inside **2026-10-05 … 2026-10-11**.

## 6. Evidence links

**Course fork (Week 10 docs):**

- [`persistence_in_distributed_systems.md`](./persistence_in_distributed_systems.md)
- [`Persistence in Distributed Systems.png`](./Persistence%20in%20Distributed%20Systems.png)
- [`release_shipping_mvp_2.md`](./release_shipping_mvp_2.md)
- [`Realse Shipping MVP2.png`](./Realse%20Shipping%20MVP2.png)

**This week (Mon–Sun) — only verified Allan evidence:**

- https://github.com/code-corhuila/opti-front/commit/9c251b839323504383f38bb17b8cb1d79a34e23e
- https://github.com/code-corhuila/opti-front/commit/0af50b828c8c8e6780a12aa4ad1388408a2b588b
- https://github.com/code-corhuila/opti-front/pull/13
- https://github.com/code-corhuila/opti-front/pull/21
- https://github.com/code-corhuila/opti-front/pull/22
- https://github.com/code-corhuila/opti-front/pull/23

**Late-reported Week 09 (Sunday 4 Oct):** none.

**Related prior work:**

- Week 09: [`09-week/hu-status/README.md`](../../09-week/hu-status/README.md)
- Week 08: [`08-week/hu-status/README.md`](../../08-week/hu-status/README.md)
- Org repos: https://github.com/orgs/code-corhuila/repositories?q=opti
