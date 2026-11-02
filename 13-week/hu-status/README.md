<!-- HU-STATUS TEMPLATE - do NOT remove the <!-- ... --> markers or the table headers.
     Your weekly grade is read AUTOMATICALLY from this file:
       13-week/hu-status/README.md  (inside YOUR fork). English. -->

# Weekly Status - Week 13

<!-- CONFIG-START - must match your profile repo (username/username) CONFIG -->
- FULL_NAME: Allan Zapata Torres
- GITHUB_USER: AllanZapata23
- TEAM: The Illusionists
- SPRINT_GOAL: Corte 3 — study security for distributed systems (AuthN/AuthZ, Zero Trust) and threat modeling / STRIDE, and draft an initial OptiView security backlog.
<!-- CONFIG-END -->

> Week window (Mon–Sun): **2026-10-26 → 2026-11-01**.

## 1. User stories worked this week
| HU ID | Title | Status (todo/doing/done) | Evidence (PR or commit URL) |
|---|---|---|---|
| HU-C3-W13-S1 | Study note + infographic: Security — AuthN/AuthZ, tokens (JWT/OAuth2/OIDC), Zero Trust | done | [note](./week13-session1-security-authn-authz-zero-trust.md) · [visual](./week13-session1-security-authn-authz-zero-trust.png) |
| HU-C3-W13-S2 | Study note + infographic: Threat modeling, STRIDE & the security backlog | done | [note](./week13-session2-threat-modeling-security-backlog.md) · [visual](./week13-session2-threat-modeling-security-backlog.png) |

## 2. My individual contribution
- Authored a study note and infographic on **securing distributed systems** (AuthN vs. AuthZ, OAuth2 / OIDC / JWT, the API gateway as the central auth point, service-to-service trust with mTLS / short-lived tokens, and the Zero Trust mindset).
- Authored a study note and infographic on **threat modeling** (the four guiding questions, data-flow diagrams and trust boundaries, the STRIDE categories, risk = likelihood × impact, and turning mitigations into a tracked security backlog).
- Mapped both to **OptiView**: `opti-auth-api` (RS256 JWTs) with `opti-api-gateway` as the public choke point, `SERVICE_TOKEN` for worker/workflow calls, an initial DFD with trust boundaries (browser → gateway → domain APIs → per-BC databases) and the top security-backlog candidates (shared secret store, gateway rate limiting, never logging Bearer tokens, stricter service tokens).
- **No personal product commits this cut.** I did not author new `opti-*` product commits in Corte 3. My Week 13 evidence is the study notes and infographics above (individual work), not product code.

## 3. Blockers and risks
- OptiView still lacks a shared secret store for qa/main; secrets are handled via per-environment config today, which is a known security-backlog item.
- The initial threat model is a design-time draft; the mitigations are not yet board items with owners and acceptance criteria.

## 4. Plan for next week
- Week 14 (Continuación C3 / certificación): study CI/CD pipelines and quality gates; begin the certification track.
- Turn the top STRIDE mitigations into concrete backlog items to propose to the team.

## 5. Compliance self-check
- [ ] Conventional Commits - `type(scope): summary`
- [ ] Per-environment HU branch + PR to that environment (hu-xxx-dev -> develop, ...)
- [ ] Testable acceptance criteria
- [ ] Tests added/updated (unit / integration)
- [ ] DDD / hexagonal boundaries respected (domain has no I/O)
- [x] No secrets; config via environment variables
> Note: the commit/PR/test items are left unchecked **on purpose** — I authored no product commits this cut, so they do not apply to this week's study deliverables. Honest status per course rules.

## 6. Evidence links
- Study note — Security (AuthN/AuthZ/Zero Trust): [`week13-session1-security-authn-authz-zero-trust.md`](./week13-session1-security-authn-authz-zero-trust.md)
- Infographic — Security: [`week13-session1-security-authn-authz-zero-trust.png`](./week13-session1-security-authn-authz-zero-trust.png)
- Study note — Threat modeling / security backlog: [`week13-session2-threat-modeling-security-backlog.md`](./week13-session2-threat-modeling-security-backlog.md)
- Infographic — Threat modeling / security backlog: [`week13-session2-threat-modeling-security-backlog.png`](./week13-session2-threat-modeling-security-backlog.png)
