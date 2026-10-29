# @platform/ui

- Components and layouts that page specs can use. Each catalog entry: Zod props schema, description, React implementation, Storybook story.
- The catalog generates the page JSON Schema, the json-render registry and the agent prompt. Never edit generated files.
- Components never fetch data; they receive data and emit named actions.
- Tailwind + Radix, CSS variables for theme, accessible by default.
- Skill: `add-ui-component`. Docs: `docs/architecture/ui/ui-catalog.md`.
