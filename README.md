# fluff-css

Baseline CSS for static sites (Astro). Cascade layers, in order:

`fluff.primitives` → `fluff.tokens` → `fluff.reset` → `fluff.base` → `fluff.composition` → `fluff.utilities`

Your project's own unlayered CSS always wins over these.

## Structure

```
index.css            entry point; declares layer order and imports src/
src/layers.css       layer order (import first when going à la carte)
src/primitives.css   raw scales (Utopia type + space); --fc-color-grey-*, --fc-step-*, --fc-space-*
src/tokens.css       semantic tokens (colors, fonts, motion, focus, customization overrides)
src/reset.css
src/base.css         bare-element styles; consume tokens, don't define them
src/composition.css  layout primitives: stack, cluster, repel, flow, auto-grid, center-xy
src/utilities.css
index.html           dev playground
```

### Variable naming

All variables use the `--fc-` prefix to avoid collisions with frameworks, third-party components, and future CSS specs:

- **Primitives** (raw scales): `--fc-color-grey-50`, `--fc-step-0`, `--fc-space-s`, `--fc-color-overlay-subtle`
- **Tokens** (semantic): `--fc-color-text`, `--fc-font-size-h1`, `--fc-animation-transition-base`, `--fc-focus-ring`
- **Customization** (consumer overrides): `--fc-anchor-text-decoration`, `--fc-heading-letter-spacing`, `--fc-cluster-space`, etc.

## Use in a project

```sh
npm i github:brad2k/fluff-css            # or pin: github:brad2k/fluff-css#v0.1.0
```

```astro
---
import "fluff-css";          // everything
// or à la carte:
import "fluff-css/tokens";
import "fluff-css/reset";
---
```

## Per-project theming

The library is generic; each project supplies a small theme file that redefines tokens and customization variables. Since all fluff-css variables live in cascade layers, your unlayered project CSS always takes precedence—import order doesn't matter.

```
my-astro-site/src/styles/
├── theme.css     # override tokens, customization vars, or add new rules
└── global.css    # imports the library, then your theme
```

```css
/* global.css */
@import "fluff-css";
@import "./theme.css";
```

### Overriding tokens

Change semantic values (colors, fonts, motion) for your brand:

```css
/* theme.css */
@font-face { font-family: "Inter"; src: url("/fonts/inter.woff2") format("woff2"); font-display: swap; }

:root {
  --fc-font-body: "Inter", system-ui, sans-serif;
  --fc-font-display: "Fraunces", serif;
  --fc-color-text: light-dark(#1a1a2e, #eaeaf5);
  --fc-color-bg-canvas: light-dark(#faf8f5, #14141f);
  --fc-color-link: light-dark(#2563eb, #60a5fa);
}
```

### Customizing component behavior

Adjust how specific elements render without editing the library:

```css
:root {
  /* Links: remove underlines in prose */
  --fc-anchor-text-decoration: none;
  
  /* Headings: tighter tracking */
  --fc-heading-letter-spacing: -0.03em;
  
  /* Composition: increase default gaps */
  --fc-cluster-space: 2rem;
  --fc-stack-space: 1.75rem;
  
  /* Focus: stronger ring for better visibility */
  --fc-focus-ring: 3px solid var(--fc-color-link);
}

**Replacing the scales entirely** (e.g. a different Utopia range): import à la carte and skip `primitives`:

```css
@import "fluff-css/layers";
@import "./my-primitives.css";   /* your own --step-* / --space-* */
@import "fluff-css/tokens";
@import "fluff-css/reset";
@import "fluff-css/base";
@import "fluff-css/composition";
@import "fluff-css/utilities";
```

## Develop locally

**Playground:** open `index.html` in a browser (or run `npx serve .` for live reload). Edit `src/*.css` and refresh.

**Live in a real project with `npm link`:**

```sh
# 1. In this repo: register it globally
cd ~/Code/fluff-css
npm link

# 2. In the consuming project: link to it
cd ~/Code/my-astro-site
npm link fluff-css
```

Edits here now show up in the project's dev server immediately. If Vite doesn't pick up changes, restart the dev server once.

To undo, in the project run `npm unlink fluff-css && npm install`.

**Alternative, no global link:** in the project's `package.json` use
`"fluff-css": "file:../fluff-css"` (symlinks the same way).

Note: `npm install` in the project may remove the link; re-run `npm link fluff-css` afterwards.

## Release

```sh
git tag v0.2.0 && git push --tags
```

Then in each project: `npm i github:brad2k/fluff-css#v0.2.0`.
