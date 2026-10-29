---
name: write-adr
description: Record an architecture decision or propose changing an existing one as an ADR. Use when a choice affects structure, technology or a documented decision.
---
# Write an ADR

1. Read `docs/decisions/README.md` and any ADR the change touches.
2. Copy `docs/decisions/template.md` to the next number: `NNNN-short-title.md`.
3. Fill context (options considered), decision, consequences. Status: Proposed.
4. Add a row to `docs/decisions/README.md`.
5. If it replaces an ADR, set the old one's Status to "Superseded by NNNN" only after approval.
6. Ask Hansa for approval before implementing.
