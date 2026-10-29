# ADR 0020: Translatable text from the first spec

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
Labels live in manifests, page specs, settings schemas and catalog components, and the studio generates many of them. Retrofitting translation across stored, generated specs later is expensive.

## Decision
- User-facing text in specs and manifests is a message key: `title: { t: customers.title }`, with optional `args`. Plain strings are allowed in drafts; `spec:check` rejects them on publish.
- Each app ships `i18n/<locale>.yaml` catalogs in ICU MessageFormat. The platform and `@platform/ui` have their own catalogs.
- `GetPage` returns the spec plus the messages it uses for the user's locale (fallback: deployment default locale, then the key). The client formats with `intl-messageformat`.
- Dates, numbers and currency are formatted with `Intl` using the user's locale and timezone.
- Errors carry a code and a message key. Server-side text (emails, job output) uses `ctx.i18n.t`.
- Translating record data (a product name in two languages) is out of scope for now.

## Consequences
- Every spec is translatable from day one; English is just the first catalog.
- The studio agent emits keys and an `en` catalog; `spec:check` reports missing keys.
- Spec caches vary by locale.
