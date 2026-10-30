# ADR 0024: Pluggable supporting modules

- **Status:** Accepted
- **Date:** 2026-10-30
- **Deciders:** Hansa
- **Amends:** ADR 0021 (contacts move to a module), ADR 0022 (the record timeline becomes a module)

## Context
Apps need shared capabilities such as contacts, a record timeline, notifications, email, files or search. Building all of them into the core up front makes the core large and slows the start. Hansa's direction (2026-10-30): build a framework that lets any capability be written once as a supporting module and plugged into an app when it needs it. Modules are not apps. Odoo works the same way: `mail` and `contacts` are supporting modules, not apps.

## Decision
Three kinds of package:

| Kind | Examples | Installed | Talks to others through |
| --- | --- | --- | --- |
| Core (kernel) | Identity, sessions, RBAC and record rules, audit, instances, jobs, SDK, UI shell, i18n, reference data | Always | Ports inside the core |
| Module | Contacts, record timeline; later notifications, email, files, search | Once per deployment, only when needed | A typed, serializable module API |
| App | CRM, invoice reminders | Per instance, many times | Versioned contracts (ADR 0004, cross-app data) |

- A module lives in `modules/<id>/` with a `module.yaml`. It has the same layering as an app. It owns its own Postgres schema `m_<id>` (when it has tables), migrations, permissions (`<module>:<resource>:<action>`), events and settings.
- A module exposes one typed API (its port). Apps call it with `ctx.module('<id>')`. The API must be serializable so sandboxed Studio apps can use it over RPC.
- A module may contribute RPC services, page specs, settings pages, jobs and namespaced catalog components (`contacts.ContactPicker`).
- A module may define providers, adapters plugged in by deployment config (for example SMTP or SES behind an email module).
- Dependencies are declared and versioned (SemVer): an app manifest lists `modules: { contacts: ^1 }`, and a module lists the modules it needs. A module may also declare a JSON Schema for per-app config, set in the app manifest.
- Build-time selection (ADR 0014) applies: the build includes enabled apps plus every module they require. Installing an app whose modules are missing is refused.
- Boundaries (dependency-cruiser): the core never imports modules. Apps and modules import only a module's `contract` package. A module touches app data only through core ports (for example `RecordAccess` for read checks under record rules).
- Module migrations run once per deployment at deploy, with the same transactional upgrade rule as instances (ADR 0023).
- The core keeps reference data (countries, currencies, units, languages, timezones, currency rates) because the kernel's formatting uses it.
- **Library modules:** a module may have no tables, no API and no `ctx`, and be only stateless code (money maths, a state machine, CSV parsing). Apps and modules import it directly. It is how code is shared between apps without copying; state still crosses apps only through contracts and module APIs.
- First modules: `contacts` (from ADR 0021) and `record-timeline` (from ADR 0022). Others are built only when a real app needs them.

## Consequences
- The core stays small. A capability is written once and reused by any app that needs it.
- Two boundaries to explain: module APIs (typed, in-process, trusted) and app contracts (versioned, isolated per instance).
- Module APIs are platform contracts: breaking changes follow SemVer, and apps pin a major version.
