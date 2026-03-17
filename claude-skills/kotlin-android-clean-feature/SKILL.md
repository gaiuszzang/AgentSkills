---
name: kotlin-android-clean-feature
description: Plan, implement, and refactor Kotlin or Android features with clean architecture boundaries, use cases, repository ports, ViewModel state, dependency injection, and tests. Use when Claude Code works on Android app features, JVM Kotlin modules, presentation-domain-data separation, or layered refactors where dependency direction matters.
---

# Kotlin Android Clean Feature

## Overview

Implement Android or pure Kotlin features as thin delivery layers over explicit domain rules. Prefer stable boundaries, testable use cases, and feature-local changes over cross-cutting rewrites.

## Workflow

1. Map the current feature slice before editing.
Identify the screen, ViewModel, use cases, repositories, DTOs, and persistence or network adapters already involved. Note any layer leak before adding new code.

2. Define the domain change first.
Name the business action as a use case. Clarify inputs, outputs, failure states, and invariants before touching UI or transport models.

3. Keep layer roles narrow.
- Domain: entities, value objects, use cases, repository contracts, pure business rules.
- Data: repository implementations, mappers, local or remote data sources.
- Presentation: ViewModel, UI state, UI events, navigation handoff.

4. Make the ViewModel a coordinator, not a rules engine.
Expose immutable state, consume intent-like events, call use cases, and translate results into screen state. Keep formatting and mapping near the presentation edge.

5. Hide frameworks behind boundaries when possible.
Avoid leaking Retrofit responses, Room entities, Android context, or DI framework details into domain logic.

## Decision Rules

- Prefer feature-oriented packages or modules over global `data`, `domain`, and `ui` dumping grounds when the codebase allows it.
- Introduce repository interfaces only where they protect domain or application logic from concrete infrastructure.
- Use one use case per meaningful business action; do not create ceremonial wrappers around trivial pass-through calls.
- Keep coroutine scope ownership explicit. Let ViewModel own UI-triggered jobs and keep suspend boundaries obvious.
- Add mappers where transport or storage models differ from domain language.

## Output Shape

- Describe the intended layer changes before editing.
- Implement the smallest coherent slice that preserves dependency direction.
- Add or update tests for use cases, repository behavior, and ViewModel state transitions when behavior changes.
- Call out any remaining architectural debt explicitly instead of silently widening it.

## Quality Gate

- Verify that domain code has no Android imports.
- Verify that UI state is reproducible from inputs and side effects.
- Verify that new abstractions remove coupling instead of adding ceremony.
