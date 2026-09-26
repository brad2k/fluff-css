# fluff-css

Baseline CSS for static sites (Astro). Cascade layers, in order:

`fluff.primitives` → `fluff.tokens` → `fluff.reset` → `fluff.base` → `fluff.composition` → `fluff.utilities`

Your project's own unlayered CSS always wins over these.

## Structure

```
index.css            entry point; declares layer order and imports src/
src/layers.css       layer order (import first when going à la carte)
src/primitives.css   raw scales (Utopia type + space)
src/tokens.css       semantic tokens (colors, fonts, motion, focus)
src/reset.css
src/base.css         bare-element styles; consume tokens, don't define them
src/composition.css  layout primitives: stack, cluster, repel, flow, auto-grid, center-xy
src/utilities.css
index.html           dev playground
```

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

The library is generic; each project supplies a small theme file that redefines tokens. `base.css` only reads variables, so nothing in the library gets edited.

```
my-astro-site/src/styles/
├── theme.css     # this project's tokens: fonts, colors, scale overrides
└── global.css    # imports the library, then the theme, then site CSS
```

```css
/* global.css */
@import "fluff-css";
@import "./theme.css";
```

```css
/* theme.css */
@font-face { font-family: "Inter"; src: url("/fonts/inter.woff2") format("woff2"); font-display: swap; }

:root {
  --font-body: "Inter", system-ui, sans-serif;
  --font-display: "Fraunces", serif;
  --color-text: light-dark(#1a1a2e, #eaeaf5);
  --color-bg: light-dark(#faf8f5, #14141f);
  --color-link-active: light-dark(#c2410c, #fb923c);
  --anchor-text-decoration: underline;
}
```

Primitives and tokens live in cascade layers, so unlayered project CSS always overrides them regardless of import order.

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
