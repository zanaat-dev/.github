# Lonca brand assets

Canonical brand files. Source of truth: this directory. Every other use (docs site, READMEs, package metadata) references files from here.

## Files

| File                                         | Use                                                                 | Size baseline  |
| -------------------------------------------- | ------------------------------------------------------------------- | -------------- |
| [`icon.svg`](./icon.svg)                     | Symbol mark alone (avatars, favicons, square containers, app icons) | 24×24, scales  |
| [`wordmark.svg`](./wordmark.svg)             | Word "lonca" alone (footers, tight inline contexts)                 | 83×24, scales  |
| [`logomark.svg`](./logomark.svg)             | Full lockup: symbol + word (primary lockup, README hero, site logo) | 114×24, scales |
| [`social-preview.png`](./social-preview.png) | GitHub repo Social Preview (1280×640)                               | —              |

## One file, all surfaces

Each SVG carries a single contrast-safe ink. The mark is drawn with `fill="currentColor"` and the SVG ships an inline `<style>` block that sets `color` to one value that stays legible on both light and dark backgrounds:

```svg
<style>
  svg { color: #6366F1; } /* cross-surface ink */
</style>
```

This means a single file works everywhere, including surfaces where background and OS theme do not match:

- **`<img src="...">`** in READMEs — npm and GitHub both render a readable mark regardless of the viewer's OS theme.
- **Inline SVG / Starlight theming** — outside CSS (`.starlight svg { color: ... }`) can still override the inline rule when a surface needs a custom tint.
- **Favicons** — modern browsers render the same stable ink color.

We scope the rule to `svg` (rather than `:root`) so that if someone inlines the SVG into HTML the rule doesn't leak onto the host page's `color`. No `*-light.svg` / `*-mono.svg` companions. No `<picture>` markup. One file, one URL.

## Color tokens

| Token                 | Hex       | Use                                                                              |
| --------------------- | --------- | -------------------------------------------------------------------------------- |
| `brand.ink`           | `#6366F1` | Default mark color. Meets contrast targets on both `#FFFFFF` and `#0F0E10`.     |
| `brand.surface-light` | `#FFFFFF` | Reference light surface.                                                         |
| `brand.surface-dark`  | `#0F0E10` | Reference dark surface.                                                          |

For accents and palette extension, defer to the docs site stylesheet (`docs/src/styles/custom.css`).

## Clearspace

Minimum padding around the mark = the height of the **bullet** (the small square at the bottom-right of the icon). For a 24px icon that's ~2px on every side. The lockup follows the same rule — don't crop closer.

## Do's and don'ts

- ✅ Scale uniformly (SVG handles this).
- ✅ Use the icon with type in your own UI — just respect clearspace.
- ✅ Override the inline color via consuming CSS when you need a non-default tint.
- ❌ Don't strip the `<style>` block — it's what keeps the canonical ink token embedded.
- ❌ Don't stretch non-uniformly.
- ❌ Don't add drop-shadows, glows, or borders. The mark is intentionally flat.

## Where these are wired

| Surface                             | File                 | Where                                                |
| ----------------------------------- | -------------------- | ---------------------------------------------------- |
| Docs site logo                      | `logomark.svg`       | `docs/astro.config.mjs` → `starlight.logo.src`       |
| Docs site favicon                   | `icon.svg`           | `docs/astro.config.mjs` → `favicon`                  |
| Root [README](../../README.md) hero | `logomark.svg`       | `<img>` (height 48)                                  |
| Per-package READMEs                 | `icon.svg`           | `<img>` (height 32) — uses GitHub raw URL            |
| GitHub repo social preview          | `social-preview.png` | Upload via Repo Settings → Social preview (one-time) |

## Updating

Edit the canonical files here, then run:

```bash
pnpm docs:sync-brand   # copies assets/brand/* → docs/public/brand/
```

(Run automatically before `pnpm dev` / `pnpm build` in `docs/`.)
