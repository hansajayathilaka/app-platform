# ADR 0021: Shared master and reference data in the core

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
Nearly every app needs contacts (people and organisations), countries, currencies and units. If each app owns its own, data is duplicated and integrations need bindings for basics. Odoo's shared `res.partner`, currencies and units show the value of one copy.

## Decision
- The core owns, in the `platform` schema:
  - **Reference data** (seeded and updated by platform releases, read-only to apps): countries, currencies, units of measure with categories, languages, timezones.
  - **Master data** (managed by users): contacts (person or organisation, emails, phones, addresses, tax id, parent organisation, tags, archived) and currency rates.
- Apps use them through `ctx.platform.contacts` and `ctx.platform.reference` (reads and commands, same rules as cross-app contracts) and react to `platform.contact.*.v1` events.
- Apps keep app-specific data about a contact (credit terms, lead score) in their own tables keyed by contact id.
- Permissions: `platform:contacts:read|write`; record rules (ADR 0018) apply.
- A built-in Contacts page in the shell.

## Consequences
- One customer record across all apps; no binding needed for basics.
- The contact model is a platform contract: it changes with SemVer and versioned events like any other.
- Merging duplicates and contact import come later.
