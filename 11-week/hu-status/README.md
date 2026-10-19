<!-- HU-STATUS TEMPLATE - do NOT remove the <!-- ... --> markers or the table headers.
     Your weekly grade is read AUTOMATICALLY from this file:
       11-week/hu-status/README.md  (inside YOUR fork). English. -->

# Weekly Status - Week 11

<!-- CONFIG-START - must match your profile repo (username/username) CONFIG -->
- FULL_NAME: Allan Zapata Torres
- GITHUB_USER: AllanZapata23
- TEAM: The Illusionists
- SPRINT_GOAL: Corte 3 kickoff — study observability (logs/metrics/traces) and reliability (SLIs/SLOs/error budgets) and map both to the OptiView platform.
<!-- CONFIG-END -->

> Week window (Mon–Sun): **2026-10-12 → 2026-10-18** · Corte 3 begins.

## 1. User stories worked this week
| HU ID | Title | Status (todo/doing/done) | Evidence (PR or commit URL) |
|---|---|---|---|
| HU-C3-W11-S1 | Study note + infographic: Observability — logs, metrics and traces | done | [note](./week11-session1-observability-logs-metrics-tracing.md) · [visual](./observability-logs-metrics-tracing-vertical.png) |
| HU-C3-W11-S2 | Study note + infographic: Reliability — SLIs, SLOs, error budgets, alerting | done | [note](./week11-session2-reliability-slis-slos.md) · [visual](./reliability-slis-slos-vertical.png) |

## 2. My individual contribution
- Authored a study note and a vertical infographic on **observability** (the three pillars — logs, metrics, traces — correlation IDs, the golden signals, and monitoring vs. observability).
- Authored a study note and a vertical infographic on **reliability engineering** (SLI / SLO / SLA, error budgets, burn-rate alerting and the reliability loop).
- Mapped both topics to **OptiView**: the existing `opti-infra` otel-collector / Prometheus / Grafana stack, the shared `X-Correlation-Id` / `traceId`, and candidate user-boundary SLIs (gateway success rate and p95 latency) that would define an error budget after the MVP 2 expo.
- **No personal product commits this cut.** I did not author new `opti-*` product commits in Corte 3. My Week 11 evidence is the study notes and infographics listed above (individual work), not product code.

## 3. Blockers and risks
- Full OpenTelemetry SDK instrumentation on every `opti-*-api` is still being rolled out service by service, so end-to-end traces are not complete yet.
- SLO targets for OptiView are not formally agreed by the team, so the error budget is still a proposal rather than a policy.

## 4. Plan for next week
- Study cloud deployment, release strategies, IaC and rollback (Week 12) and map a safe release cut for OptiView.
- Propose a first user-boundary SLI/SLO pair for the gateway to discuss with the team.

## 5. Compliance self-check
- [ ] Conventional Commits - `type(scope): summary`
- [ ] Per-environment HU branch + PR to that environment (hu-xxx-dev -> develop, ...)
- [ ] Testable acceptance criteria
- [ ] Tests added/updated (unit / integration)
- [ ] DDD / hexagonal boundaries respected (domain has no I/O)
- [x] No secrets; config via environment variables
> Note: the commit/PR/test items are left unchecked **on purpose** — I authored no product commits this cut, so they do not apply to this week's study deliverables. Honest status per course rules.

## 6. Evidence links
- Study note — Observability: [`week11-session1-observability-logs-metrics-tracing.md`](./week11-session1-observability-logs-metrics-tracing.md)
- Infographic — Observability: [`observability-logs-metrics-tracing-vertical.png`](./observability-logs-metrics-tracing-vertical.png)
- Study note — Reliability (SLI/SLO): [`week11-session2-reliability-slis-slos.md`](./week11-session2-reliability-slis-slos.md)
- Infographic — Reliability (SLI/SLO): [`reliability-slis-slos-vertical.png`](./reliability-slis-slos-vertical.png)
