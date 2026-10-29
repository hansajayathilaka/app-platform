# ADR 0016: Conventional Commits and Semantic Versioning

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
Releases, changelogs and compatibility of apps, contracts and the SDK must be predictable for people and AI agents.

## Decision
- Every commit and PR title follows [Conventional Commits 1.0](https://www.conventionalcommits.org/). Enforced by commitlint (lefthook locally, CI on PR titles).
- Everything versioned follows [SemVer 2.0](https://semver.org/). Versions and changelogs are derived from commits by release-please; nobody edits a version by hand.
- Details in `docs/guides/versioning.md`.

## Consequences
- Changelogs and tags are automatic; breaking changes are visible in history (`!` / `BREAKING CHANGE:`).
- Squash-merge PRs so the PR title is the commit that counts.
- While below 1.0.0 the public surfaces (SDK, manifest, page spec, proto) may still change in minor releases.
