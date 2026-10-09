# Week 10 (Session 2) — Release: Shipping MVP 2 (Integrated System)

**Topic:** Release — shipping MVP 2 as one integrated system  
**Course:** Sistemas Distribuidos (2026-B, G2)  
**Infographic:** [`Realse Shipping MVP2.png`](./Realse%20Shipping%20MVP2.png)

## Overview

After the persistence patterns (Session 1), Session 2 is about **delivering the product**: packaging independent services into one integrated system and shipping **MVP 2** to users. The companion PNG follows the same five blocks as this note.

## 1. What is a Release?

**What it is:** A packaged, versioned build of the system that is delivered to users.

**Why it matters:** A clear release gives everyone a known, stable version to install, test and support.

**Example:** Tagging the system as `v1.0.0` and publishing it so users can run that exact version.

**In OptiView:** A release is not “one repo green”; it is a known cut of the `opti-*` services (auth, sales, gateway, front, worker, workflow, …) that work together behind the gateway.

## 2. MVP (Minimum Viable Product)

**What it is:** The smallest version of the product that delivers real value and can be tested with users.

**Why it matters:** It lets you learn from real feedback early, without building every feature first.

**Example:** MVP 2 lets a user place an order end to end, even if advanced features (discounts, deep analytics) are not ready yet.

**In OptiView:** MVP 1 lived in `opti-view`. MVP 2 is the evolutionary path: enough of the split BCs + shell + gateway to deliver a usable integrated flow, not every portal feature.

## 3. Integrated System

**What it is:** All the independent services working together as one single product.

**Why it matters:** Services that pass their own tests can still fail together; integration proves the whole flow works.

**Example:** The User Service, Order Service and Payment Service connect so a complete purchase works from start to finish.

**In OptiView:** Auth, sales, products (via gateway routes such as `/lenses`), front shell, worker and workflow must cooperate for login → sale → payment → async side effects.

## 4. Release Steps

1. **Integrate services** — connect the services so they work as one system.
2. **Run end-to-end tests** — check the full user flow across all services.
3. **Tag a version** — mark the exact code of this release (for example, `v1.0.0`).
4. **Deploy** — publish the release to the target environment.
5. **Verify** — confirm the deployed system behaves as expected.

**In OptiView:** Practice today is `feat/*` → PR → `develop`, then promote toward `qa` / `main`. A tagged MVP 2 cut across all 19 repos is still the next milestone.

## 5. Good Practices

- **Semantic versioning** — use clear version numbers like `MAJOR.MINOR.PATCH` (e.g. `v1.2.3`).
- **Release notes** — write down what changed, so users and teammates know what is new.
- **Rollback plan** — prepare a way to return to the previous version if the release fails.

**In OptiView:** Keep Conventional Commits and small PRs so a bad cut can be reverted per BC; document what landed in each weekly HU-status and in `opti-docs` when contracts change.

## Infographic

[`Realse Shipping MVP2.png`](./Realse%20Shipping%20MVP2.png) — visual summary of sections **1–5** above (release, MVP, integrated system, release steps, good practices).