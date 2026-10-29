---
summary: The static React shell, default layouts and the shared component set.
read_when: Working on navigation, layouts or shared components.
related: [server-driven-ui.md, ui-catalog.md]
---
# Shell, layouts and components

The shell is a static Vite build served by Caddy. It owns login, navigation, permissions and rendering of page specs. App authors write specs, not React.

## Shell
- Auth gate, sidebar from installed instances, top bar with search, command palette, notifications, user menu.
- Built-in pages (also specs where practical): sign in, profile and sessions, members and invitations, roles (with record-rule scopes), API tokens, contacts, installed apps and instance settings, schedules and runs, audit log, platform settings.
- Provides the Connect client, query cache (TanStack Query), toasts, error boundaries, `useCan()`.

## Default layouts
| Layout | Structure | Used for |
| --- | --- | --- |
| `AppShellLayout` | Sidebar + top bar + content | Every signed-in page |
| `ListPage` | Header, actions, search bar (filters, group-by, saved filters), data table, pagination | Records, runs, audit |
| `DetailPage` | Header with status and actions, tabs, side panel | One record, job, user |
| `FormPage` | Sectioned form, sticky save bar, dirty guard | Create, edit, settings |
| `SettingsLayout` | Sub-nav + sections | Platform and instance settings |
| `BoardPage` | Search bar + kanban columns by a field, drag to change it | Pipelines, tickets |
| `CalendarPage` | Search bar + month/week/day calendar | Bookings, schedules |
| `DashboardLayout` | Card grid | App home, KPIs |
| `AuthLayout` | Centred card | Login, reset |
| `BlankLayout` | Full screen | Editors |

## Components
- Primitives: Button, Input, Textarea, Select, Combobox, Checkbox, Switch, DatePicker, Badge, Avatar, Tooltip, Dialog, Drawer, Tabs, Toast, Skeleton.
- Composites: DataTable (server-side sort/filter/group-by/paging, bulk actions), Form (schema-driven), PageHeader, EmptyState, ConfirmDialog, StatusBadge, KeyValueList.
- Data views: SearchBar (filters, group-by, facets, saved filters per user), Kanban, Calendar, PivotTable, Chart, RecordTimeline (comments, attachments, history; `../core/records.md`).
- Master data: ContactPicker, CountrySelect, CurrencyInput, UnitInput.
- Automation: CronInput (preview + next runs), RunStatus, LogViewer (streaming), JsonViewer, SecretInput, UserPicker, RecordPicker.
- Tailwind + Radix (shadcn pattern, code we own), CSS-variable theming, light/dark, Storybook.
