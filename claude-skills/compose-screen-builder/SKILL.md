---
name: compose-screen-builder
description: Build and refactor Jetpack Compose or Compose Multiplatform screens with unidirectional state, platform-aware UI boundaries, side-effect discipline, navigation, accessibility, and tests. Use when Claude Code edits shared or platform-specific Compose UI for Android, iOS, desktop, or web. Do not use for a purely native SwiftUI screen.
---

# Compose Screen Builder

## Overview

Build Compose UI from explicit state and event contracts instead of mixing business logic into composables. Share UI where behavior is genuinely common while preserving native platform expectations and deliberate interop seams.

## Workflow

1. Start from the contract.
Define screen state, user events, and one-off effects before composing the layout. Prefer names that reflect user intent and UI meaning.

2. Separate container and presentation concerns.
Keep platform entry points or route-level composables responsible for ViewModel ownership, navigation callbacks, lifecycle-aware collection, and platform services. Keep presentational composables focused on rendering provided state.

3. Keep side effects explicit.
Use `LaunchedEffect`, `DisposableEffect`, and state collection only for clear lifecycle reasons. Avoid hidden work during recomposition.

4. Model loading, empty, content, and error states deliberately.
Do not rely on nullable state or boolean flag combinations that create impossible UI states.

5. Decide the platform boundary before sharing UI.
Keep reusable screen content, resources, and stable interaction contracts in shared source sets. Keep permission launchers, window or scene APIs, native controls, and platform navigation at a platform edge unless the project has already chosen a tested multiplatform abstraction.

## Design Rules

- Hoist state to the lowest owner that needs to coordinate it.
- Prefer stateless reusable composables with small, typed parameters over wide parameter bags.
- Keep theming, spacing, typography, and interaction patterns aligned with the existing design system.
- Use multiplatform resources and localization APIs in shared UI; do not introduce Android resource or `Context` dependencies into `commonMain`.
- Add previews where the target tooling supports them, especially for reusable components and representative screen states; do not treat previews as cross-platform verification.
- Surface accessibility labels, roles, touch targets, and focus behavior as part of implementation, not cleanup.
- Check platform conventions such as iOS safe areas, back gestures, keyboard behavior, pointer input, and native text or selection behavior on each affected target.
- Treat `UIKitView`, `UIKitViewController`, SwiftUI wrappers, and other native interop components as owned resources: keep creation stable, updates idempotent, callbacks current, and disposal explicit.

## Output Shape

- Produce clear route/screen/component boundaries.
- Keep navigation, snackbar, permission, and analytics hooks at the route edge.
- Use typed routes and lightweight navigation arguments. Decide explicitly whether shared Compose navigation or a native platform coordinator owns the back stack.
- Add common UI tests for shared behavior plus target-level tests for platform integration. Use stable semantic tags that can map to Android and iOS automation when behavior is critical.
- Mention recomposition or performance risks if the change introduces broad state reads.

## Quality Gate

- Verify that composables remain pure given the same state.
- Verify that state transitions are representable without ambiguous flag combinations.
- Verify that `commonMain` UI has no accidental Android, UIKit, or SwiftUI dependencies.
- Verify the main visual states and critical interaction flows on every affected target, not only an Android preview.
