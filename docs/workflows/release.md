# Release workflow

1. `main` is always releasable; CI green. PRs are squash-merged with a Conventional Commit title.
2. release-please keeps an open release PR with the next SemVer versions and generated changelogs (`docs/guides/versioning.md`).
3. Merging the release PR tags `vX.Y.Z` (and `app-<id>-vX.Y.Z` for changed apps), builds and pushes images, deploys over SSH, runs migrations and a smoke test.
4. Watch logs and the runs page for 15 minutes.
5. On failure: redeploy the previous tag (migrations are expand-only within a release), then ship a `fix:` release.
