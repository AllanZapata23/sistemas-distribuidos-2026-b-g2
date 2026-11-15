# ADR-C3-02: CCE across two availability zones with CPU-based autoscaling

## Status

Proposed

## Context

All OptiView services run as containers on a single Docker host per environment (`05-architecture/deployment.md`), so the host is a single point of failure and its size must cover the peak load all the time. Our scalability requirement NFR-003 (`04-requirements/non-functional.md`) asks to scale horizontally when CPU passes 70%, to react in less than two minutes and to stay at a maximum of three instances per service. Sections 2 (Reliability and Cost) and 3 (Scalability) of the design note depend on how the services are run.

## Decision

The services (`opti-api-gateway`, the four domain APIs, `opti-workflow` and `opti-worker`) run as workloads in one CCE cluster with node pools in AZ1 and AZ2. Each API has at least 2 pods, spread across both zones, and a Horizontal Pod Autoscaler adds pods when average CPU is above 70% for 2 minutes, up to 3 pods. The Cluster Autoscaler adds an ECS node when there is no room for new pods. Images come from SWR, and we keep the same Dockerfiles the team already has in each `deploy/` folder.

## Alternatives considered

- One big ECS with Docker Compose sized for the peak: rejected because it keeps the single point of failure and we pay for peak capacity even at night when the store is closed.
- Fixed number of ECS instances with manual scaling: rejected because a person has to notice the load and react, which cannot meet the 2-minute reaction time of NFR-003.

## Consequences

- Positive: losing one node or one availability zone leaves the other pods serving traffic through the ELB, and the cluster only grows when the load needs it.
- Negative: the team has to learn Kubernetes concepts (deployments, HPA, probes) and maintain manifests in addition to the existing Compose files.
