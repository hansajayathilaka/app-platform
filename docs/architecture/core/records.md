---
summary: Record collaboration - comments, attachments and change history on any app record.
read_when: Adding comments, attachments or a history timeline to a record type, or changing RecordTimeline.
related: [audit.md, permissions.md, ../../decisions/0022-record-collaboration.md]
---
# Record collaboration

Deliberately small (ADR 0022): comments, attachments, field-change history. No followers, activities or email threads yet.

## Opt in (manifest)
```yaml
records:
  lead:
    table: leads
    comments: true
    attachments: true
    history: [stage, owner_id, amount]
```

## Storage
- Record ref = `(app_instance_id, type, id)`.
- `record_comments(id, ref, author_id, body_md, created_at, edited_at, deleted_at)`. @mentions send a platform notification.
- `record_attachments(id, ref, file_id, added_by, added_at)`; files are platform files.
- History: no table. The timeline reads `audit_events` for the record and shows only the declared `history` fields, after redaction.

## Access
- To see or add to a timeline, the user must be able to read the record: the platform runs `select 1 from <table> where id = $1` in the instance schema under the user's record rules.
- Comment and attachment changes are audited.

## UI
`RecordTimeline` in the `DetailPage` side panel. RPC: `platform.records.v1.RecordService` (`ListTimeline`, `AddComment`, `EditComment`, `DeleteComment`, `AttachFile`, `RemoveAttachment`).
