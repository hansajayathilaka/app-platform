# New app workflow

1. Confirm the app's purpose, permissions and imports in an issue.
2. Run skill `new-app` (or follow `guides/writing-an-app.md`).
3. Check: no imports of other apps' internals, all permissions declared, specs pass `spec:check`.
4. Add the app to the deployment config; install an instance in dev; add an e2e test for its main flow.
5. Review, merge, deploy; install instances in production.
