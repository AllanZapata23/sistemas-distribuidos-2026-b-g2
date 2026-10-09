# Week 10 — Persistence in Distributed Systems

**Topic:** Persistence in distributed systems — Saga, Outbox and CQRS  
**Course:** Sistemas Distribuidos (2026-B, G2)  
**Infographic:** [`Persistence in Distributed Systems.png`](./Persistence%20in%20Distributed%20Systems.png)

## Overview

Once each service owns its own data, the classic single-database transaction disappears. This note covers five ideas for keeping data reliable across independent services under *eventual consistency*. The companion PNG summarizes the same five points in the same order.

## 1. Database-per-Service

**What it is:** Each microservice owns and manages its own private database. Other services cannot read or write that database directly; they must go through the service's API.

**Why it matters:** It keeps services independent and loosely coupled, so a change in one service's data model does not break the others.

**Example:** In an online store, the Order Service, Inventory Service and Customer Service each have their own database.

**In OptiView:** `opti-auth-db` and `opti-sales-db` are separate schemas/repos. Auth stores goals and notifications; sales stores work orders and payments. Cross-BC reads go through APIs / the gateway, not shared tables.

## 2. Saga Pattern

**What it is:** A way to keep data consistent across services using a chain of local transactions. If one step fails, compensating actions undo the previous steps.

**Why it matters:** There is no global transaction across services, so sagas provide consistency without locking everything together.

**Example:** Placing an order: reserve stock → charge payment → confirm order. If the payment fails, a compensating action releases the reserved stock.

**In OptiView:** `opti-workflow` orchestrates the place-order saga and carries the caller's identity so seller attribution survives the multi-step flow.

## 3. Outbox Pattern

**What it is:** A technique to publish events reliably. The event is written to an outbox table inside the same local transaction as the business data, and a separate process later delivers it to the message broker.

**Why it matters:** It prevents the common bug where the database is updated but the event is lost (or the event is sent but the data was not saved).

**Example:** When an order is saved, an `OrderCreated` event is stored in the outbox in the same transaction, then delivered to other services.

**In OptiView:** `opti-worker` consumes events and can react asynchronously (for example, notifying a seller when they reach a sales goal) without coupling the write path of sales/auth to every side effect.

## 4. CQRS (Command Query Responsibility Segregation)

**What it is:** Separating the write model (commands) from the read model (queries). Writes and reads use different, optimized models.

**Why it matters:** Reads can be served from a fast, denormalized store, improving performance and scalability.

**Example:** Orders are written to a normalized Write DB, while a separate Read DB is kept updated for fast dashboards and queries.

**In OptiView:** Sales revenue and seller-sales reports are dedicated read paths on `opti-sales-api`, exposed through the gateway, instead of forcing every dashboard query through the same transactional write model.

## 5. Eventual Consistency

**What it is:** Data across services becomes consistent over time, not instantly.

**Why it matters:** In distributed systems you trade immediate consistency for higher availability and scalability.

**Example:** After an order is placed, the reporting service may show it a few seconds later, once the event has propagated.

**In OptiView:** A notification or report may appear shortly after a sale or goal update, once the worker / read path has processed the event — not in the same locked transaction as the original write.

## Infographic

[`Persistence in Distributed Systems.png`](./Persistence%20in%20Distributed%20Systems.png) — visual summary of sections **1–5** above (database-per-service, saga, outbox, CQRS, eventual consistency).