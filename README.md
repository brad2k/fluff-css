# fluff-css

Baseline CSS for static sites (Astro). Cascade layers, in order:

`fluff.reset` → `fluff.base` → `fluff.composition` → `fluff.utilities`

Your project's own unlayered CSS always wins over these.

## Structure

```
index.css            entry point; declares layer order and imports src/
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

Override tokens in your project after the import:

```css
:root { --color-link-active: rebeccapurple; }
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
