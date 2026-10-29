---
name: write-page-spec
description: Write or change a YAML page spec for an app and validate it. Use for any app page or screen.
---
# Write a page spec

Read first: `docs/architecture/ui/page-spec.md`. Available components: `pnpm catalog:list` (or Storybook).

1. Create `apps/<id>/specs/pages/<page>.yaml` with the `$schema` comment on line 1.
2. Pick a layout (`ListPage`, `DetailPage`, `FormPage`, `BoardPage`, `CalendarPage`, `DashboardLayout`, `SettingsLayout`).
3. Use only catalog components. Actions and sources must name RPC methods that exist.
4. Put `requires: <permission>` on every action; the permission must be in `app.yaml`.
5. Write every label as a message key (`{ t: key }`) and add it to `i18n/en.yaml` (`docs/architecture/ui/i18n.md`).
6. `pnpm spec:check` and fix errors at the reported line and column.
7. If the catalog cannot express it, do not write page-specific React: use skill `add-ui-component`.
