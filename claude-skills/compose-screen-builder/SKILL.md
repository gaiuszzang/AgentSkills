---
name: compose-screen-builder
description: Build and refactor Jetpack Compose screens and components with unidirectional state, side-effect discipline, navigation handoff, previews, accessibility, and testability. Use when Claude Code edits Compose UI, screen state models, design-system components, form flows, or UI tests in Android or multiplatform Compose codebases.
---

# Compose Screen Builder

## Overview

Build Compose UI from explicit state and event contracts instead of mixing business logic into composables. Keep screens easy to preview, test, and evolve.

## Workflow

1. Start from the contract.
Define screen state, user events, and one-off effects before composing the layout. Prefer names that reflect user intent and UI meaning.

2. Separate container and presentation concerns.
Keep route-level composables responsible for wiring ViewModel, navigation callbacks, and lifecycle-aware collection. Keep presentational composables focused on rendering provided state.

3. Keep side effects explicit.
Use `LaunchedEffect`, `DisposableEffect`, and state collection only for clear lifecycle reasons. Avoid hidden work during recomposition.

4. Model loading, empty, content, and error states deliberately.
Do not rely on nullable state or boolean flag combinations that create impossible UI states.

## Design Rules

- Hoist state to the lowest owner that needs to coordinate it.
- Prefer stateless reusable composables with small, typed parameters over wide parameter bags.
- Keep theming, spacing, typography, and interaction patterns aligned with the existing design system.
- Add previews for stable UI slices, especially reusable components and representative screen states.
- Surface accessibility labels, roles, touch targets, and focus behavior as part of implementation, not cleanup.

## Output Shape

- Produce clear route/screen/component boundaries.
- Keep navigation, snackbar, permission, and analytics hooks at the route edge.
- Add UI tests or at least stable test tags when behavior is critical.
- Mention recomposition or performance risks if the change introduces broad state reads.

## Quality Gate

- Verify that composables remain pure given the same state.
- Verify that state transitions are representable without ambiguous flag combinations.
- Verify that previews or tests cover the main visual states.
