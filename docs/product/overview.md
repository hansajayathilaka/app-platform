---
summary: What the platform is for, its goals and non-goals.
read_when: You are new to the project or need the "why" behind a requirement.
---
# Product overview: app-platform (working name)

A platform for building small apps on a shared core, mostly automation tools and scheduled jobs. Owner: Hansa.

## Goals
- Write a new automation app in a day: manifest, a few use cases, a page spec or two. Auth, permissions, audit, scheduling and UI come from the platform.
- Install the same app several times as separately named instances, each with its own data.
- Apps share data through explicit, versioned contracts, never by reading each other's tables.
- Every state change and job run is attributable to a user or service identity and lands in the audit log.
- Generate whole apps (data model, pages, workflows, logic) with the in-platform App Studio.
- Run cheaply: one VPS, low CPU and RAM.

## Non-goals (v1)
- Multi-tenancy. One deployment serves one organisation; a separate customer gets a separate deployment.
- Public self-serve signup. Admins invite users.
- Third-party developers publishing apps, billing, a marketplace.

## Users
- **Admin**: installs app instances, manages users and roles, approves studio apps.
- **Builder**: writes apps by hand or with the studio.
- **Member**: uses installed apps within the permissions granted per instance.
