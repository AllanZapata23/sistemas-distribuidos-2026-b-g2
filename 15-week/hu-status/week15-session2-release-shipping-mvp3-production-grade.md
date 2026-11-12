# Week 15 — Session 2: Delivering MVP 3 — Releasing a Production-Grade Version

**Topic:** Releasing a production-grade version
**Course:** Sistemas Distribuidos (2026-B, G1)

## Introduction

Testing shows that the system *is able* to survive production (Session 1). This session is about **actually releasing it** — taking the third and last MVP and turning it into a **production-grade** version that real users rely on. Production-grade does not mean "it runs on my machine" or even "the demo went well"; it means the system is observable, operable, secure and recoverable, and there is a plan for the day something fails. The central idea: *shipping is the start of operating, not the finish line.*

## What "Production-Grade" Really Means

A production-grade release is one you can put in front of real users with confidence and keep running at 2 a.m. without its author around. It is defined by qualities the demo version never had to have:

- **Reliable** — it reaches its availability and latency targets and degrades gracefully when a dependency fails.
- **Observable** — its behavior is visible through metrics, logs and traces, instead of being guessed.
- **Operable** — someone other than the author can deploy, monitor and recover it by following written procedures.
- **Secure** — authentication, authorization, secret handling and dependency scanning are in place.
- **Recoverable** — a tested way back exists (rollback, backups, reversible migrations) if the release goes badly.

## From MVP 1 to MVP 2 to MVP 3

The three MVPs are not three sizes of the same product; each one raises the bar:

- **MVP 1 — "it works":** the core feature exists and runs. It validates the idea.
- **MVP 2 — "it works together":** the services are integrated, the main flows are finished and it passes basic testing. It validates the system.
- **MVP 3 — "it works in production":** it is reliable, observable, operable and secure, with a release and rollback plan. It validates that it is ready for real users.

Every step adds the engineering rigor that the earlier one intentionally left out. MVP 3 is where that debt gets paid.

## Production-Readiness Checklist

Before a release can be called production-grade, verify every item — this is the gate that separates "feature-complete" from "ready to ship":

- **CI/CD gates green** — build, tests (unit, integration, contract, E2E) and security scans all pass; `main` is releasable.
- **Observability in place** — metrics, structured logs and distributed tracing are connected, with dashboards and alerts.
- **Security complete** — authN/authZ are enforced, secrets live in a vault (not in code), dependencies are scanned and there are no critical findings.
- **Load tested** — the system reaches its latency and throughput targets at the expected peak, with spare capacity.
- **Config and secrets ready** — the environment configuration and secrets exist and are correct for production.
- **Database migrations planned** — written, tested and preferably reversible/backward-compatible.
- **Backups verified** — recent backups of data and critical state exist and a restore has been tested.
- **Rollback plan ready** — versioned artifacts and a clear rollback trigger agreed in advance.
- **Runbooks written** — how to deploy, how to roll back and what to do for the most likely failure modes.

## The Release Steps

Once the checklist is complete, the release follows a disciplined sequence:

1. **Tag the version** — produce an immutable, versioned release artifact (e.g. a signed container image tagged `v3.0.0`).
2. **Deploy without downtime** — use a safe strategy (blue-green, canary or rolling) so users are never disconnected. A **canary** sends a small portion of traffic to the new version first.
3. **Run smoke and health checks** — automated checks confirm the new version is running, healthy and responding correctly before it receives all the traffic.
4. **Watch the golden signals** — follow **latency, traffic, errors and saturation** closely during and immediately after the release.
5. **Decide: keep or roll back** — if the signals stay healthy, move the release to 100%; if they cross the trigger, **roll back right away** to the last good version. Rolling back is a normal, planned result, not a personal failure.

## Operability — What Happens After the Release

Production-grade means the system can be *operated*, not only delivered. Operability is what keeps it alive long after launch:

- **Runbooks** — step-by-step instructions for deploying, rolling back and dealing with common incidents.
- **On-call** — a clearly assigned owner can be reached when alerts fire, with an escalation path.
- **SLOs and error budgets** — agreed **Service Level Objectives** (e.g. 99.9% availability, p95 latency < 300 ms) and an **error budget** that determines when to slow down and stabilize rather than ship more features.
- **Dashboards and alerting** — alerts trigger on symptoms users notice (error rate, latency) instead of noise, so on-call is only woken for real problems.
- **Incident response and postmortems** — a way to detect, contain and learn from incidents without blame, so the same failure does not happen again.

## Definition of Done for a Production-Grade Release

A release is "done" only when all of these are true — not just when the feature works:

- Code reviewed, with all tests and security scans green.
- Observability (metrics, logs, traces), dashboards and alerts are live.
- Load tested with production-like traffic.
- Rollback plan and runbooks written and verified.
- SLOs defined and an owner/on-call assigned.
- Deployed with a zero-downtime strategy and confirmed healthy in production.

## Core Principle

**Shipping is the start of operating, not the finish line.** The release moment does not end the work — it starts the system's real life, during which it has to be monitored, understood, kept within its SLOs and recovered when it fails. A production-grade MVP 3 is one designed to be *operated*, not just one that was deployed successfully once.

## Key Takeaways

- **Production-grade** = reliable, observable, operable, secure and recoverable — much more than "it works."
- The **MVP 1 → 2 → 3** path raises the bar at each step; MVP 3 brings the production rigor.
- Pass a **production-readiness checklist** before shipping: CI/CD, observability, security, load, migrations, backups, rollback, runbooks.
- Release step by step: **tag → zero-downtime deploy → smoke/health checks → watch golden signals → keep or roll back**.
- **Operability** (runbooks, on-call, SLOs/error budgets, dashboards, postmortems) keeps the system alive after launch.
- A release is **done** only when it is observable, load-tested, recoverable and owned — not merely deployed.

## Infographic

See [`week15-session2-release-shipping-mvp3-production-grade.png`](./week15-session2-release-shipping-mvp3-production-grade.png) in this folder for a visual summary.

## OptiView mapping (Week 15)

**MVP 1** validated the idea; **MVP 2** (`v2.0.0` / `v2.1.0`) showed that the services work together under Git Flow + GHCR. For OptiView, **MVP 3** means paying off the production debt: CI/DoD gates green (Week 14), observability and SLOs live (Weeks 11–12), the security backlog sufficiently closed (Week 13 Redis/secrets), evidence from contract + E2E + load tests, and then **tag `v3.0.0` → promote aiming for zero downtime → smoke → watch the golden signals → keep or roll back** to the previous SemVer. Shipping is where operations begin — runbooks and on-call ownership are part of "done".
