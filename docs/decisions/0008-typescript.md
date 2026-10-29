# ADR 0008: TypeScript on Node end to end

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
Criteria: popular for AI-assisted development, runtime deployment of studio apps, low CPU/RAM.

## Decision
TypeScript on Node.js LTS for server, SDK, sandbox code and UI.

## Consequences
- One language in browser, server and V8-isolate sandbox; studio bundles load at runtime.
- About 150 MB more RAM than Go; mitigated by a single process, static frontend, lightweight libraries.
