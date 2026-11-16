# Theming and Dynamic Color

This reference describes web theme structure and Material color concepts. Prefer M3E's documented theme component and token system for its components. Do not assume `@material/web` CSS properties or APIs work with M3E.

## Theme Architecture

Define semantic color, typography, shape, density, and motion values at an application theme boundary. Apply the theme to the document or a subtree as supported by M3E. Keep application-specific brand choices in generated/declared theme configuration rather than overriding component internals ad hoc.

M3E's `@m3e/web/theme` module documents theme support for dynamic color, expressive motion, density, and focus indicators. Read its current [theme component documentation](https://github.com/matraic/m3e/tree/main/packages/web/src/theme) for exact properties and setup. M3E and Material's specification are distinct sources: use the official pages for design decisions and M3E docs for API names.

M3E theme documentation explicitly guarantees generated Material color variables such as `--md-sys-color-primary` and `--md-sys-color-surface`. It also documents `--md-sys-density-scale` and `--md-sys-motion-spring-*`. Do not assume typography, shape, elevation, or other `--md-sys-*` families exist unless the installed release's docs/CEM declare them. For component-specific CSS properties, use the exact names in that component's API table; do not synthesize names from another library's convention.

### M3E Theme API (current repository documentation)

| Attribute | Documented values | Default / meaning |
|---|---|---|
| `color` | CSS color string | `#6750A4`; source color for generated palettes |
| `variant` | `monochrome`, `neutral`, `tonal-spot`, `vibrant`, `expressive`, `fidelity`, `rainbow`, `fruit-salad`, `content` | `tonal-spot`; generated palette style |
| `scheme` | `auto`, `light`, `dark` | `auto`; scheme selection |
| `contrast` | `standard`, `medium`, `high` | `standard` |
| `strong-focus` | Boolean | `false`; stronger focus indicators |
| `density` | Number, `-3` through `+1` | `0`; each density-aware component clamps to its supported range |
| `motion` | `standard`, `expressive` | `expressive`; spring tokens use `linear()` where supported and cubic-bezier approximations otherwise |

Use the theme element as an ancestor so custom elements inherit the theme:

```html
<body>
  <m3e-theme color="#6750A4" variant="tonal-spot" scheme="auto"
             contrast="standard" density="0" motion="expressive" strong-focus>
    <main>
      <m3e-button variant="filled">Create project</m3e-button>
    </main>
  </m3e-theme>
</body>
```

```ts
import "@m3e/web/theme";
import "@m3e/web/button";
```

When placed directly beneath `<body>`, current M3E docs say the theme also sets body background and foreground from `--md-sys-color-background` and `--md-sys-color-on-background`, plus scrollbar colors. Check global CSS interactions before adopting that placement. Theme configuration can emit a `change` event; follow its documented event shape before building a theme picker.

## Color Roles

| Role group | Roles | Typical use |
|---|---|---|
| Primary | `primary`, `on-primary`, `primary-container`, `on-primary-container` | Highest-emphasis actions and selected emphasis |
| Secondary | `secondary`, `on-secondary`, `secondary-container`, `on-secondary-container` | Supporting emphasis and selected controls |
| Tertiary | `tertiary`, `on-tertiary`, `tertiary-container`, `on-tertiary-container` | Complementary accents and contrast in the scheme |
| Error | `error`, `on-error`, `error-container`, `on-error-container` | Errors and destructive status |
| Surface | `surface`, `on-surface`, `on-surface-variant`, `surface-dim`, `surface-bright`, `surface-container-lowest` through `surface-container-highest` | Backgrounds and nested content hierarchy |
| Inverse | `inverse-surface`, `inverse-on-surface`, `inverse-primary` | Brief contrasting surfaces such as transient feedback |
| Outline | `outline`, `outline-variant` | Important boundaries versus low-emphasis separators |
| Fixed accent | `primary-fixed*`, `on-primary-fixed*`, plus secondary/tertiary equivalents | Accent roles intended to remain stable across light/dark schemes where appropriate |

### Core Role Checklist

A complete scheme normally needs paired foreground/container roles, error roles, surface hierarchy, outline roles, and inverse roles. M3E generates roles through its theme implementation; avoid maintaining a hand-copied subset that leaves components with unresolved tokens.

| Role | Use |
|---|---|
| `primary` / `on-primary` | High-emphasis action fill and its foreground |
| `primary-container` / `on-primary-container` | Prominent but less intense selected/emphasis container |
| `secondary` / `on-secondary` | Supporting accent |
| `secondary-container` / `on-secondary-container` | Tonal action and selected container |
| `tertiary` / `on-tertiary` | Complementary accent |
| `tertiary-container` / `on-tertiary-container` | Complementary emphasis container |
| `error` / `on-error` | Error affordance and foreground |
| `error-container` / `on-error-container` | Error message/status container |
| `surface` / `on-surface` | Main page surface and primary content |
| `surface-container-*` / `on-surface` | Nested content surfaces |
| `on-surface-variant` | Supporting text and lower-emphasis icons |
| `outline` | Important component boundary or focus-adjacent separation |
| `outline-variant` | Low-emphasis divider/border |
| `inverse-surface` / `inverse-on-surface` | Brief contrasting surface |
| `inverse-primary` | Action emphasis on inverse surface |

