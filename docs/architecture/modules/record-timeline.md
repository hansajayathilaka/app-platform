---
summary: The record-timeline module - comments, attachments and change history on any app record.
read_when: Adding comments, attachments or a history timeline to a record type, or changing RecordTimeline.
related: [README.md, ../core/audit.md, ../core/permissions.md, ../../decisions/0022-record-collaboration.md, ../../decisions/0024-pluggable-supporting-modules.md]
---
# Record-timeline module

Deliberately small (ADR 0022, a module per ADR 0024): comments, attachments, field-change history. No followers, activities or email threads yet.

## Opt in (app manifest)
```yaml
modules:
  record-timeline:
    version: ^1
    records:
      lead:
        table: leads
        comments: true
        attachments: true
        history: [stage, owner_id, amount]
```

## Storage (schema `m_record_timeline`)
- Record ref = `(app_instance_id, type, id)`.
- `record_comments(id, ref, author_id, body_md, created_at, edited_at, deleted_at)`. @mentions notify through a notifications module when one is installed.
- `record_attachments(id, ref, storage_key, name, size, mime, added_by, added_at)`; bytes go through the `storage` module type (`storage.md`), binding `default`.
- History: no table. The timeline reads `audit_events` for the record and shows only the declared `history` fields, after redaction.

## Access
- To see or add to a timeline, the user must be able to read the record: the module calls the core `RecordAccess.canRead(principal, ref)` port, which runs `select 1 from <table> where id = $1` in the instance schema under the user's record rules.
- Comment and attachment changes are audited.

## UI
`record-timeline.RecordTimeline` in the `DetailPage` side panel. RPC: `record_timeline.v1.TimelineService` (`ListTimeline`, `AddComment`, `EditComment`, `DeleteComment`, `AttachFile`, `RemoveAttachment`).
