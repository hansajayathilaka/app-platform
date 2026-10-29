# ADR 0007: Hexagonal architecture without an application framework

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
Hansa wants DRY, SOLID, clear separation of domain, services, data and interfaces, and the ability to swap DB, auth or interface by changing only adapters.

## Decision
Ports and adapters. Domain and use cases have no infrastructure imports. Constructor injection with a hand-written composition root. dependency-cruiser enforces the rule. Contract tests per port.

## Consequences
- Swaps are local; tests are fast with in-memory adapters; studio previews use mock adapters.
- More files and interfaces than a framework app; ports only where a swap is plausible.
