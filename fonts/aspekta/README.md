# Aspekta

A modern sans-serif family by [Ivo Dolenc](https://github.com/ivodolenc), notable for an unusually fine weight range — **20 static weights from 50 to 1000 in steps of 50**.

- **License:** SIL Open Font License 1.1 ([`OFL.txt`](./OFL.txt)), Reserved Font Name "Aspekta" — free to use, embed, and redistribute verbatim.
- **Upstream:** https://github.com/ivodolenc/aspekta (not on Fontsource / Google Fonts / npm — this is the convenient CDN copy).
- **Weights:** 50, 100, 150, … 1000 (every 50). Roman only — no italics.
- **Variable axis:** `wght` **100–900** (default 400). Note the variable range is narrower than the static set — 50, 950 and 1000 exist only as static weights.
- **Format:** `woff2` only.

## Use it (recommended — one `<link>` to the CSS)

The CSS uses **relative** font URLs, so loading the stylesheet from any host pulls the font files from the same host automatically.

### Static weights — via jsDelivr (pin to a tag for production)

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/maxlair1/open-typefaces@v1.1.0/fonts/aspekta/aspekta.css">
```

```css
body { font-family: 'Aspekta', system-ui, sans-serif; }
.hairline { font-weight: 50; }
.black { font-weight: 1000; }
```

### Variable font (arbitrary weights 100–900, smaller payload)

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/maxlair1/open-typefaces@v1.1.0/fonts/aspekta/aspekta-variable.css">
```

```css
.heading { font-family: 'Aspekta', sans-serif; font-weight: 620; }
```

### Other delivery hosts (same paths)

```
Statically:    https://cdn.statically.io/gh/maxlair1/open-typefaces/v1.1.0/fonts/aspekta/aspekta.css
GitHub Pages:  https://maxlair1.github.io/open-typefaces/fonts/aspekta/aspekta.css   (if Pages is enabled)
```

> Pin to a **tag** (`@v1.1.0`) or **commit SHA** in production for an immutable, cache-friendly URL.

## Attribution

Aspekta is © 2025 Ivo Dolenc, under the OFL with the Reserved Font Name "Aspekta". The OFL does not require attribution in your UI, but the license must travel with the font files — which is why [`OFL.txt`](./OFL.txt) sits beside them here. Don't rename the font (Reserved Font Name) and don't sell the fonts on their own.
