---
summary: The UI catalog - how components become usable in page specs.
read_when: Adding or changing a component that page specs can use.
related: [page-spec.md, shell-and-layouts.md]
---
# UI catalog

The catalog is the single source of truth for what a page spec may contain.

- Each entry: name, Zod props schema, description (also shown to the studio agent), React implementation, Storybook story.
- From the catalog we generate: the page JSON Schema, the json-render registry, the agent's component prompt, and Storybook docs. DRY: one definition, four outputs.
- Actions are named and map to Connect RPC methods; components never fetch on their own.
- Adding a component: skill `add-ui-component`.
