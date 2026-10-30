---
summary: Reference data owned by the core - countries, currencies, currency rates, units, languages, timezones.
read_when: An app or module needs countries, currencies, money, units, languages or timezones.
related: [../apps/sdk.md, ../ui/i18n.md, ../../decisions/0021-shared-master-data.md, ../../decisions/0024-pluggable-supporting-modules.md]
---
# Reference data

Kept in the core (ADR 0024) because i18n formatting of money, units and dates uses it. People and organisations are the `contacts` module (`../modules/contacts.md`).

| Tables (schema `platform`) | Who changes it | Access |
| --- | --- | --- |
| `countries`, `currencies`, `uom_categories`, `units`, `languages`, `timezones` | Platform releases (seeded from ISO lists) | `ctx.platform.reference` (read) |
| `currency_rates` | Admins, or a rates job | `ctx.platform.reference` (read); admin page |

- Codes are ISO codes (`LK`, `USD`). Updates arrive as platform migrations.
- Money is stored as integer minor units plus currency code; units convert within a category by factor.
- UI: `CountrySelect`, `CurrencyInput`, `UnitInput`.
