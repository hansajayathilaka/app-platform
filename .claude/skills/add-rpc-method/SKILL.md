---
name: add-rpc-method
description: Add or change a Connect RPC method - proto contract, breaking-change check, codegen, thin handler. Use for any API change.
---
# Add an RPC method

Read first: `docs/architecture/api/connect-rpc.md`.

1. Edit the `.proto` (platform in `proto/`, app in `apps/<id>/proto/`). New fields get new numbers; never reuse.
2. Read-only? add `option idempotency_level = NO_SIDE_EFFECTS;`. Add protovalidate rules.
3. `pnpm buf:lint && pnpm buf:breaking`. A breaking change needs a `v2` package, not an edit.
4. `pnpm buf:generate`.
5. Handler in `packages/interfaces/rpc` (or the app's interface folder): map -> use case -> map. No logic.
6. Handler test with the in-memory Connect transport.
7. If a page spec calls it, run `pnpm spec:check`.
