---
name: flutter-scalable-bloc
description: Build or refactor Flutter apps with a feature-oriented layered architecture using Cubit/BLoC, repository contracts, dependency injection, and platform services. Use for scalable Flutter features; do not use for small throwaway prototypes or when an existing project deliberately uses another architecture.
---

# Flutter Scalable Bloc

Build the smallest architecture that supports the product. Preserve the host app's established conventions when they conflict with this skill.

## First inspect

Before adding files or dependencies, inspect the existing app's `pubspec.yaml`, `lib/` structure, dependency registrations, router, theme, and closest feature. Reuse an established abstraction or widget before creating one.

Use this architecture when the app has multiple features, remote/local data, device capabilities, or a long maintenance horizon. A simple static screen does not need every layer or a Cubit.

For every network-backed feature or change to the app structure, read [project-structure.md](references/project-structure.md) first. Its folder layout and request flow are the required convention unless the host project has already established a deliberate compatible variation.

## Core boundaries

- Use this request path for feature behavior: `UI -> Cubit/BLoC -> Use case -> Repository contract -> Repository implementation -> network/service`.
- Results return through the same boundaries in reverse. The UI observes Cubit state only; it never receives DTOs, database rows, HTTP responses, or plugin types.
- Presentation renders and wires interactions. Feature Cubits own page state and orchestration; widgets do not call HTTP clients, databases, or platform plugins directly.
- Domain contains business entities, repository contracts, cross-feature stores, and reusable use cases. It does not depend on Flutter persistence or transport implementations.
- Data implements repository contracts and maps DTOs to domain entities. Keep API/JSON and database representations out of presentation and domain entities.
- Network owns request execution, endpoint definitions, interceptors, and transport concerns. Never scatter endpoint strings or direct Dio calls through features.
- Services wrap device and platform integrations behind small contracts. Register implementations through dependency injection.

Read [layering.md](references/layering.md) before introducing a repository, use case, service, or cross-feature state.
Read [network-and-session.md](references/network-and-session.md) before adding an endpoint, authentication/context behavior, a retryable mutation, or offline network handling.

## Feature work

For a navigable feature, first follow the closest existing feature. The normal module is a page, Cubit, state, navigator, and initial parameters. Split feature-only UI into its local `widgets/` directory when the page becomes difficult to scan.

- Keep route paths and argument serialization centralized.
- Keep Cubit state explicit, immutable, and specific to visible UI needs.
- Give every repository operation a dedicated domain use case. Cubits depend on use cases, not repositories.
- Promote state to a domain store only when independent features must observe or mutate it.

Read [feature-modules.md](references/feature-modules.md) for a new screen or feature.
Read [cubit-lifecycle.md](references/cubit-lifecycle.md) when a Cubit owns controllers, streams, timers, debounced work, search, refresh, or pagination.

## UI and platform behavior

Use theme tokens, shared spacing, typography, colors, radii, assets, and reusable widgets rather than literal values or duplicated widget trees. Build for constrained mobile screens first; use flexible layouts and test long text, loading, error, empty, and small-screen states.

Use a service contract for device-backed behavior such as permissions, pickers, connectivity, secure storage, notifications, files, or location. Choose Material or Cupertino interaction patterns according to the target platform and established app design rather than imitating one platform everywhere.

Read [ui-and-platform.md](references/ui-and-platform.md) when changing UI, theming, responsiveness, or device behavior.

## Remote data, local data, and offline behavior

Define the repository contract before its implementation. Preserve server-provided opaque cursors. Add durable local storage, caching, or offline behavior only when product requirements justify it; keep it behind repository contracts.

For offline-first or multi-tenant features, read [data-and-offline.md](references/data-and-offline.md) before implementation.

## Quality gate

- Do not add a package until platform APIs and installed dependencies have been considered.
- Remove duplication by extracting a shared widget, extension, use case, store, or service only after confirming the reuse is real.
- Keep files focused; extract a meaningful section rather than fragmenting trivial code.
- Run the project's formatter, analyzer, tests, and relevant integration tests after changes.
- State exactly what was verified and any setup a developer must still perform.
