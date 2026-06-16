# open-typefaces

A tiny, self-hosted **CDN of openly-licensed typefaces** — the ones worth using that aren't already on a CDN. Drop the font files in a public GitHub repo, and [jsDelivr](https://www.jsdelivr.com/), [Statically](https://statically.io/), and GitHub Pages will serve them globally, versioned, and CORS-friendly **for free, with zero build step**.

This is the same mechanism [Fontsource](https://fontsource.org/) uses under the hood (npm package → jsDelivr). The difference here: a plain GitHub repo you curate by hand, for fonts that never got packaged.

## Quick start

Each typeface ships a ready-to-use stylesheet with `@font-face` already written. Load it from a CDN and set `font-family` — that's it.

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/maxlair1/open-typefaces@v1.0.0/fonts/drafting-mono/drafting-mono.css">
<style> body { font-family: 'Drafting Mono', ui-monospace, monospace; } </style>
```

The `src` URLs inside each CSS file are **relative**, so the stylesheet and its `woff2` files always load from the same host — jsDelivr, Statically, GitHub Pages, or a local checkout. No per-consumer `@font-face` editing.

## CDN URL patterns

Replace `<ref>` with a release tag (`v1.0.0`), a commit SHA, or a branch (`main`). **Pin to a tag or SHA in production.**

| Host | Pattern |
|---|---|
| jsDelivr | `https://cdn.jsdelivr.net/gh/maxlair1/open-typefaces@<ref>/fonts/<id>/<id>.css` |
| Statically | `https://cdn.statically.io/gh/maxlair1/open-typefaces/<ref>/fonts/<id>/<id>.css` |
| GitHub Pages | `https://maxlair1.github.io/open-typefaces/fonts/<id>/<id>.css` *(if Pages enabled)* |

> Avoid `raw.githubusercontent.com` — it serves fonts as `text/plain`, sends no long-lived cache headers, and isn't a CDN. Use jsDelivr or Statically.

## Typefaces

| Typeface | Category | License | ID |
|---|---|---|---|
| [Drafting Mono](./fonts/drafting-mono/) | Monospace | OFL-1.1 | `drafting-mono` |
| [Aspekta](./fonts/aspekta/) | Sans-serif | OFL-1.1 | `aspekta` |

A machine-readable index of everything here lives in [`fonts.json`](./fonts.json).

## Add a typeface

See [`docs/adding-a-typeface.md`](./docs/adding-a-typeface.md). Short version: copy [`_template/`](./_template/) to `fonts/<id>/`, drop in the `woff2` files + the font's own license, fill in `metadata.json`, write the `@font-face` CSS, add a row to `fonts.json` and the table above, then tag a release.

### Licensing rule

**Only fonts whose license permits redistribution go in this repo** — OFL, Apache-2.0, MIT/Ubuntu Font License, etc. The license file (e.g. `OFL.txt`) **must** live in the font's folder alongside the binaries; that's both a legal requirement of most font licenses and a courtesy to anyone reading the repo. No proprietary, "free for personal use," or unclear-license fonts.

## License

- **Repo scaffolding** (CSS, docs, scripts, this README): [MIT](./LICENSE).
- **Each typeface**: governed by **its own license**, included in that font's folder. The MIT license here does **not** cover the font files.
