# ADR-C3-01: Synchronous cross-AZ standby for OptiView databases

## Status

Proposed

## Context

Today every OptiView environment has one PostgreSQL instance on the same Docker host as the services (`05-architecture/deployment.md`). If that host or its disk fails, the store cannot sell and confirmed orders can be lost. Our own requirement NFR-008 (`04-requirements/non-functional.md`) asks for an RTO below 5 minutes and an RPO below 1 second when the primary database fails. Section 4 of the design note identifies `opti-sales-db` as the most critical data store, and section 2 (Reliability) shows that the single host is the main single point of failure.

## Decision

Every `-db` repository of OptiView (`opti-auth-db`, `opti-customers-db`, `opti-products-db`, `opti-sales-db`) is deployed as its own RDS for PostgreSQL instance in primary/standby mode, with the primary in AZ1 and the standby in AZ2, using synchronous replication and automatic failover. Daily automated backups are stored in OBS and kept for 7 days to recover from logical errors. The services keep connecting through the RDS address, so a failover needs no code change.

## Alternatives considered

- Keep PostgreSQL in a container on the Docker host with daily dumps to OBS: rejected because a daily dump can lose up to 24 hours of sales and restoring it takes much longer than 5 minutes, so it misses both NFR-008 targets.
- Asynchronous read replica in AZ2 promoted by hand: rejected because asynchronous replication can lose the last transactions (RPO above zero) and a manual promotion depends on someone being awake, which breaks the 5-minute RTO.

## Consequences

- Positive: a failure of a server or of a whole availability zone does not lose confirmed sales, and recovery is automatic, so the store keeps working without the team intervening.
- Negative: the standby doubles the database cost and synchronous replication adds a few milliseconds to every write, which we accept because sales data is more important than write latency.
