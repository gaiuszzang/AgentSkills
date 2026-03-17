---
name: clean-architecture-reviewer
description: Review and improve codebases around clean architecture principles such as dependency direction, module boundaries, entity and use-case separation, ports and adapters, and test seams across Kotlin, Android, KMP, Node.js backend, and web frontend code. Use when Gemini CLI is asked to perform architecture review, identify layer leaks, propose refactors, or judge whether a design change respects clean boundaries.
---

# Clean Architecture Reviewer

## Overview

Review code with a bias toward real dependency problems, hidden coupling, and missing seams. Prefer concrete findings and the smallest structural fix that improves the dependency graph.

## Review Order

1. **Check dependency direction.**
   Identify where UI, framework, transport, or persistence concerns pull inward on domain or application logic.

2. **Check boundary quality.**
   Look for repositories that are only pass-throughs, use cases that add no business value, DTOs leaking across layers, or giant modules that defeat the intended separation.

3. **Check test seams.**
   Verify whether business rules can be tested without platform bootstrapping and whether integration points are isolated enough for focused tests.

4. **Check change cost.**
   Ask whether adding one feature would require touching unrelated layers, or whether the current abstraction makes ordinary work harder than necessary.

## Findings Format

- Present findings first, ordered by severity.
- Include concrete file references when available.
- Explain the architectural consequence, not just the style preference.
- Recommend the smallest defensible refactor that improves the boundary.

## Decision Rules

- Do not praise layering that exists only in naming while dependencies still point the wrong way.
- Do not demand extra abstractions unless they lower coupling or increase testability.
- Accept pragmatic boundary violations only when the alternative would be disproportionate and the tradeoff is explicit.
- Call out missing tests as risk multipliers when they hide architectural regressions.

## Quality Gate

- Verify that each finding maps to a real dependency or maintainability problem.
- Verify that proposed fixes respect the existing stack and team constraints.
- Verify that the review distinguishes architectural issues from naming or formatting preferences.
