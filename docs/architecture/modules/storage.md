---
summary: The storage module type - Django-style file storage, provider modules (local, S3), bindings, scoping and URLs.
read_when: Storing or serving files, writing a storage provider, or choosing a provider for an app.
related: [README.md, ../apps/sdk.md, ../../decisions/0027-pluggable-file-storage.md, ../../decisions/0024-pluggable-supporting-modules.md]
---
# Storage (module type `storage@1`)

Modelled on Django's storage API (ADR 0027): one interface, interchangeable provider modules, named bindings per app.

## Interface
| Method | Django equivalent | Notes |
| --- | --- | --- |
| `save(name, content, { contentType, overwrite })` | `Storage.save` | Streams; returns the name used (collisions get a suffix unless `overwrite`) |
| `open(name)` | `Storage.open` | Readable stream |
| `delete(name)` | `Storage.delete` | No error if missing |
| `exists(name)` | `Storage.exists` | |
| `stat(name)` | `size`, `get_modified_time` | `{ size, contentType, modifiedAt }` |
| `list(prefix)` | `Storage.listdir` | `{ dirs, files }` |
| `url(name, { expiresIn, download })` | `Storage.url` | Signed and expiring unless the binding is public |
| `getAvailableName(name)` | `get_available_name` | Used by `save` |

## Providers
| Module | When | Settings |
| --- | --- | --- |
| `storage-local` | Phase 1 | `root`; files served by the platform with signed tokens; included in backups |
| `storage-s3` | When first needed | `bucket`, `endpoint`, `region`; keys are secrets. Works with AWS, R2, MinIO, B2 |
| Others (GCS, Azure, SFTP) | On demand | Must pass the `storage` contract suite |

## Choosing providers (app manifest)
```yaml
uses:
  storage:
    type: ^1
    bindings: { default: storage-local, archive: storage-s3, public: { provider: storage-local, public: true } }
```
```ts
const files = ctx.use('storage')            // binding "default"
const name = await files.save('invoices/2026-10.pdf', stream, { contentType: 'application/pdf' })
const link = await files.url(name, { expiresIn: '15m', download: true })
```
- Names are scoped automatically (`instances/<slug>/…`, `modules/<id>/…`), sanitised and size-limited.
- Store metadata (who, which record, original filename) in your own tables; storage holds only bytes.
