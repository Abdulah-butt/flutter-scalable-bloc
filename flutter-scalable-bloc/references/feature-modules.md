# Feature modules

## Default shape

For a navigable feature, use the existing project's equivalent of:

```text
presentation/pages/<feature>/
  <feature>_page.dart
  <feature>_cubit.dart
  <feature>_state.dart
  <feature>_navigator.dart
  <feature>_initial_params.dart
  widgets/                 # only when feature-only UI is substantial
```

This is a default, not a ceremony requirement. A trivial route may omit pieces that have no job.

## Responsibilities

- **Page:** compose widgets, provide the Cubit, react to state, route user events.
- **Cubit:** validate input, call use cases/repositories, transition explicit state, request navigation or feedback through established abstractions.
- **State:** immutable view state; expose loading, data, error, selection, filters, and pagination state only when they affect rendering.
- **Navigator:** feature-specific navigation intent; prevents route construction from spreading through widgets and Cubits.
- **Initial params:** typed route arguments with a single serialization boundary when routing needs one.

## Extraction rules

- Reuse a shared widget when the UI and behavior genuinely recur across features.
- Keep feature-specific sections under that feature's `widgets/` directory.
- Extract duplication in Cubits into a use case, service, store, or extension based on responsibility—not into a vague `helpers` file.
- Keep enum API values, labels, and semantic display metadata centralized. Do not repeat status switches and color literals across widgets.
