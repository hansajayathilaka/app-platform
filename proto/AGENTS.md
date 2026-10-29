# proto

- buf-managed. Package per service and version: `platform.identity.v1`, `crm.v1`.
- Never reuse or renumber fields. Breaking change = new version package.
- Read-only methods: `option idempotency_level = NO_SIDE_EFFECTS;`
- Run `pnpm buf:lint && pnpm buf:breaking` before `pnpm buf:generate`.
