---
summary: Delivery phases, current phase, and open questions.
read_when: Planning work or deciding what to build next.
---
# Roadmap

**Current phase:** 0 - docs and ADRs in the repository; phase 1 next.

1. **Foundation**: Dev Container, monorepo, toolchain, commitlint and release-please, CI, Docker Compose, Postgres, migrations, logging, `@platform/ui` skeleton with Storybook, i18n catalogs and `intl-messageformat` wiring, docs and agent files.
2. **Core identity**: users, invitations, sign-in, sessions, RBAC with instance scopes, record rules (RLS) and field permissions, audit writer and viewer, reference data and contacts. Shell, `AppShellLayout`, `AuthLayout`, members, roles and contacts pages.
3. **App SDK and jobs**: YAML manifest, SDK as RPC-ready contract, instances (schema per instance, settings, bindings), Graphile Worker, scheduler, checkpointed long jobs, runs page, secrets, page specs (with message keys) + json-render, descriptor registry and dynamic RPC dispatcher with the generic browser `invoke`, data files and transactional instance upgrades. `SearchBar`, `BoardPage`, `CalendarPage`. First real automation app.
4. **Cross-app data**: read views, commands, outbox events, event triggers, Integrations page, `security_invoker` views with record rules and field masking, `RecordTimeline` (comments, attachments, history). Second app consuming the first.
5. **Spec runtime and sandbox**: entities and workflow graphs from specs, isolate sandbox, egress proxy, app versions, review screen.
6. **App studio**: agent, tools, mock-adapter previews, quotas.
7. **Hardening**: API tokens, TOTP/passkeys, SSO, rate limits, OpenTelemetry, backups and restore drill, audit retention.

## Proposed (2026-10-30, developer-speed review, awaiting decision)
- ADR 0024 generators and CRUD kit (`gen:app` in phase 1, the rest in phase 3); ADR 0025 list query (phase 3); ADR 0026 platform services (phases 2 to 4, each when the first app needs it); ADR 0027 kits (phase 3); ADR 0028 app-scoped components (phase 3).
- Move import/export into phase 4; stamp SDK and spec versions on `app_versions` from phase 3.

## Later (agreed, not scheduled)
- SDK and contract compatibility policy (`sdk:` range in the manifest, support window per SDK major).
- Generic CSV/XLSX import and export from list views.

## Considered and declined (2026-10-29, Odoo comparison)
- One generated entity model as the single source for every app artefact.
- Extension points (fields on other apps' entities, view slots, command hooks).
- Platform sequences (`INV/2026/0001`); apps number their own records.

## Open questions
- Who approves studio apps before install: the requester or an admin?
- Will other developers hand-write apps?
- Rough job volume.
- GitHub repository name.
