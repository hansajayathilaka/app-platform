# ADR 0003: PostgreSQL as the only database

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
NoSQL was considered for flexibility. The core is relational and relies on transactions spanning data, audit, outbox and queue.

## Decision
PostgreSQL only. jsonb columns where document flexibility helps.

## Consequences
- One database process on the VPS.
- Postgres-specific features sit behind ports so another store can be added for a specific workload later.
