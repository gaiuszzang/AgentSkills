---
name: node-backend-clean-service
description: Build and refactor Node.js backend services around clean architecture, use-case oriented modules, DTO validation, repository ports, adapters, transactions, and tests. Use when Claude Code edits APIs, workers, service modules, database flows, or backend refactors in Express, Fastify, Nest, or custom Node.js applications where layer boundaries should stay explicit.
---

# Node Backend Clean Service

## Overview

Implement backend behavior as application use cases over clear ports, with framework code kept at the edge. Favor explicit request validation, stable domain language, and testable composition.

## Workflow

1. Define the use case before the route or handler details.
State the business action, input contract, output contract, authorization rule, and side effects. Keep this language stable even if the HTTP shape changes.

2. Keep the transport edge thin.
Let controllers or handlers parse input, invoke a use case, and translate the result to HTTP, queue, or cron semantics. Do not bury business branching in framework decorators or middleware chains.

3. Validate at the boundary.
Parse and validate DTOs at ingress. Convert to domain inputs before calling application logic. Keep persistence or API response schemas out of the core.

4. Isolate infrastructure behind ports.
Hide ORM, SQL, cache, messaging, and third-party SDK details behind adapters that the use case can depend on abstractly.

5. Make transactions and idempotency explicit.
When multiple writes or side effects occur, show the unit of work clearly and define retry behavior deliberately.

## Module Rules

- Organize by feature or use case cluster before organizing by technical layer.
- Keep domain errors and application errors distinct from raw transport errors.
- Prefer composition roots and dependency factories over implicit singleton wiring.
- Do not return ORM entities directly from application logic unless the codebase already treats them as domain models.

## Output Shape

- Show the request flow from adapter to use case to port to adapter when changing behavior.
- Add focused tests for validation, use case branching, and repository or adapter behavior.
- Note migration, schema, or operational follow-up when persistence contracts change.

## Quality Gate

- Verify that framework code depends inward, not the reverse.
- Verify that validation, authorization, and persistence concerns remain visible and testable.
- Verify that new abstractions remove accidental coupling instead of hiding simple logic.
