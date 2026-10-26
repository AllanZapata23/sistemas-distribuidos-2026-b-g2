# Week 12 — Session 1: Deploying to the Cloud — Service Models, Release Strategies and Zero-Downtime

**Topic:** Cloud deployment of distributed systems
**Course:** Sistemas Distribuidos (2026-B, G1)

## Introduction

Packaging an application into containers is only half of the work: those containers also need infrastructure that puts them in front of real users, grows with demand, remains available, and allows new versions to go out without bringing the system down. This session looks at the **cloud service models** (IaaS, PaaS, containers, serverless), how **containers run in the cloud**, the **zero-downtime release strategies** (rolling, blue-green, canary) and **scaling** — all with the aim of making releases uneventful and easy to undo.

## Why Deployment to the Cloud Matters

A distributed system only delivers value when users can actually reach it. The cloud supplies infrastructure on demand and offers three things that a local server struggles to provide: **scalability** (absorbing more load by adding capacity), **availability** (continuing to run when parts fail) and **reversible releases** (shipping and rolling back safely). A good deployment process turns a risky, manual event into one that is automated and predictable.

## Cloud Service Models

Each model divides the work differently between what **you** manage and what the **provider** manages:

- **IaaS (Infrastructure as a Service):** you rent virtual machines, networks and storage, and you are responsible for the operating system and everything on top of it. You get the most control and the most responsibility. *Example: AWS EC2, Azure VMs.*
- **PaaS (Platform as a Service):** you upload your code and the platform takes care of the runtime, the operating system and scaling. Less control, but far less operational effort. *Example: Heroku, App Service.*
- **Containers / CaaS:** you build the application into an image once and run it anywhere, orchestrated by **Kubernetes** or a similar tool. It balances portability with control. *Example: GKE, EKS, AKS.*
- **Serverless / FaaS (Functions as a Service):** you deploy functions that execute on demand and **scale to zero** when nothing is calling them; the provider handles the rest. *Example: AWS Lambda, Azure Functions.*

Moving from IaaS toward serverless, you manage less and the provider takes on more.

## How Containers Run in the Cloud

The usual pipeline looks like this:

1. **Image stored in a registry** — the built container image is pushed to a registry (Docker Hub, ECR, GCR).
2. **Orchestrator schedules it** — Kubernetes (or a similar tool) places containers on healthy nodes of the cluster.
3. **Load balancer routes traffic** — incoming requests are spread across the running instances.
4. **Health checks keep instances alive** — liveness and readiness probes make sure only healthy containers get traffic; unhealthy ones are restarted or replaced.

## Zero-Downtime Release Strategies

The objective is to publish a new version **without disrupting users**:

- **Rolling update:** instances are replaced little by little — a few at a time — so the service never goes down. It is simple and very common, though the old and new versions coexist while the rollout is in progress.
- **Blue-Green:** two identical environments are kept. "Blue" handles production while "Green" carries the new version; after Green is validated, all traffic is switched over. Switching and rolling back are instant, but you pay for running two environments.
- **Canary:** a small fraction of traffic (e.g. 5%) goes to the new version first; metrics are observed and the share is increased step by step if everything looks healthy. This keeps the blast radius of a bad release small.

## Scaling

- **Horizontal scaling (scale out):** add more instances to divide the load. This is the preferred option for distributed systems because there is no single-machine limit.
- **Vertical scaling (scale up):** give one instance more CPU/memory. It is simple, but capped by the largest machine available.
- **Autoscaling:** instances are added or removed automatically according to load (for example CPU usage or request rate), so capacity follows demand without manual work.

## Core Principle

**Releases should be boring, reversible events.** Automate the deployment, watch the outcome, and always have a quick rollback ready. A deployment that can be reversed fast turns risk into a routine, low-stress task.

## Key Takeaways

- Cloud models exchange control for convenience: **IaaS → PaaS → Containers → Serverless**.
- Containers run through **registry → orchestrator → load balancer → health checks**.
- Zero-downtime strategies — **rolling**, **blue-green** and **canary** — each reduce risk in a different way.
- Scale **horizontally** by default and rely on **autoscaling** to follow demand.

## Infographic

See [`week12-session1-cloud-deployment-models-strategies.png`](./week12-session1-cloud-deployment-models-strategies.png) in this folder for a visual summary.

## OptiView mapping (Week 12)

OptiView is delivered as **containers** (run locally with Compose through `opti-infra`, with images published to **GHCR**). Environments are promoted following Git Flow (`develop` → `qa` → `release/*` → `main`) instead of maintaining two live clusters at the moment — this is our realistic route toward **boring, reversible** releases. The progressive-delivery vocabulary from Week 09 (rings / canary-minded cuts) shapes how much risk we take on with each promotion. SemVer tags (`v2.0.0`, `v2.1.0`) are the versioned artifacts we redeploy whenever a cut goes wrong.
