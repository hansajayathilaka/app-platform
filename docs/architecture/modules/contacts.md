---
summary: The contacts module - people and organisations shared by every app.
read_when: An app needs people or organisations, or you change the contacts module.
related: [README.md, ../core/reference-data.md, ../../decisions/0021-shared-master-data.md, ../../decisions/0024-pluggable-supporting-modules.md]
---
# Contacts module

One copy of people and organisations for every app (ADR 0021, as a module per ADR 0024). Apps never keep their own customer table.

- Schema `m_contacts`: `contacts(id, kind person|organization, name, display_name, emails jsonb, phones jsonb, addresses jsonb, tax_id, parent_id, tags, archived, created_at, updated_at)`. `parent_id`: the organisation a person works for.
- API (`ctx.module('contacts')`): `get`, `list(filter, sort, page)`, `create`, `update`, `archive`. Commands are authorized and audited.
- Events: `contacts.contact.created.v1`, `.updated.v1`, `.archived.v1`.
- Permissions: `contacts:contact:read|write`; record rules (ADR 0018) apply.
- App-specific data about a contact lives in the app's own tables, keyed by `contact_id`.
- Contributes: the Contacts page (list, detail with the record timeline when that module is present) and `contacts.ContactPicker`.
- Later: merging duplicates, import.
