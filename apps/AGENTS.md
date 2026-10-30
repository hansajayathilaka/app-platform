# apps

- Each app: `app.yaml`, `domain/`, `application/`, `adapters/`, `proto/`, `specs/pages/`, `migrations/`, `i18n/`, optional `data/`, `seed/`, `demo/`, `ui/` (app-scoped components).
- People and organisations come from the `contacts` module, countries, currencies and units from `ctx.platform.reference`; do not create own tables for them.
- Need a shared capability? Use or write a module (`modules/`), declared under `modules:` in `app.yaml`.
- User-facing text is a message key, never a literal.
- Use only `ctx` from `@platform/sdk`. Never import another app's code; use imports declared in `app.yaml`.
- Tables live in the instance schema; no `instance_id` columns.
- Every permission used in code or specs must be declared in `app.yaml`.
- Skills: `new-app`, `new-use-case`, `write-page-spec`, `db-migration`. Docs: `docs/architecture/apps/`.
