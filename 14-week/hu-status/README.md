<!-- HU-STATUS TEMPLATE - do NOT remove the <!-- ... --> markers or the table headers.
     Your weekly grade is read AUTOMATICALLY from this file:
       14-week/hu-status/README.md  (inside YOUR fork). English. -->

# Weekly Status - Week 14

<!-- CONFIG-START - must match your profile repo (username/username) CONFIG -->
- FULL_NAME: Allan Zapata Torres
- GITHUB_USER: AllanZapata23
- TEAM: The Illusionists
- SPRINT_GOAL: Corte 3 — study CI/CD pipelines and quality gates, turn the Definition of Done into automated gates and branch protection, and map a delivery pipeline for OptiView.
<!-- CONFIG-END -->

> Week window (Mon–Sun): **2026-11-02 → 2026-11-08** · Corte 3 continuation / certification track.

## 1. User stories worked this week
| HU ID | Title | Status (todo/doing/done) | Evidence (PR or commit URL) |
|---|---|---|---|
| HU-C3-W14-S1 | Study note + infographic: CI/CD — pipelines and quality gates | done | [note](./cicd-pipelines-quality-gates.md) · [visual](./cicd-pipelines-quality-gates.png) |
| HU-C3-W14-S2 | Study note + infographic: Your pipeline, DoD-as-gates & branch protection | done | [note](./pipeline-dod-gates-branch-protection.md) · [visual](./pipeline-dod-gates-branch-protection.png) |

## 2. My individual contribution
- Authored a study note and infographic on **CI/CD** (CI vs. Continuous Delivery vs. Continuous Deployment, the pipeline stages Commit → Build → Test → Package → Deploy → Release, quality gates that make "done" machine-verifiable, trunk-based development with required PR checks, and the four DORA metrics).
- Authored a study note and infographic on **planning a delivery pipeline** (designing concrete build/test/scan/package/deploy stages, turning each Definition of Done item into an automated gate, protecting `main` with branch-protection rules and required status checks, and the open PR → checks → review → gates green → merge → deploy flow).
- Mapped both to **OptiView**: `opti-front`'s `ci.yml` (install → test → build on `develop`/`qa`/`main`) and `publish.yml` (images to GHCR) as the current CI + package steps, Git Flow promotion as the Continuous Delivery decision, a DoD-as-gates checklist per slice, and a backlog to make required checks uniform across the 19 `opti-*` repos with coverage/SAST as blocking gates and SemVer image digests instead of only `:latest`.
- **No personal product commits this cut.** I did not author new `opti-*` product commits in Corte 3. My Week 14 evidence is the study notes and infographics above (individual work), not product code.

## 3. Blockers and risks
- Required status checks are not uniform across all 19 `opti-*` repositories, so branch protection is not yet enforced the same way everywhere.
- Coverage and SAST are not wired as blocking gates yet, and GHCR still relies on `:latest` rather than pinned SemVer digests, which weakens the rollback story.

## 4. Plan for next week
- Week 15 (Corte 3 close): study testing at scale and the production-grade MVP 3 release, and take part in the team's Corte 3 presentation (sustentación).
- Draft a uniform required-checks template to propose for the `opti-*` repositories.

## 5. Compliance self-check
- [ ] Conventional Commits - `type(scope): summary`
- [ ] Per-environment HU branch + PR to that environment (hu-xxx-dev -> develop, ...)
- [ ] Testable acceptance criteria
- [ ] Tests added/updated (unit / integration)
- [ ] DDD / hexagonal boundaries respected (domain has no I/O)
- [x] No secrets; config via environment variables
> Note: the commit/PR/test items are left unchecked **on purpose** — I authored no product commits this cut, so they do not apply to this week's study deliverables. Honest status per course rules.

## 6. Evidence links
- Study note — CI/CD pipelines & quality gates: [`cicd-pipelines-quality-gates.md`](./cicd-pipelines-quality-gates.md)
- Infographic — CI/CD pipelines & quality gates: [`cicd-pipelines-quality-gates.png`](./cicd-pipelines-quality-gates.png)
- Study note — Pipeline, DoD-as-gates & branch protection: [`pipeline-dod-gates-branch-protection.md`](./pipeline-dod-gates-branch-protection.md)
- Infographic — Pipeline, DoD-as-gates & branch protection: [`pipeline-dod-gates-branch-protection.png`](./pipeline-dod-gates-branch-protection.png)
