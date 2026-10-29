# apps

- Each app: `app.yaml`, `domain/`, `application/`, `adapters/`, `proto/`, `specs/pages/`, `migrations/`.
- Use only `ctx` from `@platform/sdk`. Never import another app's code; use imports declared in `app.yaml`.
- Tables live in the instance schema; no `instance_id` columns.
- Every permission used in code or specs must be declared in `app.yaml`.
- Skills: `new-app`, `new-use-case`, `write-page-spec`, `db-migration`. Docs: `docs/architecture/apps/`.
