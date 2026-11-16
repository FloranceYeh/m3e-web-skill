# Material 3 Expressive Guidelines for Web

This reference summarizes Google's Material 3 Expressive design principles for web applications. It describes design intent, not a promise that a particular M3E release implements each behavior. Check the linked official pages for updates before applying version-sensitive details. Google's current I/O updates are the top-level change log; use the M3 Expressive announcement and individual design pages for the underlying design guidance.

## Principles

- **Personal:** Provide coherent color and theme choices. Web has no standard browser API for extracting a user's wallpaper color; use an explicit theme seed or derive color from app content only when that behavior benefits the product.
- **Adaptive:** Reflow information and navigation for the available window, input methods, and content. Treat breakpoints as layout decisions, not device detection.
- **Expressive:** Use typography, shape, color, and motion to establish hierarchy and communicate state. Reserve the strongest expression for meaningful emphasis; maintain clarity and usability.

## Color and Theming

Use semantic roles, not raw colors scattered through component CSS. Pair each container role with its corresponding `on-*` role. Use `outline` for meaningful interactive boundaries and `outline-variant` for lower-emphasis separators. Keep error roles legible and distinguish error from selection or decorative emphasis.

Build light and dark schemes with a supported Material theme generator or the M3E theme system. Support the user's system preference when appropriate, and allow an explicit product theme setting to take precedence. Dynamic color is optional and must not reduce contrast. Do not claim that web can read Android wallpaper colors.

M3 Expressive expands the palette of color roles and expressive treatments. Prefer official role names and generated scheme output over copying static color values from an example. Check the official color guidance for current role sets and contrast expectations: [Color](https://m3.material.io/styles/color/overview).

## Typography

Use the Material type scale to establish hierarchy. Map display/headline, title, body, and label roles to the content's actual reading order. Keep labels concise and readable; do not use expressive type as a substitute for clear wording. Use the M3E theme typography or CSS tokens available in the installed version.

M3 Expressive typography includes more expressive type treatments and variable-font capabilities where available. Treat these as options for hierarchy and personality, not a mandate to enlarge every heading. Consult [Typography](https://m3.material.io/styles/typography/overview) for current scale and usage.

## Shape and Components

Use component shape tokens consistently, and use shape variation to communicate component role or state. Expressive shape treatments and morphing may be used where the web implementation supports them and the change clarifies interaction. Do not add shape motion to every element or rely on shape alone to convey a state.

Use established M3 component patterns for actions, selection, navigation, containment, and data display. Keep density and control size appropriate to the task and input method. Consult [Shape](https://m3.material.io/styles/shape/overview) and official component guidance for the component at hand.

## Motion

Motion should explain cause and effect, preserve spatial continuity, and respond to user input. M3 Expressive emphasizes spring-like, responsive motion for expressive interactions; use it when the library and browser implementation offer an appropriate primitive. CSS transitions may approximate a spec-aligned transition but are not equivalent to spring physics.

- Keep durations and distances proportionate to the amount of change.
- Animate state changes that benefit from feedback; avoid decorative movement that competes with content.
- Respect `prefers-reduced-motion: reduce`. Remove nonessential movement and retain a clear state change.
- Avoid motion as the only status signal.

See [Motion](https://m3.material.io/styles/motion/overview) and verify current expressive motion guidance.

## Elevation and Surfaces

Use surface roles and tonal differences to organize depth. Reserve shadows for components that must separate from busy content or where the component pattern calls for them. Use elevation consistently rather than applying shadow as generic decoration.

## Applying Expressive Design on the Web

Material 3 Expressive is a design system direction, not a requirement to make every control animated or visually loud. Use these decisions when translating the official guidance to a browser UI:

| Design area | Apply when | Check in the browser |
|---|---|---|
| Color expression | Brand or content palette helps the user recognize context | Semantic pairs remain legible in light/dark and selected/error/disabled states |
| Typography emphasis | A title, value, or selected item needs clear hierarchy | Text wraps, scales, and remains readable with localization and zoom |
| Shape variation | A component's role or selected state benefits from a distinct silhouette | The shape does not imply state by itself; transitions honor reduced motion |
| Spring motion | A state change benefits from responsive, interruptible feedback | M3E theme uses expressive motion; custom animations do not fight component motion |
| Adaptive composition | Wider windows can show related content simultaneously | Navigation and panes move at content-appropriate thresholds |
| Density | A data-heavy workflow benefits from compact presentation | Compact targets remain operable and controls do not become crowded |

Use expressive details to reinforce hierarchy or state. Avoid layering several high-salience treatments (bright accent, oversized type, large shape, deep shadow, long animation) on the same low-priority element.

## Web Implementation Notes

- M3E's theme component documents expressive motion, dynamic color, contrast, density, and strong-focus options. Prefer those integrated settings over separate global approximations.
- M3E's components may provide expressive behavior that older `@material/web` components do not. Do not transfer old `md-*` API assumptions into M3E code.
- The browser is a multi-input platform. Test touch, mouse/hover, keyboard/focus, zoom, and reduced motion; do not reproduce Android-only behavior as if it were a browser API.
- Expressive color or shape must never replace accessible names, visible labels, text status, focus indication, or semantic selected state.
- When the library's current version lacks a needed behavior, implement the smallest spec-aligned web behavior and document the gap in the implementation, not as if it were built-in.

## Official References

- [Material updates, Google I/O 2026](https://m3.material.io/blog/whats-new-at-io26)
- [Material 3 Expressive announcement, Google I/O 2025](https://m3.material.io/blog/whats-new-at-io25)
- [Material 3 styles](https://m3.material.io/styles)
- [Color](https://m3.material.io/styles/color/overview)
- [Typography](https://m3.material.io/styles/typography/overview)
- [Shape](https://m3.material.io/styles/shape/overview)
- [Motion](https://m3.material.io/styles/motion/overview)
- [Components](https://m3.material.io/components)

Use current official pages as the normative source. The announcement summarizes the system update; individual component and style pages hold detailed behavior.

## Design Review

- [ ] Color roles form a coherent scheme with matching foreground roles and adequate contrast.
- [ ] Type scale reflects content hierarchy and remains readable at narrow widths and zoom.
- [ ] Shape is intentional and consistent with the selected component patterns.
- [ ] Motion communicates state or spatial relationships and honors reduced motion.
- [ ] Tonal surfaces establish hierarchy; shadows are purposeful.
- [ ] Expressive details improve emphasis or feedback without obscuring task completion.
- [ ] High-salience expression is concentrated on important hierarchy or state changes.
- [ ] Custom motion does not conflict with M3E's motion tokens or component transitions.
- [ ] Mouse-only and platform-specific behaviors have usable browser alternatives.
