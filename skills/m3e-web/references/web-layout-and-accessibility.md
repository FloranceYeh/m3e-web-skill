# Web Layout and Accessibility

Use responsive web layouts informed by Material's adaptive guidance. The exact breakpoint should follow the content and available M3E layout primitives; the values below are practical starting points for CSS, not a device taxonomy.

## Responsive Structure

Use content-driven breakpoints. A practical starting map is compact below 600 CSS px, medium from 600 to 839px, expanded from 840 to 1199px, large from 1200 to 1599px, and extra-large from 1600px. These are design reference points, not mandated device categories; adjust when navigation, panes, or content need a different transition. Prefer container queries for reusable components whose available width differs from the viewport.

Material's 2026 adaptive update calls for spacing tokens around an 8dp layout rhythm. The familiar web layout examples also use a 4px base increment for component-level spacing. Keep the app's spacing scale tokenized; use 8px multiples for major layout rhythm where suitable and smaller 4px increments for fine component spacing when the chosen component spec calls for it.

| Window class | Columns | Typical margin | Typical gutter |
|---|---:|---:|---:|
| Compact | 4 | 16px | 8px |
| Medium | 8 | 24px | 16px |
| Expanded | 12 | 24px | 16px |
| Large | 12 | 24px | 24px |
| Extra-large | 12 | 24px | 24px |

```css
/* Compact is the base; move to a wider layout as content permits. */
@media (min-width: 600px) { }
@media (min-width: 840px) { }
@media (min-width: 1200px) { }
@media (min-width: 1600px) { }
```

Constrain reading width on wide windows. Use available space for additional panes or a useful grid instead of stretching a single text column. For multi-pane flows, let the active destination and application state determine whether a narrow viewport shows a list or detail view.

## App Shell and Navigation

Choose navigation based on destination count and screen space. Common patterns include a bottom navigation bar for a few primary destinations on compact screens, a rail as space allows, and a persistent or modal drawer for larger destination sets. This is a design choice, not a requirement to render three duplicate navigation trees. Preserve one route/state model and expose the appropriate presentation for each breakpoint.

Use tabs only to switch between related peer views. Use links for navigation and buttons for actions. Keep labels visible where users need to identify destinations; an icon alone needs an accessible name and should not be the only cue for a selected state.

## Layout Details

- Use a consistent spacing scale based on multiples of 4 CSS px; adapt margins and gutters to content density and window width.
- Use CSS Grid or Flexbox for canonical feed, list-detail, and supporting-pane layouts.
- Ensure flex/grid children that contain long content can shrink (`min-width: 0`) and wrap without horizontal overflow.
- Keep sticky bars and bottom navigation clear of safe areas and the on-screen keyboard where relevant.
- Avoid placing critical content or controls underneath fixed navigation or overlays.
- Use logical properties (`margin-inline`, `inset-inline-start`) for bidirectional layouts.

## Canonical Layouts

Start with the layout that matches the content model, then adapt it to available width.

### Feed

Use a feed/grid for a browsable collection. A common progression is one column on compact, two on medium, three on expanded, and up to four on large windows. Set a sensible card minimum width instead of forcing a fixed column count.

```css
.feed {
  display: grid;
  grid-template-columns: minmax(0, 1fr);
  gap: 8px;
  padding: 16px;
}
@media (min-width: 600px) {
  .feed { grid-template-columns: repeat(2, minmax(0, 1fr)); gap: 16px; padding: 24px; }
}
@media (min-width: 840px) {
  .feed { grid-template-columns: repeat(3, minmax(0, 1fr)); }
}
@media (min-width: 1200px) {
  .feed { grid-template-columns: repeat(4, minmax(0, 1fr)); gap: 24px; }
}
```

### List-detail

Use for email, files, contacts, and other collections where selecting an item reveals substantial detail. On compact windows, show either the list or selected detail using the application's route/state. At wider sizes, show both panes; constrain the list pane and let the detail pane take remaining width.

```css
.list-detail { display: grid; grid-template-columns: minmax(0, 1fr); min-height: 0; }
.list-detail__list, .list-detail__detail { min-width: 0; overflow: auto; }
@media (min-width: 600px) {
  .list-detail { grid-template-columns: minmax(280px, 1fr) minmax(0, 2fr); gap: 24px; }
}
@media (min-width: 840px) {
  .list-detail { grid-template-columns: minmax(320px, 400px) minmax(0, 1fr); }
}
```

### Supporting pane

Use when primary content benefits from persistent secondary information or controls, such as a document with properties. Stack on compact screens or move secondary content to a suitable sheet; place panes side by side when there is room.

```css
.supporting-pane { display: grid; grid-template-columns: minmax(0, 1fr); gap: 16px; padding: 16px; }
@media (min-width: 600px) {
  .supporting-pane { grid-template-columns: minmax(0, 2fr) minmax(280px, 1fr); gap: 24px; padding: 24px; }
}
```

For a draggable divider, prefer a documented M3E split-pane component when it fits. Otherwise, implement a keyboard-operable separator with a value range, arrow-key resizing, and an accessible name; a visual drag handle alone is not sufficient.

## App Shell

```html
<div class="app-shell">
  <header class="app-bar"><h1>Projects</h1></header>
  <main class="app-content">
    <section class="feed" aria-label="Recent projects">
      <m3e-card>...</m3e-card>
    </section>
  </main>
  <nav class="compact-navigation" aria-label="Primary">...</nav>
</div>
```

