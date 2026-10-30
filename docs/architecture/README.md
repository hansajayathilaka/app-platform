# Architecture index

| File | Covers | Read when |
| --- | --- | --- |
| `system-overview.md` | Components and how they connect | Any cross-cutting change |
| `code-architecture.md` | Layers, ports and adapters, dependency rule, folder layout, practices | Writing any code |
| `core/identity-and-auth.md` | Users, principals, login methods, API tokens | Login, signup, tokens |
| `core/sessions.md` | Session cookie, storage, rotation, CSRF | Session or cookie work |
| `core/permissions.md` | Permission strings, roles, instance-scoped grants, record rules, field permissions, `authorize` | Access control |
| `core/audit.md` | Audit table, same-transaction rule, retention | Anything that changes state |
| `core/reference-data.md` | Countries, currencies, rates, units, languages, timezones | Apps needing money, units or countries |
| `modules/README.md` | Supporting modules: module.yaml, `ctx.module`, schemas, providers, boundaries | Writing or using a module; core vs module vs app |
| `modules/contacts.md` | Contacts module: people and organisations | Apps needing people or orgs |
| `modules/record-timeline.md` | Record-timeline module: comments, attachments, history | Record timelines |
| `apps/app-model.md` | What an app is, build-time selection, lifecycle | Creating or changing an app |
| `apps/manifest.md` | Manifest fields and validation | Editing a manifest |
| `apps/instances.md` | Install twice, schema per instance, settings and integrations | Install/uninstall, migrations, bindings |
| `apps/sdk.md` | The `ctx` contract | Writing app code or changing the SDK |
| `apps/cross-app-data.md` | Read models, commands, events, versioning | Apps sharing data |
| `jobs/jobs-runtime.md` | Triggers, scheduler, guarantees, long jobs, run history, secrets | Jobs and automations |
| `ui/shell-and-layouts.md` | Shell, default layouts, components | UI structure |
| `ui/server-driven-ui.md` | `GetPage`, permission filtering, caching, renderer | How pages reach the browser |
| `ui/page-spec.md` | YAML spec format, validation pipeline, tooling | Writing or validating a page |
| `ui/ui-catalog.md` | Adding components to the catalog | Building UI components |
| `ui/i18n.md` | Message keys, catalogs, locale, formatting | Any user-facing text |
| `studio/app-studio.md` | Agent, outputs, pipeline, review | Studio features |
| `studio/sandbox.md` | Isolate runtime, capability tokens, egress | Running generated code |
| `api/connect-rpc.md` | Contracts, dynamic dispatch, streaming, webhooks, conventions | Any API change or calling app methods |
| `data/postgres.md` | Schemas, roles, migrations, jsonb | Database work |
