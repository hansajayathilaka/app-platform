# ADR 0013: Studio apps are specs plus sandboxed code, human-approved

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
The studio must generate everything a hand-written app has, without risking the platform.

## Decision
Agent emits manifest, entity definitions, page specs, workflow graphs and TS code steps. Code runs only in V8 isolates via the RPC SDK; custom UI in a sandboxed iframe. Checks, sandbox tests and human approval before an immutable version can be installed.

## Consequences
- Safe runtime deployment; SDK must be an RPC contract from phase 3.
- Approver role still to be decided (requester or admin).
