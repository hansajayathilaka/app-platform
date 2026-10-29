---
summary: Test strategy - pyramid, contract tests per port, fixtures, what each layer tests.
read_when: Writing or fixing tests.
---
# Testing

| Layer | Test | Tooling |
| --- | --- | --- |
| Domain | Pure unit tests, no mocks needed | Vitest |
| Use cases | Unit tests with in-memory adapters | Vitest |
| Adapters | **Port contract suite** (`packages/core-application/test/contracts/<Port>.contract.ts`) run against each adapter; Postgres via Testcontainers | Vitest + Testcontainers |
| RPC handlers | Thin mapping tests | Vitest + Connect in-memory transport |
| Page specs | `pnpm spec:check` over every spec | Ajv |
| End to end | Critical flows: sign in, install instance, run job, approve studio app | Playwright |

Rules
- Write the use case test first. Every new adapter must pass its port's contract suite.
- In-memory adapters live beside the ports and are reused by studio previews.
- No sleeping in tests: inject `Clock`.
- Never skip or disable a test to get green.
