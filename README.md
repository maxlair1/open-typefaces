# open-typefaces

Self-hosted CDN of openly-licensed fonts, served from this repo via [jsDelivr](https://www.jsdelivr.com/). Each typeface ships ready `@font-face` CSS — load it and set `font-family`. Pin to a tag in production.

```html
<link rel="stylesheet" href="https://cdn.jsdelivr.net/gh/maxlair1/open-typefaces@v1.2.0/fonts/aspekta/aspekta.css">
```

**Fonts:** a dozen typefaces vendored here (sans · serif · mono · display) — see [`fonts.json`](./fonts.json). [`reference.json`](./reference.json) is a flat `{font, href, source}` sheet of ready CDN URLs, including a few hosted elsewhere (Chivo, Iosevka, Pretendard via their own CDNs).

Adding fonts (redistributable licenses only): [`docs/adding-a-typeface.md`](./docs/adding-a-typeface.md). Scaffolding [MIT](./LICENSE); each font keeps its own license.
