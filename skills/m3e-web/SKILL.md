---
name: m3e-web
description: >
  Design and implement Material 3 Expressive interfaces for the web. Use M3E
  (matraic/m3e) Web Components as the primary component implementation, with
  React bindings where useful; adapt the same design guidance to other web
  frameworks. Covers M3 Expressive tokens, components, responsive layouts,
  theming, motion, and accessibility. Use when: "M3 Expressive web", "M3E",
  "Material 3 Expressive website", or a web UI specifically using m3e.
user-invokable: true
argument-hint: "[component|theme|layout|scaffold|audit] [description or URL]"
---

# Material 3 Expressive for Web

Use this skill to design, implement, or review web interfaces following Material 3 Expressive (M3E). Apply Google's design guidance to web platform conventions and use [M3E](https://github.com/matraic/m3e) as the preferred component library. M3E is an independent project, not a Google product; its API coverage is not the same thing as official design guidance.

## Source of Truth

Keep two questions separate:

1. **What should the interface do and look like?** Follow the current [Material Design updates](https://m3.material.io/blog/whats-new-at-io26), the [M3 Expressive introduction](https://m3.material.io/blog/whats-new-at-io25), and the relevant official pages linked from `references/expressive-guidelines.md`.
2. **What can the chosen package render and how is it configured?** Follow the current [M3E documentation](https://matraic.github.io/m3e) and `references/m3e-web-components.md`.

Do not describe an M3E implementation detail as an official Google requirement. When the design calls for a pattern M3E does not provide, compose it from available M3E components and semantic HTML/CSS, or explain the gap. Check the installed package version and current docs before relying on an API, token, component, or prop; do not assume repository `main` matches the user's installed version.

## Workflow

1. Identify the target framework, package version, screen classes, and interaction requirements. If the project already has a stack, follow it.
2. Choose a Material layout and component pattern before styling individual controls. Read `references/web-layout-and-accessibility.md` for responsive shells, panes, and interaction considerations.
3. Use M3E Web Components by default. Use `@m3e/react` bindings in React projects when they fit the installed version; use `@m3e/web` directly in other frameworks. Keep framework-neutral guidance in semantic HTML, CSS, ARIA, and design tokens.
4. Apply semantic system color, typography, shape, elevation, and motion tokens. Prefer the M3E theme APIs and tokens supported by the installed version. Read `references/expressive-guidelines.md` for normative design guidance and `references/m3e-web-components.md` for library setup and component coverage.
5. Preserve native keyboard behavior, focus visibility, reduced-motion preferences, contrast, and touch target requirements. Avoid replacing semantic controls with decorative custom elements.
6. Validate the actual target breakpoints, theme modes, interaction states, keyboard flow, and reduced-motion mode. For an audit, use the checklists in the references.

## Decision Tree

| Need | Start here |
|---|---|
| New app shell or responsive navigation | `references/navigation-patterns.md` + `references/web-layout-and-accessibility.md` |
| One component or form | `references/component-catalog.md`; verify M3E props/events/slots in `references/m3e-web-components.md` |
| Brand theme, dark mode, contrast or content-derived color | `references/theming-and-dynamic-color.md` |
| Type, shape, elevation or motion treatment | `references/typography-and-shape.md` + `references/expressive-guidelines.md` |
| Audit an existing screen or repo | Compliance Review below + `references/web-layout-and-accessibility.md` audit |

## Design Tokens

Treat the following as semantic Material role names. M3E's `m3e-theme` generates and supplies its supported tokens to nested theme-aware components; custom CSS should consume the installed release's token contract rather than redefining roles from memory.

| Token family | Roles / scales | Primary use |
|---|---|---|
| Color | `primary`, `on-primary`, `primary-container`, `on-primary-container`; equivalent `secondary`, `tertiary`, and `error` pairs | Actions, selection, semantic status |
| Surface | `surface`, `surface-dim`, `surface-bright`, `surface-container-lowest/low/default/high/highest`, `on-surface`, `on-surface-variant` | Page and nested surface hierarchy |
| Other color | `outline`, `outline-variant`, inverse roles, fixed accent roles | Boundaries, separators, contrast surfaces, stable accents |
| Typography | Display / Headline / Title / Body / Label, Large / Medium / Small | Content hierarchy, controls, metadata |
| Shape | none, extra-small, small, medium, large, large-increased, extra-large, extra-large-increased, extra-extra-large, full | Component and container geometry |
| Elevation | levels 0–5, expressed by tonal surfaces and selective shadow | Layering and overlap |
| Motion | standard or expressive spring scheme; transition behavior and reduced-motion fallback | State changes, feedback, spatial continuity |
| Density | M3E theme accepts `-3` through `+1`; components clamp to their supported range | Compactness where the interface supports it; verify the component size's effective range |

Do not assume that every Material role is emitted by every M3E version or that a system role name equals a CSS custom property. Use the exact names documented by the current theme/component API. Details and examples: `references/theming-and-dynamic-color.md` and `references/typography-and-shape.md`.

## Component Quick Reference

| Material pattern | M3E module family | Key selection |
|---|---|---|
| Buttons / button group / split button | `button`, `button-group`, `split-button` | Filled, tonal, elevated, outlined, text; confirm group selection behavior |
| FAB / FAB menu / icon button | `fab`, `fab-menu`, `icon-button` | Screen-level primary action; icon-only controls need names |
| Cards / dialog / bottom sheet | `card`, `dialog`, `bottom-sheet` | Content grouping, focused interruption, supplemental task |
| Checkbox / radio / switch / slider | `checkbox`, `radio-group`, `switch`, `slider` | Match control to independent, exclusive, immediate, or approximate choice |
| Text / select / date / time | `form-field`, `select`, `date-input`, `datepicker`, `calendar`, `timepicker` | Labels, validation, locale, keyboard and form behavior |
| Chips / autocomplete | `chips`, `autocomplete` | Assist, filter, input, suggestion, completion |
| Navigation / search / tabs | `app-bar`, `nav-bar`, `nav-rail`, `nav-menu`, `search`, `tabs` | Destination navigation versus in-page peer views |
| List / tree / progress / feedback | `list`, `tree`, `progress-indicator`, `loading-indicator`, `snackbar`, `tooltip` | Scanning, hierarchy, status, transient feedback |
| Responsive layout primitives | `content-pane`, `split-pane`, `expansion-panel` | Panes, resizing, progressive disclosure |

This is a family-level routing table, not a complete API index. See `references/component-catalog.md` for design criteria and `references/m3e-web-components.md` for package usage.

## Platform Boundary

This skill covers browser-based interfaces only: HTML, CSS, JavaScript/TypeScript, Web Components, React, and other web frameworks. Do not route to Compose or Flutter APIs. On the web, use CSS pixels and responsive/container queries; do not present Android window APIs or device-only dynamic wallpaper color as web capabilities.

## Anti-Patterns

- Do not use raw hex colors throughout components when semantic theme roles are available.
- Do not mix Material 2 patterns, Material Web (`md-*` tags), and M3E (`m3e-*` tags) in one implementation without a clear compatibility reason.
- Do not assert parity with official M3 Expressive from a library name or marketing description; verify the actual component and version.
- Do not use color alone for selected, error, focus, or disabled state.
- Do not add generic shadows, nested cards, large corner radii, or expressive motion as decoration without a hierarchy or interaction purpose.
- Do not use a switch for a choice that requires a separate submit step, or tabs for unrelated app destinations.
- Do not hide essential actions behind hover, omit labels from navigation without an accessible name, or use placeholder text as the only form label.
- Do not stretch text-heavy content across wide windows or let fixed navigation obscure content.
- Do not remove focus indicators, keyboard behavior, or reduced-motion support when customizing components.

## Compliance Review

When asked to audit a screen or application, inspect design intent and implementation separately. Read `references/expressive-guidelines.md` and the relevant token/component/layout references. Cover color and theme roles, typography, shape, elevation/surfaces, component choice and states, layout/adaptivity, navigation, motion, accessibility, and framework/library integration. For each finding, include severity, file/component or viewport, observed behavior, expected behavior/source, user impact, and a concrete correction. Separate confirmed mismatches from library/version uncertainties. Lead with blocking issues, then warnings and passes. Do not assign a numerical compliance score unless the user asks for one and the rubric is explicit.

For source audits, inspect imports, theme boundaries, custom-element usage, raw color/shape overrides, responsive CSS, interaction state wiring, and forms/events. For live pages, use available browser tools to inspect computed styles, keyboard/focus behavior, theme variants, and responsive states at the target widths. If only source is available, state that visual/runtime behavior was not verified. Report:

```markdown
## Findings
- [Critical|High|Medium|Low] `path:line` or component / viewport — behavior, impact, and fix.

## Coverage
- Color/theme: pass | findings
- Typography/shape/elevation: pass | findings
- Components/interactions: pass | findings
- Layout/navigation: pass | findings
- Motion/accessibility: pass | findings

## Verification Limits
- What was inspected and which runtime states could not be exercised.
```

Severity guide: Critical blocks a primary task or causes serious accessibility/data-loss risk; High breaks a core flow or key viewport; Medium causes a material inconsistency or friction; Low is a localized polish/spec mismatch. Cite official design pages for normative expectations and M3E docs for library behavior.

## Quick Routing

| Task | Reference |
|---|---|
| Expressive principles, typography, shape, elevation, motion | `references/expressive-guidelines.md`, `references/typography-and-shape.md` |
| Color roles, theme generation, dark mode, dynamic/content color | `references/theming-and-dynamic-color.md` |
| Install/import M3E, package choice, component inventory, API examples | `references/m3e-web-components.md`, `references/component-catalog.md` |
| Navigation selection, bars, rail, drawer, tabs, app bar | `references/navigation-patterns.md` |
| Breakpoints, canonical layouts, panes, accessibility and audit | `references/web-layout-and-accessibility.md` |

For component-specific requirements, prefer the official component page for design intent and the matching M3E documentation page for API details. Examples in this skill are illustrative; compile and test against the project's installed versions.
