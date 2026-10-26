# Week 12 — Session 2: Planning a Release — Deployment Plan, Infrastructure as Code and Rollback

**Topic:** Planning a safe deployment
**Course:** Sistemas Distribuidos (2026-B, G1)

## Introduction

Understanding deployment *strategies* (Session 1) is only the starting point; a real release has to be **planned**. This session explains how to write a **deployment plan**, how to describe infrastructure with **Infrastructure as Code (IaC)** so environments can be recreated, and how to prepare a **rollback plan** so there is always a way back. The central idea: *plan the release, automate the infrastructure, and always keep a way back.*

## Why a Deployment Needs a Plan

In a distributed system, one release affects many services at the same time. Without a plan, steps get skipped, the order gets mixed up, and recovery has to be improvised. A **written deployment plan** lowers the risk and makes the result **predictable and repeatable** — any team member can follow it and reach the same outcome.

## What the Deployment Plan Contains

A solid plan defines, clearly and ahead of time:

- **Scope and version** — what is going to be deployed and under which version/tag.
- **Prerequisites** — the approvals, dependencies and resources that have to be ready beforehand.
- **Order of services, step by step** — the precise sequence in which services are released (e.g. database → backend → frontend).
- **Responsibilities** — who owns each step and who to contact if something goes wrong.
- **Timing / maintenance window** — when the release takes place and what impact is expected.
- **Health checks that confirm success** — the specific checks (metrics, logs, smoke tests) that prove the release worked.

## Infrastructure as Code (IaC)

**IaC** means describing servers, networks and services in **files kept under version control** rather than setting them up manually.

- **Tools:** Terraform, Ansible, CloudFormation, Pulumi.
- **Benefits:** environments become **reproducible** (an identical staging/prod can be created on demand), **reviewable** (changes pass through pull requests) and **automated** (no manual clicks that nobody can reproduce).
- Since the infrastructure is stored in Git, it gains history, review and rollback in exactly the same way as application code.

## Declarative and Imperative IaC

There are two ways to describe infrastructure:

- **Declarative** — you state the **desired final state** and the tool works out how to get there (e.g. Terraform: "there should be 3 servers"). It is idempotent and self-healing.
- **Imperative** — you write down the **exact steps** to execute, in sequence (e.g. a shell script: "create server, then configure, then start"). It gives more control, but you are responsible for the ordering and the edge cases.

Most modern IaC follows the declarative approach because it is safer and easier to repeat.

## Pre-Deployment Checklist

Before deploying, make sure everything is in place:

- **Tests are passing** — unit, integration and end-to-end tests are all green.
- **Configuration and secrets are ready** — the environment configuration and secrets exist for the target environment.
- **Database migrations are planned** — migrations are written, tested and preferably reversible.
- **Backups are done** — up-to-date backups of data and critical state exist and have been verified.
- **Monitoring is ready** — metrics, logs and alerts are in place so the effect of the release can be observed.

## Rollback Plan

Always specify **how to get back to the last working version quickly**:

- **Versioned artifacts** — keep earlier versions of images and releases so an older one can be redeployed right away.
- **Reversible migrations** — design database changes so they can be reverted (backward-compatible whenever possible).
- **A clear rollback trigger** — agree beforehand on the objective conditions (e.g. error rate, failing health checks, a business metric) that mean "stop and roll back — quickly and safely."

*If the release does not behave as expected, the system can return to a known good state quickly and safely.*

## The Safe Release Loop

A repeatable process for dependable releases:

1. **Plan** — set the scope, the order and the checks.
2. **Provision with IaC** — create or update the infrastructure from code.
3. **Deploy** — roll out the new version with a zero-downtime strategy.
4. **Verify** — run the health checks and watch the metrics.
5. **Decide: Keep or Rollback** — keep the release if it is healthy; roll back quickly if it is not.

The loop then starts again for the next release.

## Key Takeaways

- A **written deployment plan** makes releases predictable and repeatable.
- **IaC** makes infrastructure reproducible, reviewable and automated — and **declarative** is the preferred style.
- A **pre-deployment checklist** stops problems before they reach production.
- A **rollback plan** with versioned artifacts, reversible migrations and a clear trigger is mandatory.
- Follow the loop: **Plan → Provision → Deploy → Verify → Keep/Rollback**.

## Infographic

See [`week12-session2-deployment-plan-iac-rollback.png`](./week12-session2-deployment-plan-iac-rollback.png) in this folder for a visual summary.

## OptiView mapping (Week 12)

A written OptiView release cut looks like this: **scope/tag** (e.g. `release/2.1.0`) → **order** (run database migrations with Flyway → APIs → portals → front/gateway) → **one PR per environment** → **verify** (health + smoke tests + Grafana/Prometheus from Week 11) → **keep or rollback** (the previous SemVer tag; Flyway `U*` scripts where they exist). The Compose files and environment examples stored in Git act as our current **IaC-lite** for the local/platform topology; a more complete declarative cloud stack (Terraform/Pulumi) is left for a future ADR once the shared target is decided.
