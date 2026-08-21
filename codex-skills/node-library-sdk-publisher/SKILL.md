---
name: node-library-sdk-publisher
description: Design, evolve, package, and release Node.js or TypeScript libraries and client SDKs with stable public exports, declarations, runtime compatibility, dependency metadata, consumer tests, and safe npm publishing. Use when Codex creates or changes a reusable package, generated client, monorepo package, or npm release. Do not use for an application that is not consumed as a library.
---

# Node Library SDK Publisher

## Overview

Treat the installed package, not its source tree, as the product. Keep the public import surface intentional, make runtime and type declarations agree, and verify the packed artifact in representative consumers before publishing.

## Workflow

1. Define the consumer contract.
Identify supported runtimes, module systems, package managers, TypeScript versions, browser or server environments, public entry points, and compatibility promise. Preserve the existing support matrix unless the task explicitly changes it.

2. Design exports before build output.
Expose only supported entry points through `exports` and align `types`, `main`, `module`, or conditional declarations with the selected compatibility strategy. Avoid new deep-import paths and do not introduce dual ESM/CJS output without checking singleton identity, side effects, and conditional export behavior.

3. Classify dependencies by consumer ownership.
Use regular dependencies for runtime requirements shipped by the package, peer dependencies for host-owned frameworks or singleton ecosystems, optional dependencies only for genuinely optional capabilities, and development dependencies for build or test tooling. Keep peer ranges broad enough for verified compatibility and fail clearly when an optional capability is unavailable.

4. Protect the public API.
Keep exported types free of private build paths and accidental third-party implementation types. Treat entry-point changes, declaration changes, default-versus-named exports, thrown errors, asynchronous behavior, and side effects as compatibility concerns, not only TypeScript compilation concerns.

5. Build a deterministic artifact.
Ensure the published `files` set contains runtime code, declarations, source maps if intended, licenses, and required assets while excluding tests, secrets, local configuration, and workspace-only files. Make generated code reproducible and keep release metadata derived from one version source where possible.

6. Validate as a consumer.
Inspect `npm pack --dry-run` or the equivalent package-manager output, create a tarball, and install that tarball into isolated fixture consumers. Exercise supported import styles and runtimes, TypeScript resolution, tree-shaking or side-effect metadata where relevant, and the minimum supported environment.

7. Prepare the release without assuming permission to publish.
Classify the change under the project's versioning policy, update release notes and migration guidance, and inspect registry, tag, access, provenance, and authentication settings. Run the actual publish or dist-tag mutation only when the user explicitly authorizes that external change.

## Design Rules

- Prefer one module format when consumer requirements allow it; compatibility output must solve a demonstrated need.
- Keep subpath exports explicit and stable. Adding `exports` to an existing package can break undocumented deep imports, so audit consumers before tightening the boundary.
- Keep package initialization free of network calls, process termination, global mutation, and heavyweight eager work.
- Do not bundle host frameworks or dependencies that consumers must share unless isolation is intentional and tested.
- Preserve source maps and declaration maps only when their paths resolve correctly from the packed artifact.
- For generated SDKs, separate generated transport models from ergonomic public adapters and document regeneration ownership.

## Output Shape

- Show the public import surface and supported environment matrix when changing package structure.
- Report packed contents, artifact size, entry-point resolution, and fixture-consumer results.
- Call out breaking changes and provide a migration path before release.
- Keep registry credentials and one-time passwords out of commands, logs, and repository files.

## Quality Gate

- Verify runtime exports and `.d.ts` declarations resolve to matching APIs from the packed artifact.
- Verify every declared dependency class matches who owns and loads it at runtime.
- Verify supported ESM/CJS, Node, TypeScript, and browser combinations proportionally to the published support matrix.
- Verify the tarball contains no secret, unpublished source, workspace link, or undeclared runtime dependency.
- Verify publishing remains a separately authorized step after all local and dry-run checks pass.
