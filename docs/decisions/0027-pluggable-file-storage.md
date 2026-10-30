# ADR 0027: Pluggable file storage (Django-style)

- **Status:** Accepted
- **Date:** 2026-10-30
- **Deciders:** Hansa

## Context
The core, modules (record-timeline attachments) and apps need to store files. Where files live must be a deployment choice (local disk on the VPS today, object storage later) with no code change. Django's storage API is the model: one abstract `Storage` class, named storages configured in settings (`STORAGES`), and backends such as `FileSystemStorage` or django-storages' S3 swapped by config.

## Decision
- A core `FileStorage` port with Django's operations, async and streaming:
  `save(name, content, opts) -> name` (returns the name actually used, after `getAvailableName` resolves collisions), `open(name) -> stream`, `delete(name)`, `exists(name)`, `stat(name) -> { size, contentType, modifiedAt }`, `list(prefix) -> { dirs, files }`, `url(name, { expiresIn, download }) -> string`.
- **Named storages** in deployment config, each a backend plus options, like Django's `STORAGES`:
  ```yaml
  storages:
    default: { backend: local, options: { root: /data/files } }
    public:  { backend: local, options: { root: /data/public, publicUrl: /files } }
    archive: { backend: s3, options: { bucket: app-archive, endpoint: https://..., region: auto } }
  ```
  Credentials come from secrets, never the config file.
- **Backends are adapters**, each passing one contract test suite: `local` (phase 1) and `s3` (any S3-compatible service: AWS, Cloudflare R2, MinIO, Backblaze B2) when first needed; others (GCS, Azure, SFTP) only on demand.
- **Access:** `ctx.storage(alias = 'default')`. The platform prefixes every name with the caller's scope (`instances/<slug>/`, `modules/<id>/`), sanitises names (no `..`, no absolute paths), and enforces size limits. An app cannot reach another app's files.
- `url()`: the S3 backend returns presigned URLs; the local backend returns a platform route with a signed, expiring token. Public storages return plain URLs.
- File metadata (owner, record, original name) is the caller's data in its own tables; the storage only holds bytes by name.

## Consequences
- Moving from disk to object storage is a config change plus a copy job.
- The local backend's files must be in the VPS backup set.
- A `files` module (upload UI, thumbnails, galleries) can be built on this port later, when an app needs it.
