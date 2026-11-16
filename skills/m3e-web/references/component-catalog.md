# Material 3 Expressive Component Catalog for Web

Use this reference for design intent and component selection. The table maps common Material component concepts to M3E's documented component families; exact element names, subpath imports, events, slots, and availability must be checked in the current [M3E documentation](https://matraic.github.io/m3e) for the installed version. A matching library component does not imply exact parity with every Google design recommendation.

## M3E Component Map

| Category | Material pattern | M3E family to check | Design use |
|---|---|---|---|
| Actions | Filled, tonal, elevated, outlined, text buttons | `button`, `button-group`, `split-button` | Choose emphasis according to action importance; normally one highest-emphasis action per decision area. |
| Actions | FAB, extended FAB, FAB menu | `fab`, `fab-menu` | A prominent, frequently used screen-level action; avoid duplicating primary actions. |
| Actions | Icon button, segmented button | `icon-button`, `segmented-button` | Compact actions with accessible names; segmented buttons select among related options. |
| Communication | Badge, progress, loading, snackbar, tooltip | `badge`, `progress-indicator`, `loading-indicator`, `snackbar`, `tooltip` | Communicate count, progress, completion feedback, or contextual help without obscuring the task. |
| Containment | Card, divider, bottom sheet, dialog | `card`, `divider`, `bottom-sheet`, `dialog` | Group related content; use modal surfaces for focused tasks that interrupt the current flow. |
| Input | Checkbox, radio, switch, slider, select, text field | `checkbox`, `radio-group`, `switch`, `slider`, `select`, `form-field` | Match the control to the value and selection model; provide visible labels and validation. |
| Input | Chips, autocomplete, date/time picker | `chips`, `autocomplete`, `datepicker`, `date-input`, `calendar`, `timepicker` | Suggestions, filtering, tokens, and structured date/time input. Verify keyboard and locale behavior. |
| Navigation | App bar, navigation bar, rail, drawer/menu, tabs, search | `app-bar`, `nav-bar`, `nav-rail`, `nav-menu`, `tabs`, `search` | Move among destinations or related peer views; see `navigation-patterns.md`. |
| Data display | List, tree, breadcrumb, paginator, table of contents | `list`, `tree`, `breadcrumb`, `paginator`, `toc` | Present structured, navigable or paginated content. |
| Layout | Content pane, split pane, toolbar, expansion panel | `content-pane`, `split-pane`, `toolbar`, `expansion-panel` | Adapt content regions and contextual actions to available space. |
| Feedback / loading | Skeleton, callout, avatar | `skeleton`, `callout`, `avatar` | Loading placeholders, contextual messages, and identity. |

This is a discovery map from the current M3E package/repository taxonomy, not a promise that every item or feature is stable in every release. Consult the per-component docs and roadmap for status.

## Verified M3E Element Examples

The following module and element names were checked against current repository component docs. They are still version-sensitive; lockfile and installed-package docs take precedence.

