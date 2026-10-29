# ADR 0022: Comments, attachments and change history on records

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
Users expect to comment on a record, attach files and see what changed (Odoo's chatter). Audit is a security log, not a user feature. Keep this small.

## Decision
- An app opts in per record type in the manifest: `records: { lead: { table: leads, comments: true, attachments: true, history: [stage, owner_id] } }`.
- Platform tables keyed by record ref (`app_instance_id, type, id`): `record_comments` (markdown, @mentions notify) and `record_attachments` (links to platform files).
- **History is read from audit**, not a new table: the timeline shows `audit_events` for the record, limited to the declared fields.
- Access: a user sees a record's timeline only if they can read the record. The platform checks with a lookup in the instance schema under the user's RLS (ADR 0018).
- One catalog component, `RecordTimeline`, for the `DetailPage` side panel.
- Not now: followers, scheduled activities, email threads.

## Consequences
- Every app gets a record timeline for one manifest line.
- History lives as long as audit retention keeps it.
