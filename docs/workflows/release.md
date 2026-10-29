# Release workflow

1. `main` is always releasable; CI green.
2. Merge to `main` triggers build, push, deploy over SSH, migrations, smoke test.
3. Watch logs and the runs page for 15 minutes.
4. On failure: redeploy the previous tag (migrations are expand-only within a release).
5. Note notable changes in the changelog (generated from conventional commits).
