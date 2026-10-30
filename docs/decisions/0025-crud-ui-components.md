# ADR 0025: Configurable CRUD UI components

- **Status:** Accepted
- **Date:** 2026-10-30
- **Deciders:** Hansa

## Context
Building list, form and detail screens is the most repeated work in internal apps. Code generators and a server-side CRUD kit were considered and rejected (2026-10-30): Hansa wants the simplest thing that is easy to maintain, and no conventions that could corner real business logic.

## Decision
- `@platform/ui` gets advanced, configurable CRUD components: `CrudTable`, `CrudForm`, `CrudDetail`. A page uses one with parameters and gets a working screen.
- **No required API shape.** Components bind to any RPC method through mappings in the spec: which input fields receive search, filters, sort and paging, and where rows, total and next page sit in the response. Paging can be `server`, `client` (method returns everything) or `none`. A missing feature degrades gracefully, for example no total gives next/previous paging.
- **Customizable at every level:** props (columns, formats, filters, actions), slots that accept any catalog component, and custom cell or field components. When that is still not enough, the page composes `DataTable` and `Form` directly or adds a catalog component. Nothing blocks a screen.
- Shared types are optional helpers, never required: `platform.common.v1` offers `PageRequest`, `SortField`, `PageInfo` for apps that want them, and the SDK and UI offer TypeScript generics (`ListResult<T>`, `CrudTable<T>`).
- `spec:check` validates only that mapped fields exist in the method's request and response descriptors.
- The server side stays hand-written use cases and handlers.

## Consequences
- A standard screen is a few lines of YAML, and any existing method can feed it.
- Mapping config is more verbose than a fixed convention; defaults (`rows`, `total`, `next`) keep simple cases short.
- These components are core catalog items and need thorough Storybook and Playwright coverage.
