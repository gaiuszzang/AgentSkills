---
name: android-library-sdk-publisher
description: Design, evolve, test, package, and publish Android library SDKs with stable Kotlin/Java APIs, AAR metadata, manifest and resource hygiene, consumer shrinker rules, binary compatibility, and Maven consumer verification. Use when Claude Code creates or changes an Android AAR, reusable Kotlin Android library, SDK release, or sample integration. Do not use for ordinary app-only feature work.
---

# Android Library SDK Publisher

## Overview

Treat the consuming application as the real build environment. Keep the SDK's public surface small and compatible, make transitive build effects visible, and verify the published artifact rather than relying only on the library module's tests.

## Workflow

1. Define the consumer contract.
Identify Kotlin and Java callers, minimum and compile SDK expectations, supported AGP/Kotlin/JDK range, artifact coordinates, variants, initialization model, permissions, processes, and compatibility promise. Preserve existing support unless the task explicitly changes it.

2. Design the public API deliberately.
Enable or follow explicit API conventions where the project supports them. Specify public types and visibility intentionally, keep Android framework and third-party implementation types out of the surface when feasible, and inspect Java call shapes for defaults, companion members, suspend APIs, flows, and nullability.

3. Keep transitive Android effects minimal.
Review the merged manifest, permissions, components, authorities, startup providers, resources, assets, native libraries, and manifest placeholders. Namespace or prefix resources to avoid consumer collisions and make automatic initialization opt-out or explicit when it has observable startup, privacy, or process cost.

4. Classify Gradle dependencies by consumer exposure.
Use `api` only when a dependency's types are part of the supported surface and `implementation` when they are not. Keep compile-only and runtime requirements accurate, avoid dynamic versions, and ensure the published POM and Gradle Module Metadata describe the same usable graph.

5. Protect compatibility.
Compare source, binary, behavioral, resource, manifest, and serialization compatibility against the previous released artifact. Treat public inline declarations and `@PublishedApi` internals as binary surface. Prefer additive overloads or deprecation cycles over changing existing JVM signatures.

6. Prepare the release artifact.
Configure the intended publication component and variants, sources and documentation artifacts when required, Maven coordinates and metadata, AAR metadata such as minimum compile SDK where needed, and consumer shrinker rules that protect only code requiring them. Do not ship broad keep rules that disable consumer optimization.

7. Test through representative consumers.
Publish to Maven Local or an isolated repository and consume the coordinates from small Kotlin and Java fixture apps. Build debug and minified release variants at the supported lower bounds, inspect dependency resolution and merged outputs, and exercise initialization, critical APIs, process recreation, and required manifest behavior.

8. Separate preparation from external publication.
Report compatibility changes, artifact contents, dependency metadata, signing or repository requirements, and release notes. Uploading to a remote Maven repository, changing release tags, or promoting an artifact requires explicit user authorization.

## Design Rules

- Keep the library `namespace` stable and distinct from application identity; an Android library does not own an `applicationId`.
- Choose `minSdk` based on APIs and automatic manifest-triggered behavior, not merely the sample application's setting. Use guarded APIs and annotations when optional newer behavior permits a lower floor.
- Avoid exposing mutable global SDK state. Make initialization idempotent, thread-safe, testable, and clear about application-versus-component lifetime.
- Never embed secrets or environment credentials in the AAR, resources, manifest, `BuildConfig`, native library, sample, or publication metadata.
- Keep telemetry, identifiers, permissions, background work, and network behavior documented and consumer-controlled.
- Ship custom lint checks or test fixtures only when they materially improve correct integration and verify their publication metadata separately.

## Output Shape

- Show the public API and consumer integration change before broad implementation.
- Report binary API diff, AAR contents, merged manifest effects, dependency graph, and minified consumer results.
- Provide migration and deprecation guidance for any changed contract.
- Keep repository credentials and signing material out of source, logs, and generated artifacts.

## Quality Gate

- Verify the released coordinates resolve without composite-build or local project shortcuts.
- Verify Kotlin and Java consumers compile against the intended API and a previously compiled consumer remains binary-compatible when promised.
- Verify the minified consumer works with narrowly scoped consumer rules and no undocumented keep configuration.
- Verify manifest, resource, native ABI, dependency, and AAR metadata are safe for supported consumer configurations.
- Verify remote publication remains a separately authorized step after local repository and consumer checks pass.
