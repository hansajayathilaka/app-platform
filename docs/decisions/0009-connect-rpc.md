# ADR 0009: Connect RPC for APIs

- **Status:** Accepted
- **Date:** 2026-10-29
- **Deciders:** Hansa

## Context
Considered gRPC instead of REST. Browsers cannot call native gRPC without a proxy.

## Decision
Connect RPC with protobuf contracts (buf). One server speaks gRPC, gRPC-Web and JSON. Plain HTTP only for inbound webhooks.

## Consequences
- Schema-first, generated clients, server streaming to browsers.
- No client/bidi streaming in browsers (WebSocket adapter later if needed); codegen step; protobuf evolution rules.
