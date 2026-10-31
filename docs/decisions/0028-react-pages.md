# ADR 0028: App pages are React components, not YAML specs

- **Status:** Accepted
- **Date:** 2026-10-31
- **Deciders:** Hansa

## Context
The product goal (agreed 2026-10-31, `product/overview.md`) is minutes from idea to a running app with Claude Code as the first-class builder. ADR 0010 made pages YAML specs rendered from a catalog, mainly so studio apps could deploy at runtime. Runtime-generated pages are no longer part of the requirement.

Options: (a) declarative YAML/JSON specs; (b) React (TSX) pages composed from platform components.
(a) validates strictly and deploys without a build, but is our own format the agent has never seen, so it needs more instruction context and makes more mistakes; we would also maintain a schema, compiler and renderer, and anything the format cannot express needs an awkward escape hatch.
(b) is where coding agents are strongest, the type checker catches wrong props at once, escape hatches are built in, and there is much less platform to build.

## Decision
App pages are React (TSX) files composed from `@platform/ui` components and layouts. Config-driven components (for example `CrudTable` bound to a method by field mappings, ADR 0025) keep pages short. Custom CSS and custom components are allowed but discouraged. The server enforces permissions on every call regardless of what the page shows.

## Consequences
- Faster app building and smaller agent instructions.
- Uniform look is enforced by lint and review, not guaranteed by a format.
- Changing a page needs a build and deploy.
- Supersedes ADR 0010 and ADR 0011 (json-render). The UI architecture docs (`architecture/ui/*`), the studio docs and the `spec:check` command must be updated in follow-up PRs.