| Module import | Elements / important API |
|---|---|
| `@m3e/web/button` | `m3e-button`; variants `filled`, `tonal`, `elevated`, `outlined`, `text`; `size` XS–XL; rounded/square; toggle and selected state |
| `@m3e/web/icon-button` | `m3e-icon-button`; filled/tonal/outlined variants, size and width options, toggle state; provide an accessible name |
| `@m3e/web/fab` | `m3e-fab`; variants `primary`, `primary-container`, `secondary`, `secondary-container`, `tertiary`, `tertiary-container`, `surface`; sizes `small`, `medium`, `large`; optional `extended` |
| `@m3e/web/card` | `m3e-card`; filled/outlined/elevated, actionable, inline, orientation; header/content/actions/footer slots |
| `@m3e/web/form-field` | `m3e-form-field`; outlined/filled; label/hint/error slots and floating-label/required/error options |
| `@m3e/web/checkbox` | `m3e-checkbox`; checked, indeterminate, disabled, required, value |
| `@m3e/web/radio-group` | `m3e-radio-group`, `m3e-radio`; group state, required/disabled |
| `@m3e/web/switch` | `m3e-switch`; checked, icons, disabled, value |
| `@m3e/web/slider` | `m3e-slider`, `m3e-slider-thumb`; min/max/step, discrete, labelled, orientation, size |
| `@m3e/web/select` | `m3e-select`; single/multiple selection and option children |
| `@m3e/web/chips` | chip, assist, input, filter, suggestion elements and corresponding sets |
| `@m3e/web/nav-bar` | `m3e-nav-bar`, `m3e-nav-item`; compact/expanded/auto mode; selection events |
| `@m3e/web/nav-rail` | `m3e-nav-rail`, rail toggle; compact/expanded/auto mode |
| `@m3e/web/tabs` | `m3e-tabs`, `m3e-tab`, `m3e-tab-panel`; primary/secondary variant, stretch and header position |
| `@m3e/web/list` | list, list item/action/option, action/selection list, expandable list item |
| `@m3e/web/progress-indicator` | `m3e-linear-progress-indicator`, `m3e-circular-progress-indicator`; determinate, indeterminate/buffer, standard/wavy variants |
| `@m3e/web/menu` | menu, menu item, checkbox/radio menu items, item group, menu trigger |
| `@m3e/web/dialog` | dialog, dialog trigger, dialog action; alert, dismissible, open and focus-trap options |
| `@m3e/web/theme` | `m3e-theme`; color, palette variant, scheme, contrast, density, motion, strong focus |

For every module, the package README documents the full attributes, events, slots, and CSS custom properties. Use those API tables when a detail matters; the summary above intentionally omits defaults and complex event semantics.

## Actions

### Buttons

M3 button emphasis generally progresses from filled (highest), filled tonal, elevated, outlined, to text (lowest). Use a filled button for the primary action, pair lower-emphasis actions alongside it, and avoid multiple equally prominent calls to action in one small decision region. M3E documents `m3e-button` variants `filled`, `tonal`, `elevated`, `outlined`, and `text`; its button also documents sizes and shape options. Verify current APIs before use.

```html
<m3e-button variant="filled">Save</m3e-button>
<m3e-button variant="outlined">Cancel</m3e-button>
```

M3E button module: `@m3e/web/button`; custom element: `m3e-button`. Current docs list `variant="filled|tonal|elevated|outlined|text"`, `shape="square|rounded"`, and `size="extra-small|small|medium|large|extra-large"`. It supports icon/trailing-icon slots, links, disabled states, and toggle selection. Check the component API for attribute/property names and form semantics. For React, its documented binding is `M3eButton` from `@m3e/react/button`.

The button API documents a cancelable `beforeinput` preceding `input` and `change` for toggle-state changes. Its CSS custom properties follow a component-specific `--m3e-...` contract with variant and state properties. Avoid overriding state tokens casually; prefer documented `variant`, `size`, and theme settings.

### Button Groups and Split Buttons

Use a group for closely related actions or choices with a clear relationship. Use a split button when one primary action has a small set of closely associated alternatives. Verify whether the group provides selection state; if it does not, own that state in the app and expose it accessibly. Avoid treating button groups as general navigation.

### Icon and segmented buttons

Give icon-only actions an accessible name. Use segmented controls for a small set of mutually exclusive or explicitly multi-select related options, not as general navigation across unrelated destinations. Preserve selected state in both appearance and accessibility semantics.

### FAB

Use a FAB for the most important, frequent action on a screen when the content model benefits from a persistent floating action. Extended FABs pair the action label with the icon; collapse or reposition only where the layout and component support it. Avoid covering content or bottom navigation.

## Card

M3E's `@m3e/web/card` provides `m3e-card` with `variant="filled|outlined|elevated"` (filled is the documented default), `actionable`, `inline`, and `orientation` options. Slots include `header`, `content`, `actions`, and `footer`; the default slot does not add content padding. Import child components explicitly.

```html
<m3e-card variant="outlined">
  <m3e-heading slot="header" variant="title" size="medium">Quarterly report</m3e-heading>
  <p slot="content">Revenue increased compared with the prior quarter.</p>
  <div slot="actions" end>
    <m3e-button variant="text">View details</m3e-button>
  </div>
</m3e-card>
```

Use `actionable` only when the entire card has one clear primary action. Do not put independent buttons or links inside an actionable card unless the component explicitly supports nested interactive content.

