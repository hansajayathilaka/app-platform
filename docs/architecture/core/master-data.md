---
summary: Shared master and reference data owned by the core - contacts, countries, currencies, units.
read_when: An app needs people, organisations, countries, currencies or units, or you change those tables.
related: [../apps/sdk.md, ../apps/cross-app-data.md, ../../decisions/0021-shared-master-data.md]
---
# Master and reference data

One copy for every app, in the `platform` schema (ADR 0021). Apps never keep their own customer or currency tables.

| Kind | Tables | Who changes it | App access |
| --- | --- | --- | --- |
| Reference | `countries`, `currencies`, `uom_categories`, `units`, `languages`, `timezones` | Platform releases (seeded from ISO lists) | `ctx.platform.reference` (read) |
| Master | `contacts`, `currency_rates` | Users with `platform:contacts:write` / admins, or a rates job | `ctx.platform.contacts` (read, commands) |

## Contacts
- `contacts(id, kind person|organization, name, display_name, emails jsonb, phones jsonb, addresses jsonb, tax_id, parent_id, tags, archived, created_at, updated_at)`. `parent_id`: the organisation a person works for.
- Commands: `CreateContact`, `UpdateContact`, `ArchiveContact`; audited like any change. Events: `platform.contact.created.v1`, `.updated.v1`, `.archived.v1`.
- Record rules (ADR 0018) apply; role assignments are platform-scoped.
- App-specific data about a contact lives in the app's own tables, keyed by `contact_id`.
- Built-in Contacts page (list, detail with `RecordTimeline`). UI: `ContactPicker`.

## Reference data
- Read-only to apps; codes are the ISO codes (`LK`, `USD`). Updates arrive as platform migrations.
- Money is stored as integer minor units plus currency code; units convert within a category by factor.

## Later
- Merging duplicate contacts, contact import.
