---
summary: Building images, deploying to the VPS, backups and rollback.
read_when: Releasing, operating, or changing infrastructure.
related: [../decisions/0015-single-vps-hosting.md]
---
# Deployment

- Images: `server` (API + scheduler + workers) and `web` (static files served by Caddy). Multi-stage Docker builds.
- VPS: Docker Compose with Caddy (TLS), server, PostgreSQL. Target 2 vCPU / 4 GB.
- CI on `main`: build, push images, deploy over SSH (`docker compose pull && up -d`), run platform and instance migrations, smoke test.
- Rollback: redeploy the previous image tag; migrations must be backward compatible for one release (expand, then contract).
- Backups: nightly `pg_dump` to off-box storage, plus WAL archiving if needed, plus the `storage-local` roots (`../architecture/modules/storage.md`). Monthly restore drill.
- Provider modules (e.g. `storage-s3`) take their settings in the admin UI; their credentials are deployment secrets.
- Secrets: env file on the VPS (never in git); master encryption key for instance secrets.
