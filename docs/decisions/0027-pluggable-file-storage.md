# ADR 0027: File storage as the first module type (Django-style)

- **Status:** Accepted
- **Date:** 2026-10-30
- **Deciders:** Hansa

## Context
The core's modules (record-timeline attachments) and apps need to store files, and where files live must be swappable without code changes. Django's storage API is the model: one abstract `Storage` class, named storages (`STORAGES`), and interchangeable backends (`FileSystemStorage`, django-storages' S3). ADR 0024 gives the mechanism: module types and provider modules.

## Decision
- Module type `storage@1` (`packages/module-types/storage`) with Django's operations, async and streaming: `save(name, content, opts) -> name` (the name actually used; `getAvailableName` resolves collisions), `open(name) -> stream`, `delete`, `exists`, `stat -> { size, contentType, modifiedAt }`, `list(prefix) -> { dirs, files }`, `url(name, { expiresIn, download })`.
- Provider modules: `storage-local` (phase 1) and `storage-s3` (any S3-compatible service: AWS, Cloudflare R2, MinIO, Backblaze B2) when first needed; others (GCS, Azure, SFTP) on demand. Each passes the `storage` contract suite. Root paths, buckets and credentials are provider settings and secrets.
- Apps choose providers with named bindings, like Django's storage aliases: `uses: { storage: { type: ^1, bindings: { default: storage-local, archive: storage-s3 } } }`, then `ctx.use('storage')` or `ctx.use('storage', 'archive')`.
- Scoping and safety are enforced by the platform around every provider: names are prefixed with the caller's scope (`instances/<slug>/`, `modules/<id>/`), sanitised (no `..`, no absolute paths) and size-limited. An app cannot reach another app's files.
- `url()`: presigned URLs on S3; on local disk a platform route with a signed, expiring token; plain URLs for a binding marked public.
- File metadata (owner, record, original name) is the caller's data in its own tables; storage only holds bytes by name.

## Consequences
- Moving from disk to object storage is a manifest change plus a copy job.
- The `storage-local` roots must be in the VPS backup set.
- A `files` module (upload UI, thumbnails, galleries) can be built on this type later, when an app needs it.
