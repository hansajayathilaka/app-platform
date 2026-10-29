# ADR 0010: Server-driven UI with YAML specs validated by JSON Schema

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
Studio apps must deploy at runtime without a frontend rebuild; Hansa wants a human-friendly, validated format rather than raw JSON.

## Decision
Static shell. Pages and manifests authored in YAML, validated (strict parse, Ajv against JSON Schema generated from Zod, semantic checks), compiled to JSON and served via GetPage, filtered by permissions. The spec format is ours.

## Consequences
- Runtime deployment and safe AI generation.
- We maintain the schema and validator; complex UIs need catalog components.
