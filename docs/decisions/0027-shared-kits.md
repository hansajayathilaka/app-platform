# ADR 0027: Shared stateless kits apps may import

- **Status:** Proposed
- **Date:** 2026-10-30
- **Deciders:** Hansa

## Context
Apps may import only other apps' published contracts. That protects data ownership but leaves no way to share code such as money arithmetic, a state machine or CSV parsing, so it gets copied.

## Decision
- `packages/kits/*`: stateless libraries any app may import. First kits: `state-machine` (statuses, transitions, permission per transition, history via audit), `money`, `numbering` (prefix/date patterns over an app's own counter table), `csv`, `dates`.
- A kit has no tables, no `ctx` and no I/O. Anything with state remains an app or a platform service, shared through contracts.
- dependency-cruiser allows `apps/* -> packages/kits/*` and forbids kits from importing apps, adapters or the SDK runtime.

## Consequences
- Common logic is written and tested once.
- Amends the rule in `app-model.md`: apps import other apps' contracts and `packages/kits/*`, nothing else.
