# UI and platform guidance

## Design system first

Before adding colors, typography, spacing, radii, assets, or repeated controls, inspect the app theme and shared widgets. Add a semantic token when a visual meaning will recur; do not promote a one-off value without evidence of reuse.

## Responsive mobile UI

- Let parent constraints drive layout decisions. Prefer `LayoutBuilder`, `Expanded`, `Flexible`, `Wrap`, scrolling regions, and content-aware sizing to fixed dimensions.
- Verify narrow widths, long localized strings, text scaling, keyboard visibility, loading, errors, empty data, and large lists.
- Preserve visible data during an explicit refresh when the product expects continuity; do not replace known layout with an unrelated full-page spinner.
- Use skeletons for known content shapes. Use indeterminate progress only when there is no meaningful final layout to represent.

## Native behavior

- Use Material conventions on Android and Cupertino conventions on iOS when the interaction is platform-native, such as date/time pickers, navigation transitions, dialogs, or action sheets.
- Put platform selection behind a service/factory when it is reused or affects business flows. Do not scatter platform checks through feature widgets.
- Respect reduced motion, platform back behavior, safe areas, accessibility labels, focus order, and adequate touch targets.

## File size and reuse

Keep a page readable by extracting a meaningful feature-only section. Do not split tiny widgets merely to meet an arbitrary line count. Prefer an existing shared widget or extension before adding a near-duplicate.
