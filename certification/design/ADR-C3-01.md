# ADR-C3-01: Synchronous cross-AZ standby for OptiView databases

## Status

Proposed

## Context

ADR-003 (`05-architecture/decisions/records/ADR-003-database-per-service.md`) already decided that each bounded context (Auth, Customers, Products, Sales) owns its private database, and the team has one `-db` repository per context with its Flyway migrations. However, `05-architecture/deployment.md` shows that all of those databases still run as containers on the same single Docker host as the services, with one replica and no automatic failover. If that host or its disk fails, every context stops at once and the last confirmed sales can be lost.

Our requirement NFR-008 in `04-requirements/non-functional.md` sets an RTO below 5 minutes and an RPO below 1 second when the primary database fails, and an RTO below 15 minutes and an RPO below 5 minutes when a whole availability zone is lost. Section 4 of the design note identifies `opti-sales-db` as the most critical data store, because it holds quotations, work orders and payments, and section 2 (Reliability) shows that the single host is the main single point of failure of the system.

## Decision

Every `-db` repository of OptiView (`opti-auth-db`, `opti-customers-db`, `opti-products-db`, `opti-sales-db`) is deployed as its own RDS for PostgreSQL instance in primary/standby mode, with the primary in AZ1 and the standby in AZ2, using synchronous replication and automatic failover. This keeps the database-per-service rule of ADR-003 and moves it to managed instances. Automated daily backups are stored in OBS with a retention of 7 days, to recover from logical errors such as a wrong migration or a mistaken delete, which the standby would replicate immediately. The Flyway images of the `-db` repositories keep applying the migrations, now against the RDS endpoint, and the services keep connecting through the RDS address, so a failover does not require any change in the code or in the configuration.

## Alternatives considered

- Keep PostgreSQL in a container on the Docker host and copy daily dumps to OBS: rejected because a daily dump can lose up to 24 hours of sales and restoring it takes much longer than 5 minutes, so it misses both targets of NFR-008, and the host would remain a single point of failure.
- Use an asynchronous read replica in AZ2 and promote it by hand: rejected because asynchronous replication can lose the last transactions before the failure (RPO above zero), and a manual promotion depends on a team member being available, which cannot guarantee the 5-minute RTO.
- Put the four schemas in one shared RDS instance to save money: rejected because it goes against ADR-003, couples the migrations of the four contexts and makes one database failure take down all of them.

## Consequences

- Positive: a failure of a database server or of a whole availability zone does not lose any confirmed sale, recovery is automatic and inside the RTO, and the team no longer has to manage PostgreSQL patches and backups by hand.
- Negative: four instances with a standby each roughly double the database cost compared with a single primary, and synchronous replication adds a few milliseconds to every write; we accept this because sales data and availability matter more to the store than write latency.
- Negative: backups protect only one region; a full region disaster (NFR-008: RTO 4 hours, RPO 1 hour) still needs cross-region backup copies, which stay as future work.
