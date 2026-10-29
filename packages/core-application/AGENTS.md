# core-application

- Use cases: one class per command or query, `execute(input)`, dependencies via constructor (ports only).
- Every state-changing use case: `authorize` first, then work inside a `UnitOfWork`, `audit` in the same unit.
- Ports live in `src/ports/` and speak domain types, never library types.
- Each port has a contract suite in `test/contracts/` and an in-memory adapter in `src/testing/`.
- No imports from `adapters/`, `interfaces/` or any framework.
- Skills: `new-use-case`, `new-port-adapter`.
