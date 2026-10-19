# Week 11 — Session 1: Observability in Practice — Logs, Metrics and Traces

**Topic:** Observability in distributed systems
**Course:** Sistemas Distribuidos (2026-B, G1)

## Introduction

Once a single user action travels through five or more services, opening one service's log file stops being a useful way to debug. **Observability** is about instrumenting a system well enough that its outputs alone reveal what is going on inside it, so that in production we can quickly explain *what is happening and why*. This document walks through the essentials: the three pillars (logs, metrics and traces), correlation IDs, the golden signals, and the difference between observability and classic monitoring.

## Defining Observability

A system is observable when its internal state can be understood purely from the data it produces — its logs, metrics and traces. The real test is whether engineers can ask **new** questions about its behavior, including ones nobody planned for, without deploying extra code to expose more information.

This is important because distributed systems break in complicated and surprising ways. A single request can involve many services, and the actual cause of a failure is frequently located far away from the place where the symptom shows up. With good observability, a team can go from "something looks wrong" to "this is precisely where and why" in minutes rather than hours.

## The Three Pillars

### 1. Structured Logs

A log is a timestamped record of a specific event, such as "an order was created" or "a payment failed". **Structured logs** use a machine-readable format (typically JSON) with named fields — `timestamp`, `level`, `service`, `message`, `trace_id`, `user_id`, etc. — rather than free text.

- **Value:** named fields can be searched, filtered and aggregated, so tracking down a particular event or error becomes much quicker.
- **Example:** an entry like `{ "level": "ERROR", "service": "payment-service", "message": "Payment failed", "trace_id": "4f61a3b9", "user_id": "u-1234" }` immediately identifies the service, the user and the request involved.
- **Cost:** logs carry a lot of detail but become expensive to store at large volumes, which makes them better suited to investigations than to permanent dashboards.

### 2. Metrics

Metrics are numbers collected and aggregated over time. The usual kinds are **counters** (values that only increase, such as total requests), **gauges** (values that rise and fall, such as memory usage) and **histograms** (distributions, such as request latency).

- **Value:** they are inexpensive to keep and quick to query, which makes them the natural choice for dashboards and alerts.
- **Examples:** requests per second, the percentage of failed requests, and p95 latency (the time within which 95% of requests finish).
- **Limit:** a metric shows *that* something has changed, but it does not describe what happened to any individual request.

### 3. Distributed Tracing

A **trace** records the path of one request as it moves through all the services it touches. Every step is a **span** that has a start time and a duration, and the spans together build a timeline of the entire request.

- **Value:** tracing shows *where* the time goes and *where* a request breaks across service boundaries, which is something logs and metrics cannot reveal by themselves.
- **Example:** a checkout trace could show `Client → API Gateway → User Service → Order Service → Payment Service → Notification Service` along with how long each hop took, so the slow or failing step stands out right away.

## Correlation IDs

A **correlation ID** (also known as a request ID or trace ID) is a unique value generated when a request first arrives in the system and then forwarded to every service that handles it.

Since that same ID shows up in the logs, metrics and traces of every service, engineers can join together *all* the information related to one request. Without correlation IDs the three pillars remain isolated from each other; with them, the complete journey of a single request can be rebuilt from start to finish.

## The Golden Signals

Google's SRE practice popularized **four golden signals** as the main indicators of how healthy a service looks to its users:

- **Latency** — how long requests take to complete, tracking the latency of *failed* requests on its own.
- **Traffic** — how much demand reaches the system (for example, requests per second).
- **Errors** — the rate at which requests fail.
- **Saturation** — how close to "full" the system is (for example CPU, memory or queue depth).

Keeping an eye on these four signals provides a short, high-level view of whether users are getting good service.

## Monitoring Compared with Observability

- **Monitoring** looks for problems that are *already known*. The team chooses ahead of time what to measure and what to alert on, so monitoring answers "is this particular thing working?" — the known unknowns.
- **Observability** supports the investigation of problems nobody *expected*. It provides enough detailed data to ask fresh questions afterwards and answer "why is this happening?" — the unknown unknowns.

Both work together: monitoring warns you *that* something is broken, and observability helps you discover *why*.

## Key Takeaways

- Observability means understanding a system's internal state from the outputs it emits.
- The three pillars are **logs** (what happened), **metrics** (how much / how often) and **traces** (where the time went across services).
- **Correlation IDs** connect the three pillars for one request.
- The **golden signals** (latency, traffic, errors, saturation) summarize health as users experience it.
- Monitoring catches known problems; observability explains unexpected ones.

## Infographic

See [`observability-logs-metrics-tracing-vertical.png`](./observability-logs-metrics-tracing-vertical.png) in this folder for a visual summary.

## OptiView mapping (Week 11)

In OptiView MVP 2, `opti-infra` already brings up **otel-collector** (receiving OTLP), **Prometheus** (scraping) and **Grafana** (dashboards). Platform services are expected to pass along a shared **`X-Correlation-Id` / `traceId`** so that logs from the gateway and the domain APIs describe a single request story. Emitting telemetry with the full OpenTelemetry SDK from every `opti-*-api` is being added gradually: the collector is ready, and instrumentation is being rolled out one service at a time.
