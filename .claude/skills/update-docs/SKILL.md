---
name: update-docs
description: Bring docs, indexes and AGENTS.md files in line with a code change. Use at the end of any change that alters documented behaviour.
---
# Update docs

1. List what changed (`git diff --stat main`).
2. Find affected docs via `docs/README.md` and `docs/architecture/README.md`.
3. Edit only those docs. Keep each under about 150 lines; split by topic rather than grow.
4. Keep the front matter (`summary`, `read_when`) accurate; update the index line if scope changed.
5. Update nested `AGENTS.md` if a local rule changed; root `AGENTS.md` only for project-wide rules.
6. Never document plans as facts: roadmap items go in `docs/roadmap.md`.