## Communication and Containment

- **Progress:** Distinguish determinate progress from indeterminate activity. Provide a textual status when the visual indicator alone would not explain what is happening.
- **Snackbar:** Use for brief, non-blocking feedback and a short optional action. Do not use it for critical errors or information that must remain visible.
- **Tooltip:** Reveal concise supplementary labels on hover and keyboard focus. Do not put essential instructions only in a tooltip, especially for touch users.
- **Card:** Choose filled, outlined, or elevated treatment to create hierarchy. Keep card content and actions coherent; avoid nesting cards without a real grouping need.
- **Dialog / sheet:** Use a dialog for a focused interruption requiring a decision or input; use sheets for supplemental content and contextual tasks. Keep focus managed and ensure dismissal behavior matches the consequence of the action.
- **Divider:** Use sparingly to separate groups; spacing and surface hierarchy often provide enough separation.

## Input

- **Checkbox:** Independent or multi-select boolean choices.
- **Radio:** One choice from a short mutually exclusive set.
- **Switch:** Immediate on/off settings that take effect when changed.
- **Slider:** Approximate value selection; provide a numeric/text alternative when precision matters.
- **Chips:** Assist for a suggested action, filter for result refinement, input for user-entered tokens, suggestion for query completion. Keep selected state clear.
- **Text fields:** Always provide a label, useful supporting/error text, input type, and appropriate autocomplete behavior. Do not rely on placeholder text as the label.
- **Date/time:** Respect locale, keyboard editing, validation, timezone, and range semantics; verify the actual M3E component behavior.

### Form Field Pattern

M3E's `@m3e/web/form-field` provides the `m3e-form-field` semantic container around a native form control. The component docs show slots for `label`, `hint`, and `error`; `variant` is `outlined` or `filled`, and `float-label="always"`, `hide-subscript`, `hide-required-marker`, and `error` configure label/validation presentation. Link the label's `for` to the control id and retain native required/type/name/value behavior.

```html
<m3e-form-field variant="outlined" hide-subscript="never">
  <label slot="label" for="email">Email</label>
  <input id="email" name="email" type="email" autocomplete="email" required />
  <span slot="hint">Used for account notifications.</span>
  <span slot="error">Enter a valid email address.</span>
</m3e-form-field>
```

Use browser validation or application validation deliberately; do not display contradictory native and custom error messages. Verify when the component updates its error slot and how it sets custom validity.

### Selection Controls

M3E documents `m3e-checkbox` with checked and indeterminate states, `m3e-radio-group`/`m3e-radio` for mutually exclusive choices, and `m3e-switch` with a `checked` state. Their control APIs resemble familiar form controls but verify `name`, `value`, form association, reset, and change events before relying on native serialization.

```html
<label><m3e-checkbox name="updates" value="yes"></m3e-checkbox>Send updates</label>

<m3e-radio-group name="plan" required>
  <m3e-radio value="basic">Basic</m3e-radio>
  <m3e-radio value="pro">Pro</m3e-radio>
</m3e-radio-group>

<label><m3e-switch name="notifications" checked></m3e-switch>Notifications</label>
```

Use `switch` only for immediate settings. If a control needs an explicit Save action, a checkbox is often a clearer fit.

### Slider and Progress

M3E's `m3e-slider` supports min/max/step, discrete and labelled modes, orientation, and sizes. Associate a visible label or `aria-label`, describe units and the selected value, and provide a more precise alternative for exact values. `m3e-linear-progress-indicator` and `m3e-circular-progress-indicator` support determinate and indeterminate patterns; current docs also describe wavy variants. Use `mode="indeterminate"` for a linear indicator whose completion is unknown.

```html
<label for="volume">Volume</label>
<m3e-slider id="volume" min="0" max="100" step="1" labelled value="35"></m3e-slider>
<m3e-linear-progress-indicator value="65"></m3e-linear-progress-indicator>
```

Confirm the exact value property and event contract for the installed version; do not assume framework two-way binding is automatic.

### Menus and Tabs

