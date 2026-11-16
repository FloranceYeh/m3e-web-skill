# M3E Web Components

M3E is a third-party Material 3 Expressive Web Components project. Its repository is a monorepo with these public packages:

| Package | Role | Use |
|---|---|---|
| `@m3e/web` | Native custom elements | Use from vanilla JS/TS and non-React frameworks. |
| `@m3e/react` | React bindings | Use in React projects where the installed package exports the needed component. |
| `@m3e/icons` | SVG icon assets | Register individual or family icons from this package when using M3E's SVG icon registry. |
| `@m3e/mcp` | MCP server | Optional development integration; not a UI runtime dependency. |

Read the package documentation before adding dependencies or copying imports. Check version compatibility between `@m3e/web` and `@m3e/react` in the project lockfile and current package documentation. Keep icon names distinct: `@m3e/web/icon` is the bundler module defining `m3e-icon`; `@m3e/icons` is the published SVG asset package; `@m3e/icon` is an import-map alias used by M3E browser examples, not the SVG package name.

### Source and Version Notes

The package/API examples in this reference were checked against M3E's public component and getting-started documentation on 2026-11-16. The hosted docs loaded `@m3e/web@2.12.1` in their examples at that time. This records the evidence behind the examples; it is not a minimum version or a claim that every current installation uses 2.12.1. For an implementation, resolve the installed package version from the lockfile and use its bundled CEM and package documentation.

Primary implementation references:

