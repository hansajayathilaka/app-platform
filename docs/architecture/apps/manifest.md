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
recordRules:              # row scopes per table (../core/permissions.md)
  rules:
    own: { field: created_by, op: eq, value: $principal.id }
fieldPermissions: {}      # e.g. rules.notes: invoice-reminders:rule:read_notes
modules:                  # supporting modules this app needs (../modules/README.md)
  record-timeline:
    version: ^1
    records: { rule: { table: rules, comments: true, history: [enabled, schedule] } }
i18n: i18n/               # message catalogs, en.yaml required (../ui/i18n.md)
data: data/               # upserted on install and upgrade (instances.md)
seed: seed/               # inserted on install only
demo: demo/               # inserted only when "demo data" is ticked
services: proto/invoice_reminders/v1/service.proto
pages: specs/pages/
egress: []                # outbound hosts (required for studio apps)
secrets: []
```

Rules: every handler named must exist; permissions used in specs must be declared; imports must match a published contract version; required modules must be in the build and their config must match the module's `appConfig` schema; record-rule and field-permission targets must exist in the app's tables and proto; user-facing text is a message key (`{ t: key }`).
