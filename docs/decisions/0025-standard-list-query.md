# ADR 0025: Standard list query and common contract types

- **Status:** Proposed
- **Date:** 2026-10-30
- **Deciders:** Hansa

## Context
DataTable, SearchBar, BoardPage, CalendarPage, PivotTable and Chart all need filtering, sorting, paging, grouping and aggregates. Without one contract every app writes these endpoints by hand, differently each time.

## Decision
- `platform.common.v1` defines `ListQuery { filter, sort[], page { cursor, size }, group_by[], aggregates[], search }`, `ListResult { rows, page_info, groups[] }`, and shared types `Money` (minor units + currency), `Ref`, `Address`, `DateRange`.
- `filter` is a tree (`and`/`or`/`not` over `field op value`), never SQL. Operators: `eq ne lt lte gt gte in like is_null between`.
- A server `ListQueryCompiler` turns it into SQL over an allowlist of declared fields (from the proto message and the CRUD kit definition), always under record rules (ADR 0018) and field permissions. Unknown fields are rejected.
- Catalog data views bind to any method that takes `ListQuery` and returns `ListResult`; saved filters store a `ListQuery`.
- Cursor paging by default; offset only for small tables.

## Consequences
- List screens need no custom endpoint; the CRUD kit's List uses the compiler.
- Group-by and aggregates on large tables need indexes; the entity generator adds them for declared filter and group-by fields.
- The contract is a platform API and evolves with SemVer like any other.
