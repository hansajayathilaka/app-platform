# Architecture change workflow

Use when changing a decision in `docs/decisions/`, adding or changing a port, or bending the dependency rule.

1. Write a new ADR (skill `write-adr`) with status Proposed: context, options, decision, consequences.
2. Get Hansa's approval. The ADR becomes Accepted; the old one is marked "Superseded by NNNN".
3. Update affected architecture docs and `AGENTS.md` golden rules if needed.
4. Implement behind ports; contract tests first.
5. Update dependency-cruiser rules if boundaries changed.
