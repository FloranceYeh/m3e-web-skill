# Material 3 Expressive Web Skill

Codex skill for designing, implementing, and reviewing Material 3 Expressive web interfaces with [M3E](https://github.com/matraic/m3e) Web Components.

## Install

Copy `skills/m3e-web` into a Codex skills directory:

```text
<workspace>/.codex/skills/m3e-web/
```

For a user-level installation, use:

```text
~/.codex/skills/m3e-web/
```

Keep the `SKILL.md` and its `references/` directory together. Invoke it in Codex as `$m3e-web`, or describe a web task that should use Material 3 Expressive and M3E.

## Scope

The skill covers browser-based interfaces across web frameworks. It uses `@m3e/web` custom elements as the default component implementation and documents `@m3e/react` bindings for React. It includes design guidance, component selection, theming, responsive layouts, motion, accessibility, no-bundler setup, SSR boundaries, and editor tooling.

Google's Material 3 Expressive documentation is the design source of truth. M3E's documentation is the source for package APIs and implementation support. M3E is an independent project and is not affiliated with Google.

## Contents

- [Skill entry point](skills/m3e-web/SKILL.md)
- [Expressive design guidance](skills/m3e-web/references/expressive-guidelines.md)
- [M3E component catalog](skills/m3e-web/references/component-catalog.md)
- [M3E web setup and API notes](skills/m3e-web/references/m3e-web-components.md)
- [Navigation patterns](skills/m3e-web/references/navigation-patterns.md)
- [Theme and dynamic color](skills/m3e-web/references/theming-and-dynamic-color.md)
- [Typography, shape, elevation, and motion](skills/m3e-web/references/typography-and-shape.md)
- [Web layout and accessibility](skills/m3e-web/references/web-layout-and-accessibility.md)

## Sources

- [Material Design 3](https://m3.material.io/)
- [Material 3 Expressive update](https://m3.material.io/blog/whats-new-at-io25)
- [M3E documentation and examples](https://matraic.github.io/m3e)
- [M3E repository](https://github.com/matraic/m3e)
