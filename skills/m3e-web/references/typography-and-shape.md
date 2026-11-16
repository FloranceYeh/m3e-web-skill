# Typography, Shape, Elevation, and Motion

Use these as design-system concepts and consult current Material guidance for normative component values. M3E may expose equivalent tokens or component options, but its API names and supported ranges are version-specific.

## Typography

The familiar Material type roles are Display, Headline, Title, Body, and Label, each with large, medium, and small styles. Expressive typography adds ways to create stronger hierarchy and personality; use those treatments selectively. Do not promote every title to display scale or sacrifice readability for character.

### Baseline Type Scale

The following is the standard Material reference scale at a 16px root size. Treat values as reference defaults; use M3E's actual theme/component typography and respect user zoom, localization, and font metrics.

| Style | Size | Line height | Weight | Typical use |
|---|---:|---:|---:|---|
| Display Large | 57px / 3.5625rem | 64px / 4rem | 400 | Rare hero/value |
| Display Medium | 45px / 2.8125rem | 52px / 3.25rem | 400 | Large display |
| Display Small | 36px / 2.25rem | 44px / 2.75rem | 400 | Display |
| Headline Large | 32px / 2rem | 40px / 2.5rem | 400 | Page headline |
| Headline Medium | 28px / 1.75rem | 36px / 2.25rem | 400 | Section headline |
| Headline Small | 24px / 1.5rem | 32px / 2rem | 400 | Dialog/section headline |
| Title Large | 22px / 1.375rem | 28px / 1.75rem | 400 | App bar title |
| Title Medium | 16px / 1rem | 24px / 1.5rem | 500 | Card/list title |
| Title Small | 14px / 0.875rem | 20px / 1.25rem | 500 | Compact title/tab |
| Body Large | 16px / 1rem | 24px / 1.5rem | 400 | Text field/body |
| Body Medium | 14px / 0.875rem | 20px / 1.25rem | 400 | Default supporting copy |
| Body Small | 12px / 0.75rem | 16px / 1rem | 400 | Dense metadata |
| Label Large | 14px / 0.875rem | 20px / 1.25rem | 500 | Button/chip |
| Label Medium | 12px / 0.75rem | 16px / 1rem | 500 | Navigation label |
| Label Small | 11px / 0.6875rem | 16px / 1rem | 500 | Compact badge |

Default tracking reference: Display Large `-0.25px`; Title Medium `0.15px`; Title Small/Label Large `0.1px`; Body Large `0.5px`; Body Medium `0.25px`; Body Small and Label Medium/Small `0.4px`/`0.5px`/`0.5px`; other styles `0`. Verify font-specific adjustments against the current spec.

| Role | Typical use |
|---|---|
| Display | A major value or rare high-level screen moment |
| Headline | Page or prominent section heading |
| Title | Card, pane, or compact section heading |
| Body | Explanatory and supporting copy |
| Label | Buttons, controls, tabs, chips, and compact metadata |

Common component mapping: button/chip label → Label Large; navigation label → Label Medium; card title → Title Medium; top app bar → Title Large; list headline → Body Large; list supporting text → Body Medium; dialog headline → Headline Small; tooltip → Body Small. Let semantics and content hierarchy win over mechanical mapping.

M3E's theme docs explicitly describe generated `--md-sys-color-*`, `--md-sys-density-scale`, and `--md-sys-motion-spring-*` tokens. Do not assume `--md-sys-typescale-*` or `--md-sys-shape-*` variables are generated unless current package docs or CEM list them. For custom HTML typography, use the actual M3E heading component or define application-owned type tokens; do not reference unverified system variables.

Use the M3E theme's documented type roles or CSS variables. Check wrapping, line length, zoom, and localization. Variable fonts such as Roboto Flex may offer optical sizing and weight axes, but font choice remains a product decision and must preserve legibility.

## Shape

Material shape scales range from square corners through small, medium, large, extra-large, and full/pill treatments; expressive styles can use increased corner sizes and stronger shape contrast. Apply shape by component role and hierarchy, not as an arbitrary page-wide radius. Use the library's supported shape choices and tokens instead of copying old Material Web component variables.

| Shape role | Reference radius | Common pattern |
|---|---:|---|
| None | 0px | Square edges where required |
| Extra-small | 4px | Compact transient surfaces |
| Small | 8px | Fields, menus, compact containers |
| Medium | 12px | Cards and standard containers |
| Large | 16px | FAB / large containers |
| Large increased | 20px | Expressive increase |
| Extra-large | 28px | Dialog/sheet scale |
| Extra-large increased | 32px | Expressive increase |
| Extra-extra-large | 48px | Large expressive surface |
| Full | 9999px | Pill / circular affordance |

The values are reference tokens; M3E components may use adaptive shape treatments and different slot-specific properties. The `m3e-button` docs, for example, document `shape="square|rounded"`; use component APIs rather than assigning a radius directly.

Shape changes can reinforce selection or interaction when clearly tied to state. Morphing is optional; keep the label and state visible, and honor reduced motion. Do not use shape alone to encode selected, error, or disabled state.

## Elevation and Surface

Use tonal surface roles to communicate layering and grouping. Shadows are supplementary and should be reserved for actual overlap, separation from busy content, or a component pattern that calls for them. Avoid adding generic shadows to every card or container. Keep overlays, menus, sheets, and dialogs distinguishable from page content.

| Elevation level | Reference height | Typical role |
|---:|---:|---|
| 0 | 0dp | Resting surface and most controls |
| 1 | 1dp | Elevated card/sheet |
| 2 | 3dp | Menus, navigation surfaces, scrolled bars |
| 3 | 6dp | FAB, dialog, picker |
| 4 | 8dp | Hover/focus increase where specified |
| 5 | 12dp | Highest interaction elevation where specified |

Treat these as conceptual reference levels, not CSS `z-index` values. Tonal surfaces provide much of the hierarchy; overlay ordering still needs a separate, documented stacking strategy in the application.

## Motion

Expressive motion favors responsive spring-like movement for stateful interactions where it improves feedback. Use M3E's documented motion system when available. For custom web transitions, use motion tokens or a restrained CSS transition, but do not describe a timed easing curve as spring physics.

Use motion to communicate entrance, exit, state, or spatial relationship. Keep repeated micro-interactions short; use more time only for larger transitions. Respect `prefers-reduced-motion: reduce` by removing nonessential movement while preserving clear state feedback.

M3E's current theme docs expose `motion="standard|expressive"` and describe spring-derived `--md-sys-motion-spring-*` tokens, using CSS `linear()` when supported and cubic-bezier approximations otherwise. Prefer that system for M3E controls. Do not override component spring tokens globally without confirming how the installed release composes them.

For custom transitions that do not use spring tokens, the Material emphasized curves are `cubic-bezier(0.2, 0, 0, 1)` (emphasized), `cubic-bezier(0.05, 0.7, 0.1, 1)` (enter/decelerate), and `cubic-bezier(0.3, 0, 0.8, 0.15)` (exit/accelerate). A typical transition reference is 400ms for enter and 200ms for exit; tune to distance, prominence, and interaction latency. These curves describe timed transitions, not spring physics.

```css
@media (prefers-reduced-motion: no-preference) {
  .custom-stateful-region {
    transition: opacity 180ms ease, transform 220ms ease;
  }
}
```

For native M3E controls, avoid overriding component motion unless there is a specific product need; global overrides can break interaction feedback and focus affordances.
