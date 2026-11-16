# Navigation Patterns

Use a navigation component that matches the number and hierarchy of destinations. Navigation changes location; tabs switch among related peer views within the current context.

## Selection Guide

| Pattern | Typical use | Placement |
|---|---|---|
| Navigation bar | A few top-level destinations on compact screens | Persistent bottom region |
| Navigation rail | A few destinations when a persistent side region fits | Start edge on medium/expanded layouts |
| Navigation drawer/menu | Many destinations, grouped hierarchy, or larger desktop layouts | Persistent on wide screens or modal when space is constrained |
| Tabs | Related peer views inside one destination | Adjacent to the content they control |
| Search | Find content across destinations or within a view | App bar or dedicated search surface |

As a starting point, use a bottom bar for roughly 3–5 primary destinations on compact layouts, a rail when side navigation fits, and a drawer for larger destination sets or hierarchy. This is guidance, not a hard destination-count rule; choose the component that keeps labels clear and content usable.

## App Bar

Use a top app bar for the current destination title and a small set of contextual actions. Choose center-aligned, small, medium, or large treatment based on hierarchy and available vertical space. Larger bars may collapse on scroll where appropriate. Keep a stable title and ensure controls remain reachable as the bar changes.

## Navigation Bar and Rail

Use a navigation bar for persistent compact navigation. Keep destination labels legible and indicate the selected destination with more than color alone. Use a rail when there is room for side navigation and content should remain visible; a rail can include a leading primary action when appropriate.

M3E's `@m3e/web/nav-bar` exposes `m3e-nav-bar` and `m3e-nav-item`. The documented bar `mode` values are `compact`, `expanded`, and `auto`; items support selected, link, and disabled states. `@m3e/web/nav-rail` exposes `m3e-nav-rail` and a rail toggle, with corresponding compact/expanded/auto modes. The navigation components emit `beforeinput`, `input`, and `change` when item selection changes. Verify router integration and event details before wiring route state.

```html
<m3e-nav-bar mode="auto" aria-label="Primary navigation">
  <m3e-nav-item href="/overview" selected>
    <m3e-icon slot="icon" name="dashboard"></m3e-icon>Overview
  </m3e-nav-item>
  <m3e-nav-item href="/activity">
    <m3e-icon slot="icon" name="history"></m3e-icon>Activity
  </m3e-nav-item>
  <m3e-nav-item href="/settings">
    <m3e-icon slot="icon" name="settings"></m3e-icon>Settings
  </m3e-nav-item>
</m3e-nav-bar>
```

Import `@m3e/web/nav-bar` and `@m3e/web/icon`; use an icon font or registry supported by M3E. Use real `href` destinations for navigation and synchronize selected state with the active route. Use a button pattern instead when the interaction is an action within the current route.

## Drawer

Use a modal drawer when navigation temporarily overlays content; use a persistent drawer when the expanded layout can accommodate it without constraining the task area. Group related destinations with headings or separators. The current destination should be apparent, and opening/closing the drawer must preserve a logical focus order.

The M3E navigation family includes `nav-menu`; confirm persistent/modal modes, trigger, item, and close behavior in the package docs. Do not infer its API or DOM structure from the older `md-navigation-drawer` element.

## Tabs

Use primary tabs for peer sections at one level and secondary tabs for subordinate views. Associate each tab with its panel, expose selected state semantically, support expected arrow-key navigation, and avoid using tabs as a substitute for app navigation or a long list of unrelated destinations.

## Responsive Transformation

Navigation may transform from bottom bar to rail to drawer as width permits. Keep route identity and selected state in the host application; do not create three independent navigation state trees. Use M3E's `nav-bar`, `nav-rail`, and `nav-menu` families where their documented behavior fits, and test at the actual transition widths.

```css
.nav-bar, .nav-rail, .nav-menu { display: none; }
@media (max-width: 599px) { .nav-bar { display: flex; } }
@media (min-width: 600px) and (max-width: 839px) { .nav-rail { display: flex; } }
@media (min-width: 840px) { .nav-menu { display: flex; } }
```

Avoid rendering visually hidden but still focusable duplicate items. Use `hidden`, conditional rendering, or an equivalent mechanism that also removes inactive variants from keyboard and assistive technology navigation.

Avoid swapping navigation solely from viewport width when the app is rendered in an embedded/narrow panel. Base transformations on the available shell/container width where possible.

## Navigation Review

- [ ] Destination count and hierarchy fit the selected pattern.
- [ ] Route changes, selected styling, and current-page semantics stay synchronized.
- [ ] Tabs control associated peer panels and support expected keyboard behavior.
- [ ] Icon-only controls have accessible names; selection is not conveyed by color alone.
- [ ] Inactive responsive variants are absent from the accessibility tree and tab order.
- [ ] Drawers/menus expose open state, dismiss predictably, and restore focus to the trigger.
- [ ] Route links remain links; immediate actions remain buttons.
