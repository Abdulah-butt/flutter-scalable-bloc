# Required presentation structure

Organize presentation by user-facing area and feature, not by Flutter type. Preserve the parent flow in the directory path.

```text
presentation/
├── pages/
│   ├── <area>/
│   │   ├── widgets/                         # reusable only inside this area
│   │   └── <feature>/
│   │       ├── <feature>_page.dart
│   │       ├── <feature>_cubit.dart
│   │       ├── <feature>_state.dart
│   │       ├── <feature>_navigator.dart
│   │       ├── <feature>_initial_params.dart
│   │       ├── widgets/                     # only for this feature
│   │       └── <subfeature>/                # a child route/flow, using the same screen unit
│   └── common/                              # routes intentionally shared by multiple areas
├── sheets/                                  # app-wide reusable modal sheets
│   └── widgets/                             # pieces shared only by those sheets
├── view_data/                               # presentation-only display/derived types
├── notifications/                           # presentation notification/deep-link handling
└── widgets/                                 # app-wide reusable UI components
```

## Screen-unit rule

Each navigable screen owns its page, Cubit, state, navigator, and typed initial params in the same folder. The page is a thin composition root: provide or retrieve the Cubit using the established pattern, select state, and assemble named components.

Do not put a complete screen, large forms, multiple sections, cards, dialogs, sheets, and list item renderers in one page file. When a visual block has its own purpose, state inputs, interaction, or would make the page difficult to scan, move it to `<feature>/widgets/<purpose>.dart`.

Prefer purpose names such as `profile_header.dart`, `address_form.dart`, `order_summary.dart`, `loading_content.dart`, or `filter_sheet.dart`; never create vague `widget1.dart`, `components.dart`, or catch-all `helpers.dart` files.

## Widget placement decision

Choose the narrowest scope that has real reuse:

1. Used by one feature: `pages/<area>/<feature>/widgets/`.
2. Used by sibling features in one area: `pages/<area>/widgets/`.
3. Used by unrelated areas: `presentation/widgets/`.
4. Used only by app-wide modal sheets: `presentation/sheets/widgets/`.

Do not promote a widget to a wider folder based on possible future reuse. Conversely, do not copy an existing shared component into a feature folder.

## Page, widget, and state boundaries

- Pages compose; widgets render a meaningful section; Cubits coordinate behavior; states contain immutable renderable values.
- Put a feature-specific dialog or bottom sheet in that feature's `widgets/` folder. Put it in `presentation/sheets/` only when multiple unrelated features use it.
- Use `presentation/view_data/` for display-only transformations or view models that should not become domain entities or API models.
- Keep route construction in `<feature>_navigator.dart` and typed incoming route values in `<feature>_initial_params.dart`; do not pass untyped maps through widgets.
- Keep controllers and form mechanics in the smallest owner. If the Cubit owns them, follow [cubit-lifecycle.md](cubit-lifecycle.md).