M3E menus include `m3e-menu`, menu items, checkbox/radio items, groups, and a trigger. Use menu items for commands or compact option lists; use a select control for a value that belongs in a form. For tabs, use `m3e-tabs`, `m3e-tab`, and `m3e-tab-panel`; link every tab to its panel, keep one selected tab, and verify keyboard navigation and selection-change events.

### Chips

The `@m3e/web/chips` module includes `m3e-chip`, `m3e-chip-set`, assist, input, filter, and suggestion chip elements. Use a chip set to group chips. M3E documents `outlined` and `elevated` appearance variants. Filter chip sets represent filtering selection; input chip sets represent user-entered values and support editing/removal. Do not use a static chip for an action or use an action chip when users expect it to persist as a selected filter.

## Navigation and Search Controls

Use `references/navigation-patterns.md` for destination selection and responsive transformation. Additional M3E modules include `app-bar`, `nav-menu`, `nav-rail`, `search`, and `tabs`. Validate the actual component role, focus model, selected state, event contract, and link behavior before wiring it into a router. Tabs should control associated panels; nav items should represent destinations.

## Loading and Feedback

M3E includes progress and loading indicators, skeleton placeholders, callouts, snackbars, and tooltips. Choose feedback by duration and severity:

| Situation | Pattern |
|---|---|
| Known task percentage or count | Determinate progress plus textual status when context is not obvious |
| Unknown completion time | Indeterminate loading; do not invent a percentage |
| Content structure is loading | Skeleton matching final content; do not animate indefinitely |
| Brief successful/background result | Snackbar with concise message and optional reversible action |
| Persistent error or blocking issue | Inline callout or dialog based on whether the user must stop and decide |
| Supplemental control label | Tooltip on hover/focus, with the same action discoverable on touch |

## Lists and Data Display

Use list rows for related, scannable items. Reserve leading/trailing areas for meaningful icons, metadata, or controls. Keep row actions keyboard-operable and distinguish item selection from nested action buttons. For hierarchical data, use the tree pattern and preserve expected arrow-key navigation.

M3E's `@m3e/web/list` currently documents `m3e-list`, `m3e-list-item`, `m3e-list-action`, `m3e-list-option`, `m3e-action-list`, `m3e-selection-list`, and `m3e-expandable-list-item`. List item slots include leading content, overline, supporting text, and trailing content. Use action and selection list containers for their respective interaction models rather than making every row clickable.

```html
<m3e-list>
  <m3e-list-item>
    <m3e-icon slot="leading" name="folder"></m3e-icon>
    Design system
    <span slot="supporting-text">Updated 10 minutes ago</span>
    <span slot="trailing">12 files</span>
  </m3e-list-item>
</m3e-list>
```

Import `@m3e/web/list` and the icon module used by the example. Confirm icon font/registration setup in the M3E icon docs.

## Dialog Example

M3E's dialog module documents `m3e-dialog`, `m3e-dialog-trigger`, and `m3e-dialog-action`. Use an explicit title/header and label destructive actions clearly. The current API documents open/dismissible/alert and close/focus-trap options; avoid disabling Escape/backdrop dismissal unless the task truly requires the user to complete the dialog.

```html
<m3e-button variant="filled">
  <m3e-dialog-trigger for="delete-dialog">Delete project</m3e-dialog-trigger>
</m3e-button>
<m3e-dialog id="delete-dialog" alert dismissible>
  <span slot="header">Delete this project?</span>
  <p>This action cannot be undone.</p>
  <div slot="actions" end>
    <m3e-button><m3e-dialog-action return-value="cancel">Cancel</m3e-dialog-action></m3e-button>
    <m3e-button variant="filled" autofocus>
      <m3e-dialog-action return-value="delete">Delete</m3e-dialog-action>
    </m3e-button>
  </div>
</m3e-dialog>
```

Import `@m3e/web/dialog` and `@m3e/web/button`. Verify return-value handling, cancel events, focus restoration, and default focus against the installed release.

## Components Without a Matching Pattern

Use semantic HTML plus M3/M3E tokens for gaps in library coverage. Preserve the design intent, interaction states, responsive behavior, and accessibility. Do not copy `md-*` tags or CSS custom properties from `@material/web`; M3E uses its own `m3e-*` elements, modular imports, theme integration, and token names.
