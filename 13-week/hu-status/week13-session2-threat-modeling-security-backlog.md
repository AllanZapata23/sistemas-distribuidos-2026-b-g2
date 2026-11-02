# Week 13 — Session 2: Finding Threats Early — Threat Modeling and the Security Backlog

**Topic:** Planning security into the system
**Course:** Sistemas Distribuidos (2026-B, G1)

## Introduction

Understanding security mechanisms (Session 1) is only part of the job; a team also has to actively **search for the threats** facing its own system and plan how to deal with them. This session presents **threat modeling** — an organized way to discover and rate threats while the system is being designed — and the **security backlog**, where the selected mitigations turn into concrete work that can be tracked. The central idea: *find the threats on paper before attackers find them in production.*

## What Threat Modeling Is

Threat modeling is an **organized process for discovering, rating and handling security threats early** — at design time, before any code exists. Correcting a design flaw while it is still on paper is much cheaper and safer than correcting it once it has been released.

## The Four Guiding Questions

A solid threat model is built around four simple questions:

1. **What are we building?** — understand the system, the data it handles and its goals.
2. **What can go wrong?** — find and analyze possible threats.
3. **What are we going to do about it?** — select the mitigations.
4. **Did we do a good job?** — check that the risks were brought down to an acceptable level.

## Mapping the System

Sketch a **data-flow diagram (DFD)** that includes:

- **External entities** (users, third-party systems),
- **Processes** (your services),
- **Data stores** (databases, queues), and
- **Trust boundaries** — the points where data moves from a less-trusted zone into a more-trusted one.

Most threats show up at trust boundaries, since that is where untrusted input enters a more privileged part of the system.

## STRIDE: Categories of Threats

**STRIDE** works as a checklist of the threat types to search for:

- **Spoofing** — impersonating someone else. *e.g. an attacker accesses an API with stolen credentials.*
- **Tampering** — changing data without permission. *e.g. modifying a message while it travels between services.*
- **Repudiation** — denying that an action took place. *e.g. a user denies making a sensitive change and no audit logs exist.*
- **Information disclosure** — exposing sensitive data. *e.g. a badly configured store leaks records.*
- **Denial of service** — making the system unavailable. *e.g. flooding the API until its resources run out.*
- **Elevation of privilege** — obtaining more permissions than intended. *e.g. a regular user ends up with admin access.*

## Rating and Prioritizing

Threats are not all equally urgent. Estimate **risk = likelihood × impact** with a method like **DREAD** (Damage, Reproducibility, Exploitability, Affected users, Discoverability) or a basic **High / Medium / Low** matrix. The rating determines what gets fixed first, instead of attempting to fix everything at the same time.

## Responding to Threats

For every serious threat, pick one of these responses:

- **Mitigate** — introduce a control that lowers the likelihood or the impact (the most frequent choice).
- **Eliminate** — drop the risky feature completely.
- **Transfer** — hand the risk to a third party (e.g. a managed payment provider, insurance).
- **Accept** — consciously accept a low risk that fits within the risk appetite, and keep monitoring it.

## The Security Backlog

The selected mitigations become **prioritized work items that can be tracked** — user stories or tickets — and they go into the sprint **next to feature work**. Each one includes:

- a clear **priority** (taken from the risk rating),
- an **owner**, and
- **acceptance criteria** that state when the mitigation is really finished.

This prevents security from being left for the end: it competes for time like any other work and can be verified.

## Key Takeaways

- Threat modeling discovers and rates threats **at design time** by answering four guiding questions.
- Map the system with a **data-flow diagram** and highlight the **trust boundaries**.
- Use **STRIDE** to list threat types and **risk = likelihood × impact** to set priorities.
- Respond by choosing to **mitigate / eliminate / transfer / accept**.
- Track the chosen mitigations in a **security backlog** with priority, owner and acceptance criteria.

## Infographic

See [`week13-session2-threat-modeling-security-backlog.png`](./week13-session2-threat-modeling-security-backlog.png) in this folder for a visual summary.

## OptiView mapping (Week 13)

An initial OptiView DFD places trust boundaries at **browser → front/gateway**, **gateway → domain APIs**, **workflow → customers/products/sales**, and **each BC → its own database**. STRIDE examples include forged JWTs, altered saga payloads, missing audit records (repudiation), secrets written to logs (disclosure), a flood against the gateway (DoS) and a seller escalating to admin. The top backlog candidates after MVP 2 are: a **shared secret store** for qa/main, rate limiting at the gateway, never logging Bearer tokens, and stricter service-to-service tokens — each one should become a board item with an owner and acceptance criteria.