Never rely on hue alone for semantic state. Keep status meaning, icon/text, and accessible announcements consistent when the theme changes.

### Tonal Palette Model

Material schemes are generated from tonal palettes derived from source color(s), then mapped to semantic roles for a selected scheme. A traditional baseline mapping uses approximately primary tone 40 in light and 80 in dark; on-primary 100/20; primary-container 90/30; on-primary-container 10/90; surface around 98/6; and on-surface around 10/90. These are explanatory baseline values, not rules for manually writing M3E tokens. M3E's variant/contrast algorithms may choose different results.

Pair foreground and container roles as designed (for example, `primary-container` with `on-primary-container`). Use `outline` for meaningful boundaries and `outline-variant` for decorative separators. Check current official color pages for the full role set and contrast guidance.

## Seed-Based Schemes and Contrast

When a theme starts from a seed color, use a maintained Material theme generator or M3E's supported theme API instead of inventing tonal mappings. Material Color Utilities may be used for application-level scheme generation when needed, but verify that generated role names and values match the M3E theme integration. Generate light and dark schemes together. Test text, icons, focus, disabled controls, and error states in both modes.

Material supports user-controlled contrast levels in applicable scheme-generation workflows. M3E's current theme docs use named `standard`, `medium`, and `high` settings. If exposing contrast choices, use the M3E/theme mechanism supported by the installed version and verify contrast outcomes rather than assuming the setting alone guarantees accessibility.

## Web Dynamic Color

There is no standard browser API for Android wallpaper-derived Material color. Dynamic web color can come from an explicit user-selected seed, a product theme choice, or app content such as media artwork. Content-derived color is contextual, optional, and should not destabilize navigation or controls unexpectedly. Preserve contrast and provide a stable fallback when extraction fails or content changes.

## Dark Mode and Theme Switching

Support system preference with `prefers-color-scheme` when appropriate, while allowing an explicit user choice to override it. Prefer updating the theme boundary through M3E's theme API. Avoid manually setting only a few color variables if the component set expects a broader scheme.

```css
:root { color-scheme: light; }
@media (prefers-color-scheme: dark) {
  :root { color-scheme: dark; }
}
/* An explicit app setting may override the system preference. */
[data-theme="light"] { color-scheme: light; }
[data-theme="dark"] { color-scheme: dark; }
```

When using M3E, prefer setting the `scheme` attribute on the theme element instead of implementing a competing CSS-only palette. `auto` follows the library's documented automatic scheme behavior; explicit user preference should select `light` or `dark`. Persist only the user's preference, not the computed palette, so scheme and source color changes can recompute tokens.

## Brand Colors and Scoped Themes

Start with a brand seed when a coherent generated scheme is desired. Treat additional brand colors as custom roles only when the product requires them; harmonize or test them against the Material scheme and do not replace semantic error/success states with arbitrary brand colors. Scoped themes can be useful for a clearly separated region, but nested theme changes should not make the application feel like unrelated products.

If a custom brand palette is required, use this decision path:

1. Use a single seed if the brand identity can be represented by one source color.
2. Choose the M3E `variant` intentionally; compare the resulting primary, secondary, tertiary, and neutral roles rather than selecting by variant name alone.
3. Add custom colors only for distinct product concepts that do not fit the standard roles.
4. Check all foreground/container pairs, focus indicators, error states, illustrations, and data visualizations in light/dark and standard/high contrast.
5. Validate CSS theme inheritance across M3E shadow roots and any nested theme boundaries.

## Theme Audit

- [ ] Light, dark, and explicit theme preference behave consistently.
- [ ] All semantic roles used by M3E components are defined by the theme integration.
- [ ] Foreground/container pairs retain contrast in every theme.
- [ ] Dynamic/content color has a stable fallback and does not obscure state colors.
- [ ] Custom brand colors do not replace error or focus semantics.
- [ ] Theme scopes are intentional and work across nested components.
- [ ] The theme wrapper encloses every M3E component that should inherit its color, density, motion, and focus settings.
- [ ] The selected M3E theme options are supported by the locked package version.
- [ ] Global body/background/scrollbar styles do not conflict with `m3e-theme` placement.
- [ ] Pairings are semantic and no application status is conveyed by color alone.
- [ ] A changed seed/variant does not produce illegible disabled, selected, or focus states.
- [ ] Brand-specific colors are intentionally mapped and do not leave partial schemes.
