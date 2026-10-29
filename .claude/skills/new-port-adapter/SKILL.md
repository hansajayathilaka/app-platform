---
name: new-port-adapter
description: Add a new port (interface) or a new adapter implementing an existing port, with its contract test suite. Use when introducing or swapping infrastructure (DB, auth, queue, mail, LLM, storage).
---
# New port or adapter

Read first: `docs/guides/ports-and-adapters.md`.

New port:
1. `packages/core-application/src/ports/<Name>.ts` in domain terms, no library types.
2. Contract suite `test/contracts/<Name>.contract.ts` exporting `run<Name>Contract(makeAdapter)`.
3. In-memory adapter in `src/testing/` passing the suite.

New adapter:
1. `packages/adapters/<tech>-<purpose>/` implementing the port.
2. A test file calling the port's contract suite (Testcontainers for Postgres).
3. Wire in `services/server/src/composition-root.ts`.
4. Verify nothing outside the adapter and composition root changed. If something had to, the port leaks: fix the port instead.
5. `pnpm depcruise && pnpm check`.
