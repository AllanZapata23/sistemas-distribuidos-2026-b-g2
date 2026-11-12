<!-- HU-STATUS TEMPLATE - do NOT remove the <!-- ... --> markers or the table headers.
     Your weekly grade is read AUTOMATICALLY from this file:
       15-week/hu-status/README.md  (inside YOUR fork). English. -->

# Weekly Status - Week 15

<!-- CONFIG-START - must match your profile repo (username/username) CONFIG -->
- FULL_NAME: Allan Zapata Torres
- GITHUB_USER: AllanZapata23
- TEAM: The Illusionists
- SPRINT_GOAL: Corte 3 close — study testing at scale and the production-grade MVP 3 release, and take part in the team's Corte 3 presentation (sustentación).
<!-- CONFIG-END -->

> Week window (Mon–Sun): **2026-11-09 → 2026-11-15** · Corte 3 close / certification.

## 1. User stories worked this week
| HU ID | Title | Status (todo/doing/done) | Evidence (PR or commit URL) |
|---|---|---|---|
| HU-C3-W15-S1 | Study note + infographic: Testing at scale — contract, E2E, load & chaos | done | [note](./week15-session1-testing-at-scale-contract-e2e-load-chaos.md) · [visual](./week15-session1-testing-at-scale-contract-e2e-load-chaos.png) |
| HU-C3-W15-S2 | Study note + infographic: Releasing a production-grade MVP 3 | done | [note](./week15-session2-release-shipping-mvp3-production-grade.md) · [visual](./week15-session2-release-shipping-mvp3-production-grade.png) |
| HU-C3-W15-EXPO | Corte 3 team presentation (sustentación) — participation & Q&A | done | Classroom presentation on 2026-11-11 — see §2 (team sustentación; Q&A participation) |

## 2. My individual contribution
- Authored a study note and infographic on **testing distributed systems at scale** (the distributed testing pyramid, consumer-driven contract tests with Pact, integration tests with real dependencies via Testcontainers, E2E on critical paths, load testing with k6/JMeter, and chaos engineering).
- Authored a study note and infographic on **releasing a production-grade MVP 3** (what "production-grade" means, the production-readiness checklist, the tag → zero-downtime deploy → smoke → golden signals → keep/rollback sequence, and operability: runbooks, on-call, SLOs).
- Mapped both to **OptiView**: contract + one E2E smoke test around the `place-order` saga, load/chaos against the hypothesized SLOs and the `opti-infra` golden-signals stack as the gate before tagging a production-grade release.
- **Corte 3 presentation (sustentación) — 2026-11-11:** I took part in the team's Corte 3 presentation as a **developer** on The Illusionists. The presentation **opened with a video**, and the team then walked through the project. During **Q&A I answered questions from the professor and classmates focused on the project's architecture and deliverables (entregables).**
- **No personal product commits this cut.** I did not author new `opti-*` product commits in Corte 3. My evidence is the study notes and infographics above plus my participation in the team presentation and its Q&A — not product code.
- **Platform context (not my authorship):** the production-grade **MVP 3 release / tag (`v3.0.0`)** is still **pending team work** and was **not done by me**; it is listed here only as team-release context, not as a personal contribution.

## 3. Blockers and risks
- The MVP 3 production-grade release is not tagged yet; contract/E2E/load/chaos evidence for OptiView is still being assembled by the team.
- The recorded intro video shown at the start of the presentation is not in my possession, so it is not attached here.

## 4. Plan for next week
- Week 16 (course close): support the team in closing the remaining Corte 3 items.
- **Huawei certification: in progress** — I am starting the certification track now; it is **not complete**, so it is reported as a plan, not as evidence.

## 5. Compliance self-check
- [ ] Conventional Commits - `type(scope): summary`
- [ ] Per-environment HU branch + PR to that environment (hu-xxx-dev -> develop, ...)
- [ ] Testable acceptance criteria
- [ ] Tests added/updated (unit / integration)
- [ ] DDD / hexagonal boundaries respected (domain has no I/O)
- [x] No secrets; config via environment variables
> Note: the commit/PR/test items are left unchecked **on purpose** — I authored no product commits this cut, so they do not apply to this week's study deliverables and presentation participation. Honest status per course rules.

## 6. Evidence links
- Study note — Testing at scale: [`week15-session1-testing-at-scale-contract-e2e-load-chaos.md`](./week15-session1-testing-at-scale-contract-e2e-load-chaos.md)
- Infographic — Testing at scale: [`week15-session1-testing-at-scale-contract-e2e-load-chaos.png`](./week15-session1-testing-at-scale-contract-e2e-load-chaos.png)
- Study note — Production-grade MVP 3 release: [`week15-session2-release-shipping-mvp3-production-grade.md`](./week15-session2-release-shipping-mvp3-production-grade.md)
- Infographic — Production-grade MVP 3 release: [`week15-session2-release-shipping-mvp3-production-grade.png`](./week15-session2-release-shipping-mvp3-production-grade.png)
- Corte 3 team presentation (sustentación): in-class on **2026-11-11** — started with a video; I participated in the Q&A on architecture and deliverables (no personal file artifact).
