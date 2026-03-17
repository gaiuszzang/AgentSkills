---
name: node-frontend-feature-architecture
description: Implement and refactor Node.js-based frontend features with clear feature boundaries, server and client responsibilities, UI state discipline, and maintainable data flows. Use when Codex edits React, Next.js, Remix, Vite, or similar frontend codebases for page flows, forms, data fetching, component organization, or frontend architectural cleanup.
---

# Node Frontend Feature Architecture

## Overview

Structure frontend work by feature and user flow instead of scattering logic across shared folders too early. Keep data loading, mutation handling, and UI rendering responsibilities explicit.

## Workflow

1. Start from the user flow.
Clarify what the page or feature must show, what the user can do, and which server interactions or local state transitions occur.

2. Separate data concerns from presentation.
Keep loaders, queries, mutations, or server actions close to the route or feature container. Keep presentational components reusable and mostly unaware of transport details.

3. Normalize external data before it reaches the UI.
Convert raw API or database shapes into view models or feature models so rendering code does not absorb backend noise.

4. Keep side effects and async states deliberate.
Model loading, success, empty, error, and optimistic transitions explicitly. Avoid spreading fetch calls and mutation handling across arbitrary components.

## Module Rules

- Prefer feature folders or route-local modules before promoting code to global shared space.
- Keep form schemas, validation, and submission orchestration near the feature that owns them.
- Keep design-system components dumb and composable; keep business rules out of them.
- Distinguish server-only code, client-only code, and shared utilities clearly in frameworks that support both.

## Output Shape

- Explain the feature boundary before broad refactors.
- Update tests closest to behavior: component tests, route tests, or integration tests.
- Call out caching, stale data, or hydration risks when the framework makes them relevant.

## Quality Gate

- Verify that rendering code no longer depends on raw backend response noise.
- Verify that async states are explicit and recoverable.
- Verify that shared components or hooks were extracted because of repeated need, not guesswork.
