---
summary: CrudTable, CrudForm and CrudDetail - configurable CRUD screens bound to any RPC method by mapping.
read_when: Building a list, form or detail screen, or changing the CRUD components.
related: [page-spec.md, ui-catalog.md, shell-and-layouts.md, ../../decisions/0025-crud-ui-components.md]
---
# CRUD components

Configurable screens in `@platform/ui` (ADR 0025). They impose no API shape: the spec maps component state to any method's input and output.

## CrudTable
```yaml
- component: CrudTable
  source: ListCustomers
  request:                          # where table state goes in the method's input
    search: query                   # $table.search -> input.query
    filters: { status: statusIn }   # filter "status" -> input.statusIn
    sort: orderBy
    page: { size: limit, cursor: pageToken }
    fixed: { includeArchived: false }
  response:                         # where results sit in the output (defaults: rows, total, next)
    rows: customers
    total: totalCount
    next: nextPageToken
  paging: server                    # server | client | none
  columns: [name, email, { field: createdAt, format: date }]
  filters: [{ name: status, type: enum, options: [active, archived] }]
  search: true
  rowActions: [{ action: edit, open: CustomerForm }, { action: ArchiveCustomer, requires: crm:customer:write, confirm: true }]
  create: { open: CustomerForm, requires: crm:customer:write }
  slots: { toolbar: [{ component: Button, label: { t: customers.import } }] }
```
- No `total` in the response: next/previous paging. `paging: client`: the method returns all rows; the table pages, sorts and filters in the browser.
- Custom cells: `{ field: status, cell: StatusBadge }` or any catalog component.

## CrudForm
```yaml
- component: CrudForm
  id: CustomerForm
  load: { method: GetCustomer, input: { id: $route.params.id }, from: customer }
  save:
    create: { method: CreateCustomer, input: { customer: $form } }
    update: { method: UpdateCustomer, input: { id: $route.params.id, customer: $form } }
  sections:
    - title: { t: customers.details }
      fields: [name, { field: email, widget: Input, type: email }, { field: ownerId, widget: UserPicker }]
```
- Validation: protovalidate rules from the method's descriptor plus optional spec rules. Server field errors map back to fields by path.

## CrudDetail
Header, key-value sections, tabs and actions, loaded by the same `load` mapping. Modules can add panels (e.g. `record-timeline.RecordTimeline`).

## Escape hatches (in order)
1. Props and slots. 2. Custom cell and field components from the catalog. 3. Compose `DataTable` and `Form` directly. 4. Add a catalog component.

## Optional helpers
`platform.common.v1` `PageRequest`, `SortField`, `PageInfo`, and TypeScript `ListResult<T>`, `CrudTable<T>`. Use them when they fit; nothing requires them.
