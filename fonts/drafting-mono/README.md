# Drafting Mono

An original monospaced typeface by [indestructible type\*](https://indestructibletype.com/Drafting/) — inspired by typewriters and the "still-in-the-works" spirit of drafting.

- **License:** SIL Open Font License 1.1 ([`OFL.txt`](./OFL.txt)) — free to use, embed, and redistribute.
- **Upstream:** https://github.com/indestructible-type/Drafting
- **Weights:** 100–700 (Thin · ExtraLight · Light · Regular · Medium · SemiBold · Bold), each with an italic.
- **Variable axis:** `wght` 100–700 (default 400), roman + italic.
- **Format:** `woff2` only (smallest; supported by every current browser).

## Use it (recommended — one `<link>` to the CSS)

The CSS uses **relative** font URLs, so loading the stylesheet from any host pulls the font files from the same host automatically.

### Static weights — via jsDelivr (pin to a tag for production)

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/maxlair1/open-typefaces@v1.0.0/fonts/drafting-mono/drafting-mono.css">
```

```css
body { font-family: 'Drafting Mono', ui-monospace, monospace; }
strong { font-weight: 700; }
```

### Variable font (arbitrary weights, smaller payload)

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/maxlair1/open-typefaces@v1.0.0/fonts/drafting-mono/drafting-mono-variable.css">
```

```css
.heading { font-family: 'Drafting Mono', monospace; font-weight: 430; }
```

### Other delivery hosts (same paths)

```
Statically:    https://cdn.statically.io/gh/maxlair1/open-typefaces/v1.0.0/fonts/drafting-mono/drafting-mono.css
GitHub Pages:  https://maxlair1.github.io/open-typefaces/fonts/drafting-mono/drafting-mono.css   (if Pages is enabled)
```

> Pin to a **tag** (`@v1.0.0`) or **commit SHA** in production for an immutable, cache-friendly URL. `@latest` / `@main` can change under you and is rate-limited differently.

## Direct file URLs

Individual `woff2` files live in [`files/`](./files/). Family name in `@font-face` is `'Drafting Mono'`.

## Attribution

Drafting Mono is © 2021 The Drafting Mono Project Authors, designed by indestructible type\*. The OFL does not require attribution in your UI, but does require that the license travel with the font files — which is why [`OFL.txt`](./OFL.txt) sits beside them here. Do not sell the fonts on their own.
