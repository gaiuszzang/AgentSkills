---
name: ios-swiftui-clean-feature
description: Build and refactor native iOS features in Swift and SwiftUI with explicit state ownership, structured concurrency, navigation, dependency seams, accessibility, and tests. Use when Claude Code works on SwiftUI screens, observable models, UIKit handoff, Apple platform services, or an iOS consumer of a KMP module. Do not use for UI implemented primarily in Compose Multiplatform.
---

# iOS SwiftUI Clean Feature

## Overview

Build native iOS features around user-visible state and explicit dependencies. Keep SwiftUI views declarative, actor ownership clear, and Apple framework or KMP integration at testable edges.

## Workflow

1. Map the feature and its owners.
Identify the SwiftUI entry point, navigation owner, observable state, domain operation, persistence or network adapter, and any UIKit or KMP boundary already in use. Preserve the project's architecture and minimum deployment target unless the task changes them.

2. Define state and actions before composing the view.
Represent loading, empty, content, error, and presentation state without contradictory flags. Keep durable model state separate from short-lived view state such as focus, animation, or sheet visibility.

3. Keep ownership visible.
Let the feature or composition root create long-lived dependencies and models. Inject them into views instead of constructing services in `body`. Use the project's Observation or `ObservableObject` conventions consistently, and choose `@State`, bindings, and environment injection according to actual ownership.

4. Use structured concurrency deliberately.
Tie view-triggered work to a lifecycle-aware task or an owned model task. Propagate cancellation, avoid detached work without a concrete isolation reason, and keep UI mutation on the appropriate actor. Do not use `@MainActor` to hide blocking CPU or I/O work.

5. Keep navigation and platform effects at the edge.
Use lightweight, stable route values rather than passing full models through navigation state. Isolate permissions, notifications, keychain, camera, location, and other Apple frameworks behind focused adapters when they participate in business behavior.

6. Adapt shared Kotlin at the iOS boundary.
Wrap generated KMP APIs in Swift-facing adapters when names, errors, nullability, collections, suspend functions, or flows are awkward for SwiftUI. Give subscriptions and tasks an explicit cancellation owner; do not leak generated interop types throughout native presentation code.

## Design Rules

- Keep `View.body` free of business branching, service lookup, and unowned asynchronous work.
- Prefer small feature models or use cases over a single application-wide observable object.
- Preserve one source of truth; derive display values instead of copying synchronized state across views and models.
- Make deep-link, sheet, full-screen cover, and back-stack ownership explicit when they can be driven programmatically.
- Respect safe areas, Dynamic Type, VoiceOver labels and traits, Reduce Motion, contrast, keyboard focus, and platform-standard gestures.
- Use UIKit interop only at a narrow wrapper with clear creation, update, coordinator, and teardown behavior.

## Output Shape

- State the feature boundary and dependency flow before broad edits.
- Implement the smallest coherent slice, including loading and failure behavior rather than only the happy path.
- Add Swift Testing coverage for new unit-level logic when supported by the project; keep XCTest/XCUITest for UI automation and existing suites.
- When KMP is involved, show the Swift-facing adapter contract and test cancellation and error translation.

## Quality Gate

- Verify state and observable mutations satisfy the project's actor-isolation rules and build under its Swift language mode.
- Verify tasks and subscriptions have an owner, cancellation path, and no unintended retain cycle.
- Verify navigation restoration or deep-link behavior when route state changes.
- Verify critical flows with accessibility identifiers and at least one target-level integration or UI test where risk warrants it.