- [M3E installation and editor integration](https://matraic.github.io/m3e/getting-started/installation)
- [M3E framework integration](https://matraic.github.io/m3e/getting-started/frameworks)
- [M3E button component](https://matraic.github.io/m3e/components/button)
- [M3E theme component](https://matraic.github.io/m3e/components/theme)
- [M3E icon component](https://matraic.github.io/m3e/components/icon)
- [M3E Web package exports and CEM declaration](https://github.com/matraic/m3e/blob/main/packages/web/package.json)
- [M3E SVG icon package](https://github.com/matraic/m3e/tree/main/packages/icons)

## Installation and Imports

Install only the packages the application uses:

```bash
npm install @m3e/web
```

For React, use the documented React package and its peer/dependency requirements:

```bash
npm install @m3e/react
```

Import the component entry points needed by the screen. Avoid an all-components import unless the application intentionally needs the full set. Follow the current docs for registration, SSR/hydration, CSS, and framework integration; custom elements must be registered before use in environments that require explicit registration.

The exact subpath exports and JSX APIs evolve. Treat examples below as package discovery hints, not guaranteed import paths:

```tsx
"use client";

import { M3eButton } from "@m3e/react/button";

export function SaveAction() {
  return <M3eButton variant="filled">Save</M3eButton>;
}
```

The React bindings are client-only. In Next.js, place them inside a `"use client"` boundary.

```ts
import "@m3e/web/button";
```

```html
<m3e-button variant="filled">Save</m3e-button>
```

M3E components use `m3e-*` custom-element names. The Web package is modular; import the module for the element being used (for example, `@m3e/web/button`) and consult its component reference for the complete setup and supported attributes.

### No-Bundler Setup

The package is built on Lit and its ESM modules have bare-specifier dependencies. Installing `@m3e/web` alone does not make those imports resolvable in a browser. With a bundler, let the bundler resolve package dependencies. Without a bundler, add an import map before module scripts and map the Lit dependencies plus every M3E entry point being imported. M3E's installation guide provides this core-entry example; update package/CDN versions together with the installed release:

```html
<script type="importmap">
{
  "imports": {
    "tslib": "https://cdn.jsdelivr.net/npm/tslib@2.8.1/+esm",
    "lit": "https://cdn.jsdelivr.net/npm/lit@3.3.0/+esm",
    "lit/": "https://cdn.jsdelivr.net/npm/lit@3.3.0/",
    "lit-html": "https://cdn.jsdelivr.net/npm/lit-html@3.3.0/+esm",
    "lit-html/directive.js": "https://cdn.jsdelivr.net/npm/lit-html@3.3.0/directive.js",
    "lit-html/directives/if-defined.js": "https://cdn.jsdelivr.net/npm/lit-html/directives/if-defined.js",
    "lit-html/directives/class-map.js": "https://cdn.jsdelivr.net/npm/lit-html/directives/class-map.js",
    "@lit/reactive-element": "https://cdn.jsdelivr.net/npm/@lit/reactive-element@2.0.4/+esm",
    "@lit/reactive-element/": "https://cdn.jsdelivr.net/npm/@lit/reactive-element@2.0.4/",
    "@m3e/web/core": "/node_modules/@m3e/web/dist/core.js",
    "@m3e/web/core/a11y": "/node_modules/@m3e/web/dist/core-a11y.js",
    "@m3e/web/core/anchoring": "/node_modules/@m3e/web/dist/core-anchoring.js",
    "@m3e/web/core/bidi": "/node_modules/@m3e/web/dist/core-bidi.js",
    "@m3e/web/core/layout": "/node_modules/@m3e/web/dist/core-layout.js",
    "@m3e/web/core/platform": "/node_modules/@m3e/web/dist/core-platform.js",
    "@m3e/web/button": "/node_modules/@m3e/web/dist/button.js",
    "@m3e/web/theme": "/node_modules/@m3e/web/dist/theme.js",
    "@m3e/web/icon": "/node_modules/@m3e/web/dist/icon.js",
    "@m3e/icon": "/node_modules/@m3e/web/dist/icon.js"
  }
}
</script>
```

This is the documented core map, not a universal map for every component. Map all bare imports required by the selected component entry points. The docs also show `dist/all.min.js` for browser-only use; prefer modular entry points where appropriate. Import maps must precede module scripts that use them.

The alias distinction matters in this example: `@m3e/icon` maps to the `m3e-icon` element module in browser documentation, while a bundler import uses `@m3e/web/icon`. SVG assets are registered from `@m3e/icons`. These names serve different purposes and are not interchangeable.

For SVG icons, import the element module separately and register assets with the actual `@m3e/icons` package, for example `@m3e/icons/outlined/search`. For Material Symbols fallback, follow `m3e-icon` docs. Never use `@m3e/icon` as an npm install command.

## Theme Integration

M3E documents `m3e-theme` as the application-level theme boundary. It applies dynamic color, expressive motion, density, and strong focus indicators to nested theme-aware elements. Supported attributes in the current repository documentation include `color`, `variant`, `scheme`, `contrast`, `strong-focus`, `density`, and `motion`; check the installed release before relying on exact enum values.

```html
<m3e-theme
  color="#6750A4"
  variant="tonal-spot"
  scheme="auto"
  contrast="standard"
  density="0"
  motion="expressive"
  strong-focus>
  <main class="app-shell">
    <m3e-button variant="filled">Create</m3e-button>
  </main>
</m3e-theme>
```

Import the theme module and each used component module. If `m3e-theme` is nested directly beneath `<body>`, M3E documents behavior that sets body background/text colors and viewport scrollbar colors. Confirm this is compatible with the host app's global styles. Nested themes can scope an alternate scheme; keep boundaries deliberate and test shadow-DOM descendants.

M3E's documented `scheme` options include `auto`, `light`, and `dark`; `contrast` includes `standard`, `medium`, and `high`; `motion` includes `standard` and `expressive`. Its theme `variant` controls generated palette style (for example `tonal-spot`, `expressive`, `vibrant`, `neutral`, `monochrome`, `fidelity`, `rainbow`, `fruit-salad`, and `content`). Theme density accepts `-3` through `+1`, but individual components clamp values to their own supported ranges. For a documented example, a medium `m3e-button` clamps the minimum to `-2`; values `-3` and `-2` render identically for that size. Density-aware components preserve their documented minimum touch target. Check the exact component docs rather than assuming the theme's full range applies to every size.

Prefer the theme boundary over hand-authoring incomplete role sets. Use semantic roles in custom layout CSS only when verified in the actual M3E token contract; the library may include legacy and additional roles. Do not assume that `@material/web` component variables are accepted by M3E.

```css
/* Verify role availability and token names against the installed M3E version. */
.app-shell {
  color: var(--md-sys-color-on-surface);
  background: var(--md-sys-color-surface);
}

.compact-density {
  --md-sys-density-scale: -3;
}
```

Component custom properties are API surface. Read that component's CSS property table before overriding one; prefer changing the theme role to changing a single state-specific component token. Avoid reaching into shadow DOM with selectors or global `!important` rules.

## Framework and Rendering Notes

- **React:** `@m3e/react` provides typed bindings with ref forwarding. Import per component (for example `@m3e/react/button`); the documented `M3eButton` binding is client-only. In Next.js it must be rendered below a `"use client"` boundary. Custom element event props and JSX type declarations should follow that binding's docs.
- **Vanilla JS/TS:** import individual `@m3e/web/<component>` modules (for example `@m3e/web/button`), then use the registered `m3e-*` elements. Avoid importing every module unless the application uses all of them.
- **Vue/Svelte/Angular:** use `@m3e/web` custom elements and configure the framework compiler to treat the elements as custom elements where required. Bind DOM properties/events using that framework's custom-element conventions; test event payloads and boolean attributes.
- **SSR:** custom elements need client registration and may upgrade after initial HTML parse. React bindings are client-only per package documentation. Use framework-supported client boundaries/islands where necessary; avoid assuming server rendering, hydration, or no-JS behavior that has not been verified.
- **Next.js:** keep `@m3e/react` bindings in a `"use client"` module as required by the React package docs. For direct `@m3e/web` custom elements, isolate registration and DOM-dependent setup in a client component. Do not import browser-only modules across a server-rendered boundary unless the installed release explicitly supports it; verify that server and initial client markup match before custom-element upgrade.
- **Forms:** verify native form association, name/value serialization, validation, reset, disabled behavior, and submit semantics for each component. Do not infer these from visual similarity to a native control.
- **Events:** custom elements may emit native or custom DOM events. In framework wrappers, confirm event name casing, cancellation, bubbling/composition, and whether state is controlled or internally managed.

## Editor Tooling: Custom Elements Manifest

`@m3e/web` publishes `dist/custom-elements.json` (CEM), describing element names, attributes, properties, slots, events, and CSS custom properties. It is generated package metadata for editor tooling, not a runtime import.

The M3E installation guide recommends the Custom Elements Manifest Language Server extension for VS Code; it detects the installed package manifest and provides tag/attribute completion and hover documentation. The documented alternative is to point VS Code at the package custom-data files:

```json
{
  "html.customData": ["./node_modules/@m3e/web/dist/html-custom-data.json"],
  "css.customData": ["./node_modules/@m3e/web/dist/css-custom-data.json"]
}
```

Use CEM/custom-data from the installed package version so suggestions match the code being built. Keep wrapper types and metadata aligned; do not edit generated files under `node_modules`.

## Component Discovery

The repository documents a broad component set, including actions, app bars, buttons and button groups, FAB/FAB menu, cards, chips, dialogs, menus, form controls, date/time pickers, lists, navigation bar/menu/rail, search, segmented and split buttons, panes, tabs, tooltips, snackbars, progress/loading, skeletons, tree, and gesture-related components. This is a discovery list, not a guarantee that every component is stable or available in every package/version.

For each requested component:

1. Check the live M3E docs and package exports for the installed version.
2. Confirm whether the example uses `@m3e/web` custom elements or `@m3e/react` bindings.
3. Verify required child components, slots/props, events, controlled state, and accessibility behavior.
4. Check whether the current M3 Expressive design calls for behavior that the selected component does not implement. Add only the missing behavior needed by the screen.

Reference the [M3E showcase and documentation](https://matraic.github.io/m3e), [repository](https://github.com/matraic/m3e), [React package](https://github.com/matraic/m3e/tree/main/packages/react), and [Web Components package](https://github.com/matraic/m3e/tree/main/packages/web). The public docs are the API source; do not copy assumptions from Material Web (`@material/web`) or invent `md-*` elements when an M3E component is available.

## Framework-Neutral Use

For Vue, Svelte, Angular, or other web frameworks, use the native `@m3e/web` custom elements when supported by the framework and project configuration. Adapt event naming, property binding, and SSR registration to that framework. Keep layout, routing, form state, and application data in the host framework; use M3E for design-system controls rather than introducing a second state model.

When no matching M3E component exists, use semantic HTML and CSS driven by M3/M3E tokens. Preserve keyboard operation, form semantics, focus behavior, and accessible names. Do not create a custom component solely to mimic a missing visual detail if native controls already meet the need.
