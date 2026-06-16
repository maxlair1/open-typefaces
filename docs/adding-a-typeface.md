# Adding a typeface

## 0. Check the license first

Only add fonts whose license **permits redistribution and web embedding**:
OFL, Apache-2.0, MIT, Ubuntu Font License, and similar. Skip anything marked
"free for personal use only", "demo", or with no clear license. When in doubt,
find the license file in the font's source repo — if there isn't one, don't add it.

## 1. Get web-ready files

You want `woff2` (every current browser supports it; it's the smallest format).

- If upstream already ships `woff2` (many GitHub font repos do, under `fonts/woff2/`), just copy them.
- If upstream only has `ttf`/`otf`, convert:

  ```bash
  python3 -m venv .venv && .venv/bin/pip install "fonttools[woff]" brotli
  .venv/bin/python3 - <<'PY'
  from fontTools.ttLib import TTFont
  f = TTFont("Input.ttf"); f.flavor = "woff2"; f.save("output.woff2")
  PY
  ```

  Variable fonts compress the same way — keep the variable file as a single `woff2`.

## 2. Lay out the folder

```
fonts/<id>/
├── README.md                 # copied from _template, then edited
├── metadata.json             # copied from _template, then filled in
├── OFL.txt                   # the FONT'S OWN license — REQUIRED, must sit here
├── <id>.css                  # @font-face for the static weights
├── <id>-variable.css         # @font-face for the variable font (if any)
└── files/
    ├── <id>-regular.woff2
    ├── <id>-bold.woff2
    ├── <id>-variable.woff2   # if variable
    └── ...
```

`<id>` is kebab-case, e.g. `drafting-mono`. Name weight files
`<id>-<weight-name>.woff2` and `<id>-<weight-name>-italic.woff2`.

## 3. Write the CSS

Use **relative** `src` URLs so the stylesheet works from any host:

```css
@font-face {
  font-family: 'Font Name';
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url('./files/<id>-regular.woff2') format('woff2');
}
```

For a variable font, give `font-weight` a range and use the variations hint:

```css
@font-face {
  font-family: 'Font Name';
  font-style: normal;
  font-weight: 100 900;
  font-display: swap;
  src: url('./files/<id>-variable.woff2') format('woff2-variations');
}
```

See [`fonts/drafting-mono/`](../fonts/drafting-mono/) for a complete worked example.

## 4. Register and release

1. Add the typeface to the table in the top-level [`README.md`](../README.md).
2. Add an entry to [`fonts.json`](../fonts.json).
3. Commit, then **tag a release** so consumers get an immutable URL:

   ```bash
   git tag -a v1.1.0 -m "Add <name>"
   git push origin main --tags
   ```

   jsDelivr and Statically pick up the new tag automatically. Consumers pin to it:
   `https://cdn.jsdelivr.net/gh/maxlair1/open-typefaces@v1.1.0/fonts/<id>/<id>.css`
