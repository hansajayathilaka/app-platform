---
summary: Pluggable file storage - the FileStorage port, named storages, backends, scoping and URLs (Django-style).
read_when: Storing or serving files, adding a storage backend, or configuring storages for a deployment.
related: [../apps/sdk.md, ../../guides/ports-and-adapters.md, ../../decisions/0027-pluggable-file-storage.md]
---
# File storage

Modelled on Django's storage API (ADR 0027): one port, named storages chosen in config, backends as adapters.

## Port
| Method | Django equivalent | Notes |
| --- | --- | --- |
| `save(name, content, { contentType, overwrite })` | `Storage.save` | Streams; returns the name used (collisions get a suffix unless `overwrite`) |
| `open(name)` | `Storage.open` | Readable stream |
| `delete(name)` | `Storage.delete` | No error if missing |
| `exists(name)` | `Storage.exists` | |
| `stat(name)` | `size`, `get_modified_time` | `{ size, contentType, modifiedAt }` |
| `list(prefix)` | `Storage.listdir` | `{ dirs, files }` |
| `url(name, { expiresIn, download })` | `Storage.url` | Signed and expiring unless the storage is public |
| `getAvailableName(name)` | `get_available_name` | Used by `save` |

## Named storages (deployment config)
```yaml
storages:
  default: { backend: local, options: { root: /data/files } }
  public:  { backend: local, options: { root: /data/public, publicUrl: /files } }
```
- `default` must exist. Backend credentials are secrets (`docs/guides/deployment.md`), never in this file.

## Backends
| Backend | When | Notes |
| --- | --- | --- |
| `local` | Phase 1 | Files under `root`; served by the platform with signed tokens; included in backups |
| `s3` | When first needed | Any S3-compatible service (AWS, R2, MinIO, B2); presigned URLs |
| Others (GCS, Azure, SFTP) | On demand | Same contract suite |

## Use
```ts
const files = ctx.storage()            // or ctx.storage('public')
const name = await files.save('invoices/2026-10.pdf', stream, { contentType: 'application/pdf' })
const link = await files.url(name, { expiresIn: '15m', download: true })
```
- Names are scoped automatically (`instances/<slug>/…`, `modules/<id>/…`) and sanitised; size limits per storage.
- Store metadata (who, which record, original filename) in your own tables; storage holds only bytes.
