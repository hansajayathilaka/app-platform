---
summary: The app manifest - fields, format (YAML) and validation.
read_when: Creating or editing an app manifest.
related: [app-model.md, ../ui/page-spec.md]
---
# App manifest

Authored in YAML (`apps/<id>/app.yaml`), validated by JSON Schema generated from Zod, the same pipeline as page specs (`../ui/page-spec.md`).

```yaml
# yaml-language-server: $schema=https://<platform>/schemas/manifest/v1.json
id: invoice-reminders
version: 1.2.0
multiInstance: true
permissions:
  - invoice-reminders:rule:read
  - invoice-reminders:rule:write
settings:                 # JSON Schema subset; renders the instance Settings page
  fromEmail: { type: string, format: email }
imports:
  customers: { contract: crm.Customer, version: v1 }
exports:
  readModels: [Reminder.v1]
  events: [reminder.sent.v1]
subscribes:
  - event: crm.customer.created.v1
    handler: OnCustomerCreated
jobs:
  sendDue: { handler: SendDueReminders, schedule: "0 9 * * *", timeout: 5m }
services: proto/invoice_reminders/v1/service.proto
pages: specs/pages/
egress: []                # outbound hosts (required for studio apps)
secrets: []
```

Rules: every handler named must exist; permissions used in specs must be declared; imports must match a published contract version.
