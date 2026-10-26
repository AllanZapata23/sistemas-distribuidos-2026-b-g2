<!-- HU-STATUS TEMPLATE - do NOT remove the <!-- ... --> markers or the table headers.
     Your weekly grade is read AUTOMATICALLY from this file:
       12-week/hu-status/README.md  (inside YOUR fork). English. -->

# Weekly Status - Week 12

<!-- CONFIG-START - must match your profile repo (username/username) CONFIG -->
- FULL_NAME: Allan Zapata Torres
- GITHUB_USER: AllanZapata23
- TEAM: The Illusionists
- SPRINT_GOAL: Corte 3 — study cloud deployment models, zero-downtime release strategies, Infrastructure as Code and rollback planning, and map a safe release cut for OptiView.
<!-- CONFIG-END -->

> Week window (Mon–Sun): **2026-10-19 → 2026-10-25**.

## 1. User stories worked this week
| HU ID | Title | Status (todo/doing/done) | Evidence (PR or commit URL) |
|---|---|---|---|
| HU-C3-W12-S1 | Study note + infographic: Cloud deployment models & zero-downtime release strategies | done | [note](./week12-session1-cloud-deployment-models-strategies.md) · [visual](./cloud-deployment-models-strategies.png) |
| HU-C3-W12-S2 | Study note + infographic: Deployment plan, Infrastructure as Code & rollback | done | [note](./week12-session2-deployment-plan-iac-rollback.md) · [visual](./deployment-plan-iac-rollback.png) |

## 2. My individual contribution
- Authored a study note and infographic on **cloud deployment** (IaaS / PaaS / containers / serverless, registry → orchestrator → load balancer → health checks, and the rolling / blue-green / canary strategies plus horizontal autoscaling).
- Authored a study note and infographic on **planning a safe release** (deployment plan contents, declarative vs. imperative IaC, the pre-deployment checklist, and a rollback plan with versioned artifacts and a clear trigger).
- Mapped both to **OptiView**: delivery as containers (Compose via `opti-infra`, images on GHCR), Git Flow promotion (`develop → qa → release/* → main`), a written release cut (Flyway migrations → APIs → portals → front/gateway → verify → keep/rollback) and SemVer tags as the rollback artifacts.
- **No personal product commits this cut.** I did not author new `opti-*` product commits in Corte 3. My Week 12 evidence is the study notes and infographics above (individual work), not product code.

## 3. Blockers and risks
- OptiView currently relies on Compose + Git Flow promotion rather than two live environments; a full declarative cloud stack (Terraform/Pulumi) is deferred to a future ADR, so "IaC" is still IaC-lite.
- Reversible database migrations (`U*` scripts) are not yet complete for every service, which weakens the rollback story.

## 4. Plan for next week
- Study distributed-systems security (AuthN/AuthZ, Zero Trust) and threat modeling / STRIDE (Week 13).
- Draft a one-page OptiView release cut for the next MVP promotion to review with the team.

## 5. Compliance self-check
- [ ] Conventional Commits - `type(scope): summary`
- [ ] Per-environment HU branch + PR to that environment (hu-xxx-dev -> develop, ...)
- [ ] Testable acceptance criteria
- [ ] Tests added/updated (unit / integration)
- [ ] DDD / hexagonal boundaries respected (domain has no I/O)
- [x] No secrets; config via environment variables
> Note: the commit/PR/test items are left unchecked **on purpose** — I authored no product commits this cut, so they do not apply to this week's study deliverables. Honest status per course rules.

## 6. Evidence links
- Study note — Cloud deployment: [`week12-session1-cloud-deployment-models-strategies.md`](./week12-session1-cloud-deployment-models-strategies.md)
- Infographic — Cloud deployment: [`cloud-deployment-models-strategies.png`](./cloud-deployment-models-strategies.png)
- Study note — Deployment plan / IaC / rollback: [`week12-session2-deployment-plan-iac-rollback.md`](./week12-session2-deployment-plan-iac-rollback.md)
- Infographic — Deployment plan / IaC / rollback: [`deployment-plan-iac-rollback.png`](./deployment-plan-iac-rollback.png)
