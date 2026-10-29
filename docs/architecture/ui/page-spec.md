---
summary: The YAML page spec format, its validation pipeline and editor tooling.
read_when: Writing, generating or validating a page spec.
related: [server-driven-ui.md, ui-catalog.md, ../apps/manifest.md]
---
# Page spec (YAML)

Pages are written in YAML by people and the studio agent; the browser only receives JSON.

```yaml
# yaml-language-server: $schema=https://<platform>/schemas/page/v1.json
version: 1
layout: ListPage
title: Customers
actions:
  - label: New customer
    action: crm.CreateCustomer      # must exist in the app's .proto contract
    requires: crm:customer:write
body:
  - component: DataTable
    source: crm.ListCustomers
    columns: [name, email, createdAt]
```

## Validation pipeline (on `pnpm spec:check` and on publish)
1. **Parse**: `yaml` (eemeli/yaml) strict mode, no custom tags, alias limits. Keeps line/column ranges.
2. **Schema**: Ajv (+ `ajv-formats`, `ajv-errors`) against JSON Schema generated from the catalog's Zod schemas (`z.toJSONSchema`).
3. **Semantics**: actions and sources exist in the app's contracts, bound fields exist, `requires` permissions are declared in the manifest.
Errors report YAML line and column.

## Compile
A valid spec is stored as JSON with the app version. The client never parses YAML.

## Tooling
- VS Code: Red Hat YAML extension reads the `$schema` comment (autocomplete, inline errors).
- Studio: Monaco + `monaco-yaml` with the same schema.
