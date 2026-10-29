# apps

- Each app: `app.yaml`, `domain/`, `application/`, `adapters/`, `proto/`, `specs/pages/`, `migrations/`, `i18n/`, optional `data/`, `seed/`, `demo/`.
- People, organisations, countries, currencies and units come from `ctx.platform`; do not create own tables for them.
- User-facing text is a message key, never a literal.
- Use only `ctx` from `@platform/sdk`. Never import another app's code; use imports declared in `app.yaml`.
- Tables live in the instance schema; no `instance_id` columns.
- Every permission used in code or specs must be declared in `app.yaml`.
- Skills: `new-app`, `new-use-case`, `write-page-spec`, `db-migration`. Docs: `docs/architecture/apps/`.
