---
name: kmp-shared-module-design
description: Design and refactor Kotlin Multiplatform shared modules with clear commonMain and platform boundaries, expect/actual seams, repository ports, and shared domain logic. Use when Codex works on KMP module extraction, cross-platform data or domain sharing, platform adapter design, or preventing Android or iOS concerns from leaking into shared code.
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

## Module Rules

- Prefer small, capability-based shared modules over one giant `shared` bucket.
- Keep serialization, networking, and storage libraries behind adapters when they create lock-in or platform friction.
- Model errors and results in shared language that each platform can translate cleanly.
- Document ownership of source sets and platform implementations when extracting a new seam.

## Output Shape

- Show the proposed source set or module boundary before heavy edits.
- Implement shared contracts and platform adapters together so the seam is complete.
- Add tests in `commonTest` for shared behavior and platform tests only for platform-specific adapters.

## Quality Gate

- Verify that `commonMain` remains free of platform imports.
- Verify that each new seam has at least one real platform implementation.
- Verify that the shared abstraction reduces duplication instead of spreading platform complexity everywhere.
