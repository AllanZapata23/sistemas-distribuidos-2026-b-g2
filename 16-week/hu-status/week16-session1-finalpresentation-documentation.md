# Week 16 — Session 1: Final Presentation and Documentation

**Topic:** Presenting a distributed system and shipping the documentation that lets others run it
**Course:** Sistemas Distribuidos (2026-B, G1)

## Introduction

A distributed system is only as valuable as other people's ability to understand it, trust it and keep it running. This last programming course closes the way real projects close: the team **presents** the system to someone who did not build it, and **documents** it so that a different team could operate and extend it tomorrow. Both activities are engineering work, not decoration. A clear pitch earns trust and decisions; a complete documentation set turns a team's private knowledge into something that survives the team. The central idea of the session: *working software first, explanation second, and every claim backed by evidence.*

## Why the Presentation Is Engineering

Stakeholders (a client, a manager, an employer) do not read code. They decide whether a system deserves their money, their users or a job offer based on what they see and hear in a few minutes. That makes communication a technical skill with concrete criteria:

- **Clarity of the problem** — the audience must understand *what pain the system removes* before any architecture appears.
- **Proof over promises** — a live, running demo beats any number of slides that describe features.
- **Reasoned decisions** — the value of the architecture is in *why* it is shaped that way and *what was rejected*, not in the list of frameworks.
- **Honesty** — showing what failed and what is still missing makes everything else more credible.

## A Narrative That Lands

A strong presentation follows a fixed arc, where each step answers the question the previous one raises:

1. **Problem and goal** — who suffers, what they need, how success is measured.
2. **Architecture and ADRs** — one diagram and the three or four decisions that shaped it.
3. **Live demo** — a real flow end to end, and then a failure and its recovery.
4. **Metrics and resilience** — latency, error rate, recovery time: numbers, not adjectives.
5. **Lessons learned** — what hurt, what the team would do differently.
6. **Next steps** — the honest roadmap of what is still open.

The order matters: *context → decisions → proof → honesty → future.* Starting with technology loses the audience; starting with the problem keeps them.

## Telling the Architecture Story

The architecture part is not an inventory of tools. It is a short story of decisions, each with its trade-off:

- **Why these bounded contexts** — how the domain was cut and why each service owns its data.
- **Why synchronous here and asynchronous there** — which calls need an immediate answer and which side effects can happen later.
- **Why saga and outbox** — how consistency is kept across services without distributed transactions.
- **What was rejected** — the alternatives that lost, and why, pointing to the ADRs as the written evidence.

One clean diagram is enough. Stakeholders remember *why* a decision was made long after they forget which framework version was used.

## The Demo That Proves It

A convincing demo has two halves:

- **The happy path, live** — perform a real business operation in the running system and show that it persisted (in the UI, in the database or in the logs).
- **The failure, live** — stop a service or a dependency, show how the system behaves (degrades, retries, compensates), then bring it back and show the recovery.

Showing the failure is what separates a prototype from a distributed system. It proves that the resilience patterns studied during the course are actually wired in, not just described.

## The Documentation Set

The documentation must allow a new engineer to run, understand, operate and extend the system without calling the original team. The essential set:

| Document | Answers the question |
|---|---|
| **README** | What is it, why does it exist and how do I run it (ideally with one command)? |
| **Architecture + ADRs** | How is it shaped and why were these decisions taken? |
| **API docs (OpenAPI / proto)** | How do I talk to each service — as a machine-readable contract, not a chat message? |
| **Runbooks** | How do I deploy, roll back and respond to the most likely incidents? |
| **CHANGELOG + versions** | What changed in each release and which version is running? |
| **Data-flow diagrams** | How does a request or an event travel through the services? |

Each document has to be *current*. Outdated documentation is worse than none, because it is trusted and wrong.

## A Path You Will Face in Real Life

**The great system with the terrible pitch.** A team built a solid system, but the presentation is twenty slides of architecture, no running software and no ADRs; the stakeholder leaves unconvinced. The fix is to reorder, not to add slides: open with the problem and a live demo (place an order, show it persisted, stop a service, show it recover), then one architecture diagram with the key decisions, then metrics and lessons. Engineers who can build *and* communicate are the ones who get hired and promoted.

## Applying It to OptiView

Our team's project, **OptiView** (a system for optical stores: patients, frames and lenses, work orders, sales), already has most of the documentation set in the `opti-docs` repository, which makes it a good test of these ideas:

- **README and governance** — `opti-docs/README.md`, plus a README in each `opti-*` repository with run instructions.
- **Architecture and ADRs** — `05-architecture/overview.md` and six accepted ADRs, among them database-per-service (ADR-003), the orchestrated place-order saga (ADR-004), outbox-style side effects (ADR-005) and CQRS read models for sales reports (ADR-006).
- **API contracts** — OpenAPI files in `07-api/contracts/openapi/` and an `openapi/openapi.yaml` inside services such as `opti-sales-api` and `opti-customers-api`.
- **Operations** — `13-operations/incident-management.md` (severity levels, response playbook and post-mortem structure) and the rollback section of `05-architecture/deployment.md`.
- **Versions** — a `CHANGELOG.md` following Keep a Changelog, with the `3.0.0` entry for the MVP 3 release.

A natural demo for OptiView would be the place-order flow: create a sale, show it in the sales portal and in `opti-sales-db`, then stop one service during the saga and show that `opti-workflow` compensates or resumes it. The weakest part of the set is runbooks: the operational knowledge is spread between the incident playbook and the deployment document, instead of a short runbook per common failure.

## Common Mistakes

- **Listing technologies** instead of explaining decisions and trade-offs.
- **Slides instead of software** — no live, working demo.
- **Only the happy path** — no failure and no recovery shown.
- **No README or runbooks** — nobody outside the team can run the system.
- **Stale documents** — diagrams and contracts that no longer match the code.
- **Hiding what went wrong** — no honest lessons learned, which makes the rest less credible.

## Core Principle

**Show it working, explain why, prove it with evidence.** The presentation convinces people that the system deserves trust; the documentation makes sure that trust does not depend on the people who built it.

## Key Takeaways

- The presentation follows a fixed arc: problem → architecture and ADRs → live demo with a failure → metrics → lessons → next steps.
- Architecture is told as decisions with trade-offs, using one diagram and pointing to the ADRs.
- A good demo shows a real flow *and* a failure with its recovery.
- The documentation set is README, architecture + ADRs, OpenAPI contracts, runbooks, CHANGELOG and data-flow diagrams — all kept current.
- Honesty about lessons and open work is part of a professional presentation, not a weakness.
