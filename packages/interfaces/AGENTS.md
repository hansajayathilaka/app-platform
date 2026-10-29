# interfaces (driving adapters)

- `rpc/`: Connect handlers. Thin: map request -> use case input, call, map result. No business logic.
- `webhooks/`: plain HTTP, verify signatures, then call a use case.
- `cli/`: admin commands calling the same use cases.
- Contracts come from `proto/` via `buf generate`; never hand-edit generated code.
- Skill: `add-rpc-method`. Docs: `docs/architecture/api/connect-rpc.md`.
