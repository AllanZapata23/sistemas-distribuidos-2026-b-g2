# Week 16 — Session 2: Course Close — Retrospective, Handover and the Path Forward

**Topic:** Closing a distributed-systems project like a professional
**Course:** Sistemas Distribuidos (2026-B, G1)

## Introduction

This is the last session of the course and of the programming track of the degree. The team has built and operated a real distributed system for a whole semester, and the work does not end with the last commit. A professional close has four parts: a **final retrospective** that turns experience into principles, a **handover** that lets the system outlive the team, a **portfolio** that makes the effort visible, and a **plan to keep growing**. The central idea: *a project is only really finished when someone else can continue it and you can explain what you learned from it.*

## 1. The Final Retrospective

A sprint retrospective looks at two weeks; the final retrospective looks at the whole semester. Its goal is not to list what went wrong, but to extract principles that will be useful in the next project and the first job.

Good questions for the final retro:

- **What made the system reliable?** Which practices actually paid off (contracts, CI gates, observability, sagas)?
- **Where was the pain?** Integration between services, data consistency, deployment and operations, team coordination.
- **What would we do from day one next time?** The decisions that would have saved the most time if taken earlier.

Two rules make it work:

- **Blameless** — the discussion is about the system and the process, never about who made a mistake. People hide problems when they fear blame, and hidden problems repeat.
- **Concrete** — every insight ends as a short principle ("define the API contract before writing the consumer", "never deploy on Friday without a rollback plan"), not as a vague feeling.

## 2. Handover: Beating the Bus Factor

The **bus factor** is the number of people who would have to disappear for a project to stop. A system that only one person understands has a bus factor of 1, and that is a risk for any organization. A professional handover raises it: any competent engineer should be able to take the system and keep it alive using only the documentation.

The handover is real when a stranger can do four things:

| Goal | What makes it possible |
|---|---|
| **Run it** | A README with one command (or a short script) to start the whole system locally |
| **Operate it** | Runbooks for deploy, rollback and the most likely incidents |
| **Extend it** | ADRs that explain the decisions and API contracts (OpenAPI/proto) that define the boundaries |
| **Understand it** | An architecture diagram and data-flow diagrams that match the current code |

A clean, current **CHANGELOG** completes the set, because it tells the next team what is running and how it got there.

## 3. Your Portfolio

This project is one of the strongest pieces of evidence a student can show an employer: it proves the ability to build and operate a distributed system with a team, not just to pass an exam. To make it count, it has to be curated:

- **A clear repository (or set of repositories)** with a strong README at the entry point.
- **The architecture diagram** and **the ADRs**, because they show the reasoning behind the system.
- **A short demo video** (two or three minutes) showing a real flow and a failure being handled.
- **A paragraph on your individual contribution**, supported by evidence. In this course that evidence already exists: the weekly `hu-status` history, the commits and pull requests, and the study notes.

A repository without a README or diagram makes the effort invisible; a curated one turns it into a conversation in an interview.

## 4. Where to Grow Next

The course gives a solid foundation; the field is much larger. Natural next steps:

- **Kubernetes and cloud-native operations** — running and scaling services in a real cluster.
- **Event streaming** — Kafka and event-driven architecture at scale.
- **Advanced observability** — SLO engineering, error budgets and alerting on user impact.
- **Data-intensive systems** — replication, partitioning and consistency in depth (the book *Designing Data-Intensive Applications* is the classic reference).
- **Platform engineering and SRE** — making operations a product for other teams.
- **Security engineering** — threat modeling, identity and supply-chain security.

Tools will change every few years. The mindset is what transfers to any stack: **design for failure, make quality mechanical, decide with evidence.**

## 5. What You Leave With

At the end of the course, a student should be able to:

- Model a domain with **DDD** and shape services with **hexagonal architecture**.
- Make services communicate (**REST, gRPC, events**) and keep them consistent (**saga, outbox**).
- **Containerize** and deploy with **zero downtime**.
- **Observe, secure and test** the system under load and chaos.
- Work as an **agile team** with git-flow, contracts and **CI/CD**.

Together, that is the toolkit of a professional distributed-systems engineer.

## Applying It to OptiView

Looking at our project, **OptiView**, with the handover and portfolio lens:

- **Strengths for a handover** — the `opti-docs` repository already has the architecture overview, six ADRs (database-per-service, place-order saga, outbox, CQRS reports, among others), OpenAPI contracts in `07-api/contracts/openapi/`, an incident-management playbook with a post-mortem structure, and a `CHANGELOG.md` with the `3.0.0` MVP 3 release.
- **Gaps that keep the bus factor low** — the system is split into many repositories (`opti-*-api`, `opti-*-db`, `opti-*-portal`, gateway, workflow, worker, infra), so "run it with one command" depends on `opti-infra` and on knowing the order of the pieces. Operational knowledge is spread between the incident playbook and the deployment document instead of short runbooks per failure.
- **My own retrospective** — the clearest lesson for me is that documentation and evidence must be produced *while* working, not at the end: this semester I saw how much harder it is to reconstruct dates, decisions and proof afterwards than to record them weekly. My individual portfolio evidence for the course is my `hu-status` history with the study notes and infographics, and the Corte 3 certification work.

## Common Mistakes at the Close

- **Stale or missing handover documents** — the bus factor stays at 1.
- **Portfolio repositories with no README or diagram** — the effort is invisible.
- **Treating the retrospective as a formality** — no principles come out of it.
- **Stopping learning** — the field moves fast; keep the mindset and keep adding tools.

## Core Principle

**A system is finished when it can live without you, and a course is finished when you can explain what it taught you.** The retrospective captures the lessons, the handover protects the system, the portfolio shows the work, and the growth plan keeps the engineer moving.

## Key Takeaways

- The final retrospective is blameless and concrete, and produces principles to carry forward.
- Bus factor = 1 is a risk; a real handover lets a new engineer run, operate, extend and understand the system from the docs alone.
- A good portfolio combines repository, README, diagram, ADRs, demo video and evidence of individual contribution.
- Next areas to grow: Kubernetes/SRE, event streaming, data-intensive systems, observability and security.
- The lasting mindset: design for failure, make quality mechanical, decide with evidence.
