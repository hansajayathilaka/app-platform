# ADR 0028: App-scoped UI components for hand-written apps

- **Status:** Proposed
- **Date:** 2026-10-30
- **Deciders:** Hansa
- **Amends:** ADR 0010

## Context
Today a screen the catalog cannot express needs a reviewed platform catalog component, even in a trusted hand-written app. Hand-written apps are bundled at build time (ADR 0014), so their code is as trusted as the shell.

## Decision
- A hand-written app may ship components in `apps/<id>/ui/`, each with a Zod props schema, registered in the catalog under its namespace (`<app>.SchedulingBoard`), bundled into the shell at build time and usable only in that app's specs.
- Same rules as catalog components: no data fetching, data in and named actions out, a Storybook story.
- A component a second app wants is promoted to `@platform/ui`.
- Studio apps stay catalog-only; their custom UI keeps running in the sandboxed iframe (ADR 0013).

## Consequences
- Complex screens no longer wait on platform review, and the server-driven model stays intact.
- The shell bundle grows with enabled apps; per-app components are code-split and loaded on first use.
