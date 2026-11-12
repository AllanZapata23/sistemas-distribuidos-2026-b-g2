# Week 15 — Session 1: Testing Beyond the Unit — Contract, E2E, Load and Chaos

**Topic:** Testing distributed systems
**Course:** Sistemas Distribuidos (2026-B, G1)

## Introduction

In a distributed system, the difficult bugs are not found inside a single service — they appear **between** services: in the network, in contracts two teams agreed on long ago, under heavy load, and in failures nobody anticipated. Passing unit tests shows that each piece works on its own; it does not show that the *system* works. This session reviews the testing layers a distributed system really needs — the **testing pyramid** adapted to many services, **contract tests**, **integration tests** with real dependencies, **end-to-end (E2E) tests**, **load testing** and **chaos engineering**. The central idea: *prove the distributed system survives production before users find the limits.*

## Why Unit Tests Fall Short

Unit tests are quick, cheap and necessary — but they mock everything around the unit under test. As a result, they still pass when:

- One service calls another with a payload the receiver no longer accepts.
- The network is slow, loses packets or times out.
- Two services are each correct on their own but disagree on a shared format or an ordering assumption.
- The system works fine for one user but falls apart with ten thousand.
- A dependency is unavailable and the caller has no fallback.

A green unit-test suite only shows the code is correct according to *your assumptions*. Distributed failures happen precisely when those assumptions are wrong. The **interactions** need testing, not only the components.

## The Testing Pyramid in a Distributed Context

The traditional testing pyramid still applies, but every layer gains a distributed meaning:

- **Unit tests (base, numerous, fast):** a single function or class in isolation, with dependencies mocked. They take milliseconds and run on every commit.
- **Integration / contract tests (middle):** a service tested against a real dependency (database, queue) or against the *contract* of a service it calls. They take seconds and run in CI.
- **End-to-end tests (top, few, slow):** a real user journey through several running services. They take minutes and run before a release.
- **Load and chaos (next to the top):** not about whether a single path is correct, but about how the system behaves under stress and failure.

Keep many fast tests at the base and only a few slow ones at the top. An "ice-cream cone" shape (mostly slow E2E tests and few unit tests) is slow, flaky and costly to maintain.

## Contract Tests

When service A calls service B, they share an implicit **contract**: the request format A sends and the response format B returns. If B modifies its response and A is not adjusted, the system breaks — even though B's own tests keep passing.

**Contract testing** (for example with **Pact**, in a *consumer-driven* style) makes this contract explicit and verifies it automatically:

- The **consumer** (A) states exactly what it sends and what it expects to receive.
- That expectation becomes a **contract** the **provider** (B) has to verify in its own pipeline.
- If B changes in a way that breaks the contract, **B's build fails** — before anything is deployed.

This detects integration breakages **without needing to run both services together**, which makes it fast and reliable. For teams that own different services, it is the single most valuable type of test.

## Integration Tests Using Real Dependencies

Contract tests verify the *shape* of an interaction; integration tests verify that a service truly works with a **real** dependency instead of a mock:

- A real database (often started in a container, for example with **Testcontainers**) to catch schema, query and transaction problems.
- A real message queue to confirm publishing/consuming behavior and serialization.
- A real cache, search engine or third-party sandbox.

These tests are slower than unit tests but expose an entire category of bugs — incorrect SQL, broken migrations, serialization mismatches — that mocks hide by design. Rule of thumb: mock what you don't own, and use the real thing for what you do.

## End-to-End (E2E) Tests

E2E tests run a **full user journey** through the entire running system — several services, the real network, real data stores — just as a user experiences it. For example: *register → log in → place an order → receive confirmation*, passing through every service in the chain.

E2E tests provide the strongest confidence that the system works as a whole, but they are:

- **Slow** (minutes), since real services and real I/O are involved.
- **Flaky**, since any service, any network hiccup or any timing problem can make them fail.
- **Costly** to maintain.

For that reason, keep them **few and limited to critical paths**. They are the last proof before a release, not the place to cover every edge case — those belong in contract and unit tests.

## Load Testing

Functional tests answer *"does it work?"*. Load testing answers *"does it keep working under realistic and peak traffic?"* It shows how the system reacts as concurrency increases — where latency grows, where throughput levels off and where it breaks.

Common tools: **k6**, **JMeter**, **Gatling**, **Locust**.

Types of load test:

- **Load test** — expected normal and peak traffic; confirm latency and throughput stay within targets (SLOs).
- **Stress test** — go beyond the expected peak to locate the breaking point and observe how it fails.
- **Soak / endurance test** — sustained load for hours to reveal memory leaks and resource exhaustion.
- **Spike test** — a sudden burst of traffic to check autoscaling and back-pressure.

Track the **golden signals** during the tests: latency, traffic, errors and saturation. The aim is to know your limits *before* real users run into them.

## Chaos Engineering

In a distributed system, failure is not the exception — at scale it is the normal state. Chaos engineering is the practice of **injecting failure on purpose** into a system (ideally in production or a production-like environment) to demonstrate that it is resilient:

- **Kill** a service instance and confirm that traffic is rerouted and the system stays up.
- **Add latency** or packet loss on a network path and check that timeouts, retries and fallbacks work correctly.
- **Exhaust** CPU, memory or disk on a node and verify graceful degradation.
- **Break** a dependency and confirm that circuit breakers trip and the blast radius stays contained.

The method: state a **hypothesis** ("if payment-service dies, checkout degrades gracefully and recovers"), run a small, controlled **experiment**, measure the result, and fix what you discover. Tools include **Chaos Monkey** / the Simian Army, **Gremlin** and **LitmusChaos**. Chaos engineering replaces "we believe it is resilient" with "we have evidence that it is."

## Core Principle

**Test the system, not just the code.** Every service passing its own unit tests says nothing about whether they work *together*, handle load or recover from failure. Confidence in a distributed system comes from testing the interactions, the contracts, the limits and the failure modes — on purpose, and before users do it for you.

## Key Takeaways

- **Unit tests are not enough** — distributed bugs live between services, under load and in failures.
- Respect the **testing pyramid**: many fast unit tests, fewer integration/contract tests and very few E2E tests.
- **Contract tests (Pact, consumer-driven)** detect integration breakages early, without running both services together.
- **Integration tests** rely on real dependencies (DB, queue) in containers to catch what mocks hide.
- **E2E tests** validate critical user journeys from end to end — keep them few and focused.
- **Load testing** (k6, JMeter, Gatling) discovers latency and throughput limits before users do.
- **Chaos engineering** injects failure deliberately to prove resilience and limit the blast radius.

## Infographic

See [`week15-session1-testing-at-scale-contract-e2e-load-chaos.png`](./week15-session1-testing-at-scale-contract-e2e-load-chaos.png) in this folder for a visual summary.

## OptiView mapping (Week 15)

OptiView's critical path is the **`place-order` saga** (gateway → customers / products / sales / workflow). Several repos already have unit/CI checks; **consumer-driven contract tests** between those hops and **one E2E smoke test** are the most valuable layers to add next. Load and chaos tests run against the hypothesized SLOs (Week 12 workshop) and the golden signals (otel/Prometheus/Grafana in `opti-infra`) are what turn "the demo works" into evidence that the system survives peak traffic and failure — before tagging a production-grade MVP 3.
