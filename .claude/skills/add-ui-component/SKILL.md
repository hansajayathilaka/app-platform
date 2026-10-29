---
name: add-ui-component
description: Add a component or layout to the @platform/ui catalog so page specs can use it. Use when a page needs UI the catalog lacks.
---
# Add a UI catalog component

Read first: `docs/architecture/ui/ui-catalog.md`.

1. Check the catalog first; extend an existing component if it fits.
2. In `packages/ui/src/catalog/<Name>/`: Zod props schema, description (written for the studio agent), React implementation (Tailwind + Radix), Storybook story.
3. Components do not fetch; they take data and emit named actions.
4. Register it in the catalog index; run `pnpm catalog:generate` (JSON Schema, registry, agent prompt).
5. Storybook, accessibility check, `pnpm check`.
