# ADR 0012: Graphile Worker, checkpointed long jobs, no Temporal

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
Some jobs exceed 15 minutes. Temporal needs its own server and about 1 GB RAM.

## Decision
Graphile Worker behind JobQueue. Long jobs are checkpointed workflow steps that resume after a crash.

## Consequences
- No extra infrastructure; transactional enqueue.
- We own the checkpoint logic; revisit if workflows become complex.
