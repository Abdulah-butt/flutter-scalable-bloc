# Feature modules

## Default shape

For every navigable feature, use this screen unit:

```text
presentation/pages/<feature>/
  <feature>_page.dart
  <feature>_cubit.dart
  <feature>_state.dart
  <feature>_navigator.dart
  <feature>_initial_params.dart
  widgets/                 # feature-only visual sections, cards, forms, dialogs, and sheets
```

Keep nested subfeatures under their parent feature, for example `pages/<area>/<feature>/<subfeature>/`. Do not flatten related flows into unrelated top-level page folders.

## Responsibilities

- **Page:** provide the Cubit, compose named widgets, react to state, and route user events. It is not a home for a full screen's widget tree.
- **Cubit:** validate input, call use cases, transition explicit state, request navigation or feedback through established abstractions.
- **State:** immutable view state; expose loading, data, error, selection, filters, and pagination state only when they affect rendering.
- **Navigator:** feature-specific navigation intent; prevents route construction from spreading through widgets and Cubits.
- **Initial params:** typed route arguments with a single serialization boundary when routing needs one.

## Extraction rules

- Reuse a shared widget when the UI and behavior genuinely recur across features.
- Keep feature-specific sections under that feature's `widgets/` directory.
- Extract duplication in Cubits into a use case, service, store, or extension based on responsibility—not into a vague `helpers` file.
- Keep enum API values, labels, and semantic display metadata centralized. Do not repeat status switches and color literals across widgets.

Read [presentation-structure.md](presentation-structure.md) before deciding whether a widget belongs to a feature, area, or app-wide folder.
