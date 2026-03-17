---
name: api-contract-client-sync
description: Define, evolve, and synchronize API contracts, DTO mappings, and error models across Node.js backends, Android or KMP clients, and web frontends. Use when Gemini CLI changes request or response shapes, aligns backend and client models, plans backward-compatible API evolution, or prevents contract drift between services and consumers.
---

# API Contract Client Sync

## Overview

Treat the API contract as a product boundary, not a side effect of implementation. Change server and client mappings together so integration risk stays visible.

## Workflow

1. **Start from the capability, not the JSON.**
   Clarify the business action, expected client experience, and compatibility constraints before deciding field names or nesting.

2. **Separate contract models from internal models.**
   Do not expose persistence entities or internal service responses as the public contract unless that is already an explicit platform rule.

3. **Update producer and consumers as one change set when possible.**
   Modify server DTOs, serializers, API documentation, client mappers, and tests together. If the rollout must be staged, state the compatibility plan explicitly.

4. **Define errors and nullability deliberately.**
   Avoid ambiguous optional fields and transport-level surprises. Prefer explicit error codes or typed failure shapes where the ecosystem supports them.

## Compatibility Rules

- Prefer additive changes over breaking changes when active clients may lag.
- Mark renamed, deprecated, or newly required fields clearly in code and notes.
- Keep timestamp, enum, pagination, and identifier semantics consistent across platforms.
- Add translation layers at the client edge when backend compatibility must be preserved temporarily.

## Output Shape

- Show the before and after contract in concise terms when changing existing APIs.
- Update backend and client tests or fixtures that prove the contract still holds.
- Mention rollout order if server and client cannot ship together.

## Quality Gate

- Verify that each contract field has a clear owner and meaning.
- Verify that server and client mappings agree on nullability, defaults, and error handling.
- Verify that breaking changes are either avoided or called out with a migration path.
