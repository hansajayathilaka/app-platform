# ADR 0026: App-scoped UI components for hand-written apps

- **Status:** Accepted
- **Date:** 2026-10-30
- **Deciders:** Hansa
- **Amends:** ADR 0010

## Context
A screen the catalog cannot express needed a new platform catalog component, even inside a trusted hand-written app. Hand-written apps are bundled at build time (ADR 0014), so their code is as trusted as the shell. The platform must not block real business screens.

## Decision
- A hand-written app may ship React components in `apps/<id>/ui/`, each with a Zod props schema and a Storybook story, registered in the catalog under the app's namespace (`crm.SchedulingBoard`). Modules can do the same (ADR 0024).
- They are bundled at build time, code-split per app, and usable only in that app's page specs.
- Same rules as catalog components: data comes in through props and actions go out by name; data loading stays with the shell and RPC methods.
- A component a second app wants is promoted to `@platform/ui`.
- Studio apps stay catalog-only; their custom UI keeps running in the sandboxed iframe (ADR 0013).

## Consequences
- Complex screens (boards, editors, planners) no longer wait on platform review, and pages stay server-driven.
- The shell bundle grows with enabled apps; per-app chunks load on first use.
