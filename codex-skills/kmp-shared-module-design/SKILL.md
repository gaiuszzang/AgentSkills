---
name: kmp-shared-module-design
description: Design and refactor Kotlin Multiplatform modules and their Kotlin-to-Swift boundary with clear source-set ownership, platform seams, exported APIs, integration packaging, and shared domain logic. Use when Codex works on KMP extraction, Android/iOS sharing, Swift interop, or framework integration. Use compose-screen-builder for shared Compose UI details.
---

# KMP Shared Module Design

## Overview

Design shared modules that maximize reused business logic without forcing fake abstraction. Keep common code stable and push platform variability to deliberate seams.

## Workflow

1. Decide what truly belongs in shared code.
Share domain rules, value transformations, orchestration, and stable contracts. Keep SDK bindings, UI frameworks, device APIs, and platform persistence details out of `commonMain`.

2. Choose the seam type deliberately.
- Use interface ports when the platform difference is infrastructural.
- Use `expect` and `actual` only when the shared API itself must vary by platform.
- Use shared DTO-to-domain mapping only if the transport semantics are genuinely shared.

3. Protect common models.
Use platform-neutral types in shared code. Avoid Android classes, Java time types that do not map cleanly, and UI-specific models unless the project already standardizes them.

4. Keep dependency direction inward.
Let platform modules depend on shared modules. Do not let shared modules reach back into application-specific wiring.

5. Design the Swift-facing API as a product boundary.
Inspect the API as generated for Swift, not only as Kotlin source. Keep exported names, nullability, sealed hierarchies, errors, suspend functions, flows, and collection types usable from the project's chosen Objective-C interop or Swift export path. Do not adopt experimental Swift export implicitly; preserve the project's integration mode and state its limitations when proposing it.

6. Choose iOS integration and ownership deliberately.
Distinguish direct Xcode integration from distributed XCFramework, SwiftPM, or CocoaPods workflows. Keep framework linkage, exported dependencies, build variants, and simulator/device architectures consistent with how the iOS app consumes the shared module.

## Module Rules

- Prefer small, capability-based shared modules over one giant `shared` bucket.
- Use intermediate source sets such as `appleMain` or `iosMain` only for genuinely shared Apple/iOS behavior, not as a second dumping ground.
- Keep serialization, networking, and storage libraries behind adapters when they create lock-in or platform friction.
- Model errors and results in shared language that each platform can translate cleanly.
- Make coroutine scope ownership, cancellation, and callback or Flow lifetime explicit at the Swift boundary; dispatch UI updates according to the native UI owner rather than assuming every exported call belongs on the main thread.
- Avoid exporting broad dependency graphs accidentally. Treat every public declaration and exported dependency as part of the iOS binary API and build-time cost.
- Document ownership of source sets and platform implementations when extracting a new seam.

## Output Shape

- Show the proposed source set or module boundary before heavy edits.
- Implement shared contracts and platform adapters together so the seam is complete.
- Add tests in `commonTest` for shared behavior and platform tests only for platform-specific adapters.
- When the change affects iOS consumption, show the Swift-facing call shape and the selected Xcode integration path.

## Quality Gate

- Verify that `commonMain` remains free of platform imports.
- Verify that each new seam has at least one real platform implementation.
- Verify exported APIs by building the Apple target or framework and, when practical, compiling the Swift consumer rather than trusting Kotlin compilation alone.
- Verify cancellation, lifecycle, and error translation across the Kotlin/Swift boundary.
- Verify that the shared abstraction reduces duplication instead of spreading platform complexity everywhere.
