---
summary: Delivery phases, current phase, and open questions.
read_when: Planning work or deciding what to build next.
---
# Roadmap

**Current phase:** 0 - docs and ADRs in the repository; phase 1 next.

1. **Foundation**: Dev Container, monorepo, toolchain, commitlint and release-please, CI, Docker Compose, Postgres, migrations, logging, `@platform/ui` skeleton with Storybook, i18n catalogs and `intl-messageformat` wiring, docs and agent files.
2. **Core identity**: users, invitations, sign-in, sessions, RBAC with instance scopes, record rules (RLS) and field permissions, audit writer and viewer, reference data. Module framework (`module.yaml`, `ctx.module`, `m_<id>` schemas) and the `contacts` module. Shell, `AppShellLayout`, `AuthLayout`, members and roles pages.
3. **App SDK and jobs**: YAML manifest, SDK as RPC-ready contract, instances (schema per instance, settings, bindings), Graphile Worker, scheduler, checkpointed long jobs, runs page, secrets, page specs (with message keys) + json-render, descriptor registry and dynamic RPC dispatcher with the generic browser `invoke`, data files and transactional instance upgrades. `CrudTable`, `CrudForm`, `CrudDetail`, `SearchBar`, `BoardPage`, `CalendarPage`. First real automation app.
4. **Cross-app data**: read views, commands, outbox events, event triggers, Integrations page, `security_invoker` views with record rules and field masking, `record-timeline` module (comments, attachments, history). Second app consuming the first.
5. **Spec runtime and sandbox**: entities and workflow graphs from specs, isolate sandbox, egress proxy, app versions, review screen.
6. **App studio**: agent, tools, mock-adapter previews, quotas.
7. **Hardening**: API tokens, TOTP/passkeys, SSO, rate limits, OpenTelemetry, backups and restore drill, audit retention.

## Later (agreed, not scheduled)
- SDK and contract compatibility policy (`sdk:` range in the manifest, support window per SDK major).
- Generic CSV/XLSX import and export from list views.

## Considered and declined (2026-10-29, Odoo comparison)
- One generated entity model as the single source for every app artefact.
- Extension points (fields on other apps' entities, view slots, command hooks).
- Platform sequences (`INV/2026/0001`); apps number their own records.
- Code generators, a server-side CRUD kit and a required list API shape (2026-10-30); replaced by configurable CRUD UI components (ADR 0025).

## Under discussion (2026-10-30, one at a time)
From the developer-speed review, still open: building-block modules (notifications, email, files, connections, webhooks, search), shared stateless libraries, import/export timing, recording the SDK version on app versions.

## Open questions
- Who approves studio apps before install: the requester or an admin?
- Will other developers hand-write apps?
- Rough job volume.
- GitHub repository name.