Use the M3E component docs for exact custom element names and slots. The shell owns responsive placement and app routing; component internals should own their documented interaction behavior.

```css
.app-shell {
  display: grid;
  grid-template-rows: auto minmax(0, 1fr) auto;
  min-height: 100dvh;
  color: var(--md-sys-color-on-surface);
  background: var(--md-sys-color-surface);
}
.app-content { min-width: 0; min-height: 0; overflow: auto; }
@media (min-width: 600px) {
  .app-shell { grid-template-columns: 80px minmax(0, 1fr); grid-template-rows: auto minmax(0, 1fr); }
  .app-content { grid-column: 2; }
}
```

Treat viewport units carefully on mobile. Prefer `100dvh` with a fallback where needed; account for safe-area insets around fixed bottom navigation (`env(safe-area-inset-bottom)`) and ensure focused controls are not obscured by the virtual keyboard.

## Card Grid

Use M3E cards for coherent subjects and keep actions inside their subject. Give media a stable aspect ratio and meaningful alt text. Avoid turning a decorative card into a link without a clear accessible name and focus state.

```html
<section class="project-grid" aria-label="Projects">
  <m3e-card variant="outlined">
    <img slot="content" src="project.webp" alt="Project dashboard preview" />
    <m3e-heading slot="header" variant="title" size="medium">Project name</m3e-heading>
    <p slot="content">Updated today</p>
    <div slot="actions" end><m3e-button variant="text">Open</m3e-button></div>
  </m3e-card>
</section>
```

```css
.project-grid { display: grid; grid-template-columns: repeat(auto-fill, minmax(min(100%, 280px), 1fr)); gap: 16px; }
.project-grid img { display: block; width: 100%; aspect-ratio: 16 / 9; object-fit: cover; }
```

Check slot composition against the card component docs: a component may allow only one element per slot or expect a wrapper for mixed media/content.

## Form Layout

Group related controls, use explicit labels and supporting/error messages, and keep submit/cancel actions adjacent to the form. Use M3E `m3e-form-field` for its documented label/subscript treatment with native input controls; consult its API for association and validation behavior.

```html
<form class="form-layout">
  <m3e-form-field>
    <label slot="label" for="name">Name</label>
    <input id="name" name="name" autocomplete="name" required />
  </m3e-form-field>
  <m3e-form-field>
    <label slot="label" for="email">Email</label>
    <input id="email" name="email" type="email" autocomplete="email" required />
  </m3e-form-field>
  <div class="form-layout__actions">
    <m3e-button type="reset">Reset</m3e-button>
    <m3e-button variant="filled" type="submit">Continue</m3e-button>
  </div>
</form>
```

```css
.form-layout { display: grid; gap: 16px; width: min(100%, 560px); }
.form-layout__actions { display: flex; justify-content: flex-end; gap: 8px; margin-top: 8px; }
```

Confirm that each custom control participates in native form submission and reset as expected; custom elements do not automatically inherit every native input behavior.

## Large Screens and Foldables

For desktop, tablets, and unfolded foldables, use the extra width for multiple useful panes or a bounded grid. Keep long-form reading width constrained (often around 840–1040 CSS px), and avoid stretching one text column across an ultra-wide viewport. Web fold/hinge APIs have limited support; only use viewport segment media features when the target browser exposes them, and keep a conventional responsive fallback. Never place essential controls across a hinge or segment gap.

When targeting foldables, consider book posture for list-detail content and tabletop posture for media plus controls. Validate on the target browser/device because posture and viewport-segment support are not universal web capabilities.

For readable desktop content, constrain body copy to a measure near 65–80 characters where practical. Use expanded space for another pane, metadata, or grid column instead of widening paragraphs indefinitely. Support hover as supplemental feedback; never make it the only way to discover an action.

### Container Queries

Use container queries for components that appear in both full-width and narrow panes, such as a card reused in a feed and a side panel.

```css
.card-region { container: card-region / inline-size; }
@container card-region (min-width: 400px) {
  .project-card { display: grid; grid-template-columns: 8rem minmax(0, 1fr); }
}
```

## Accessibility and Input

- Use semantic HTML and correct roles; keep buttons, links, form labels, and headings meaningful.
- Ensure all functionality works by keyboard. Provide visible focus and return focus appropriately after dialogs, menus, and sheets close.
- Give icon-only controls accessible names. Announce meaningful asynchronous status changes without flooding assistive technology.
- Maintain readable contrast for text, icons, boundaries, disabled states, and focus indicators across themes.
- Keep touch targets comfortable for touch input, and do not remove touch affordances because a desktop pointer is available.
- Support browser zoom and text enlargement without clipped controls or overlapping content.
- Respect `prefers-reduced-motion`; avoid making hover the only way to reveal information.
- Validate custom elements' keyboard, form, focus, and screen-reader behavior in the installed library version; do not assume visual parity guarantees accessible behavior.

## Responsive and Accessibility Audit

- [ ] Test the actual minimum supported width, intermediate widths, and a wide desktop window.
- [ ] Navigation and panes adapt without duplicating routes or hiding the current location.
- [ ] Content remains readable and does not overflow when labels are long or text is enlarged.
- [ ] All actions can be completed with keyboard alone and focus is visible.
- [ ] Dialogs, menus, sheets, and popovers have correct accessible names and predictable focus return.
- [ ] Light and dark themes preserve contrast and state clarity.
- [ ] Reduced-motion mode keeps state changes understandable without nonessential animation.
- [ ] Touch, mouse, keyboard, and assistive technology behavior are checked for the components in use.
