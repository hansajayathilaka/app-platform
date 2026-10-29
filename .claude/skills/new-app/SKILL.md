---
name: new-app
description: Scaffold a new hand-written platform app (manifest, layers, proto, page specs, migrations). Use when asked to create or start a new app.
---
# New app

Read first: `docs/architecture/apps/app-model.md`, `docs/architecture/apps/manifest.md`.

1. Confirm id (kebab-case), purpose, permissions, imports, jobs. Ask if unclear.
2. Run `pnpm gen:app <id>`; it creates `apps/<id>/` with `app.yaml`, `domain/`, `application/`, `adapters/`, `proto/<id>/v1/service.proto`, `specs/pages/`, `migrations/`.
3. Fill `app.yaml`: permissions, settings schema, imports/exports, jobs, `multiInstance`.
4. Domain first (entities + unit tests), then use cases (skill `new-use-case`).
5. Contract: methods in `service.proto` (skill `add-rpc-method`).
6. Tables: skill `db-migration` (they run per instance; no `instance_id`).
7. Pages: skill `write-page-spec`.
8. Enable the app in `deploy/apps.yaml`; `pnpm dev`; install an instance; add a Playwright test for the main flow.
9. `pnpm check`. Done only when it passes.
