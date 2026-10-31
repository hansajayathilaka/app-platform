# ADR 0011: json-render as the web renderer

- **Status:** Superseded by 0028
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
Options: json-render, A2UI, amis, JSON Forms/RJSF, own renderer. Mobile is possible but unconfirmed.

## Decision
json-render (React) behind the PageRenderer port, pinned exact versions.

## Consequences
- TS/Zod native, catalog prompt for the agent, streaming render, React Native option.
- Young project (Jan 2026): churn risk, contained by the port and our own spec format; A2UI remains the fallback for fully native mobile.
