---
name: add-typeface
description: Add an openly-licensed typeface to this open-typefaces CDN repo. Use whenever the user wants to add, include, vendor, or host a new font/typeface here — usually given a GitHub repo or font source URL ("add <font>", "include <url>", "can you host X"). Vets the license, checks whether the font is already on a real CDN, converts to woff2 if needed, generates the @font-face CSS, registers it in fonts.json + the README table, commits under the maxlair1 account, and tags a release. The human-readable companion is docs/adding-a-typeface.md.
---

# Add a typeface to open-typefaces

`open-typefaces` is a self-hosted CDN of openly-licensed typefaces served from this
public GitHub repo via jsDelivr / Statically. Each typeface ships a ready-to-use
`@font-face` stylesheet whose `src` URLs are **relative**, so one `<link>` loads the
fonts from whatever host serves the CSS. Follow the steps below in order. Do the work
in a temp dir, assemble into `fonts/<id>/`, then commit + tag.

`<id>` is always **kebab-case** (e.g. `drafting-mono`, `aspekta`). Look at the existing
`fonts/drafting-mono/` folder as the reference shape before you start.

## 0. Vet the license — hard gate

Only add fonts whose license permits **redistribution AND web embedding**: OFL-1.1,
Apache-2.0, MIT, Ubuntu Font License, and similar. Find the license file in the source
(`LICENSE`, `LICENSE.txt`, `OFL.txt`).

- GitHub's API may report `"Other"` for OFL — that's a detection gap, not a problem.
  Open the file and confirm it says "SIL Open Font License".
- If the license is absent, "free for personal use only", "demo", or unclear → **STOP**.
  Tell the user and do not add it.

## 1. Check it isn't already on a real CDN

The user only wants to vendor fonts that *aren't* already conveniently hosted. Before
doing the work, check — and if it IS already on a real CDN, report that and ask whether
they still want a curated copy here before proceeding.

```bash
ID=<kebab-id>
# Fontsource (most common — it's npm → jsDelivr under the hood)
curl -sS -o /dev/null -w "fontsource static   -> %{http_code}\n" "https://cdn.jsdelivr.net/npm/@fontsource/$ID/index.css"
curl -sS -o /dev/null -w "fontsource variable -> %{http_code}\n" "https://cdn.jsdelivr.net/npm/@fontsource-variable/$ID/index.css"
# An npm package published by the author (check the README for the package name)
curl -sS -o /dev/null -w "npm registry        -> %{http_code}\n" "https://registry.npmjs.org/<pkg-name>"
```

Also check Google Fonts (web search). **Caveat:** the source repo's own
`https://cdn.jsdelivr.net/gh/<owner>/<repo>/...` path returning 200 does **not** count as
"already on a CDN" — that's the same DIY trick this repo uses, and it has no ready
`@font-face` CSS. Vendoring it here (clean URLs + generated CSS + pinned version) is
still worthwhile.

## 2. Get web-ready woff2

`woff2` only — it's the smallest format and every current browser supports it.

- **If upstream already ships woff2** (often under `fonts/woff2/`, `webfonts/`, or
  `packages/fonts/`), just copy them. `git clone --depth 1 <repo>` into `/tmp`.
- **If upstream only has ttf/otf**, convert (this also produces a variable woff2 from a
  variable ttf):

  ```bash
  python3 -m venv /tmp/woff2venv && /tmp/woff2venv/bin/pip install -q "fonttools[woff]" brotli
  /tmp/woff2venv/bin/python3 - <<'PY'
  from fontTools.ttLib import TTFont
  f = TTFont("Input.ttf"); f.flavor = "woff2"; f.save("out.woff2")
  PY
  ```

For a **variable** font, read the real axis range (needed for the CSS `font-weight`
range) — read the `.ttf` variable file so you don't need brotli:

```bash
/tmp/woff2venv/bin/python3 - <<'PY'
from fontTools.ttLib import TTFont
for a in TTFont("VariableFont.ttf")["fvar"].axes:
    print(a.axisTag, a.minValue, a.defaultValue, a.maxValue)
PY
```

## 3. Lay out the folder

```
fonts/<id>/
├── README.md          # copy fonts/drafting-mono/README.md and edit
├── metadata.json      # copy _template/metadata.json and fill in
├── OFL.txt            # the FONT'S OWN license — REQUIRED, sits beside the binaries
├── <id>.css           # @font-face for the static weights
├── <id>-variable.css  # @font-face for the variable font (only if one exists)
└── files/
    ├── <id>-<weight>.woff2          # e.g. aspekta-400.woff2 (use the real weight numbers)
    ├── <id>-<weight>-italic.woff2   # only if italics exist
    └── <id>-variable.woff2          # if variable
```

Also copy any `AUTHORS`/`FONTLOG` provenance files if upstream has them. Name static
files `<id>-<weight>.woff2`. Use **named** weights when the font uses the standard 9
(`thin/extralight/light/regular/medium/semibold/bold/extrabold/black`) or **numeric**
weights when it uses non-standard steps (e.g. Aspekta's `50`–`1000` in 50s → `aspekta-450.woff2`).

## 4. Generate the CSS (programmatically — never hand-type the blocks)

Relative `src` URLs are mandatory. Generate the static CSS in a loop so weights and
filenames can't drift:

```bash
FD=fonts/$ID
printf '/* %s — static weights (woff2). License: see ./OFL.txt. Upstream: %s\n * Relative src URLs: one <link> to this file loads the woff2 from the same host. */\n' "$NAME" "$UPSTREAM" > "$FD/$ID.css"
# rows: "weight:style:filename"  (style is normal|italic)
for r in "100:normal:$ID-thin.woff2" "400:normal:$ID-regular.woff2" "700:normal:$ID-bold.woff2"; do
  IFS=':' read -r wght style file <<< "$r"
  cat >> "$FD/$ID.css" <<EOF

@font-face {
  font-family: '$NAME';
  font-style: $style;
  font-weight: $wght;
  font-display: swap;
  src: url('./files/$file') format('woff2');
}
EOF
done
```

Variable CSS uses a `font-weight` **range** and the variations hint:

```css
@font-face {
  font-family: 'Font Name';
  font-style: normal;
  font-weight: <min> <max>;
  font-display: swap;
  src: url('./files/<id>-variable.woff2') format('woff2-variations');
}
```

Follow the global CSS style rule: properties flush left, single space after the colon,
no aligned/tabular spacing.

## 5. Fill in metadata.json

Copy `_template/metadata.json`. Set `id`, `name`, `category`, `designer`, `version`,
the `license` block, `copyright` (from the OFL header), `source.upstream` + `addedAt`
(today's date), `cssFamily`, the `css` filenames, `weights`, `styles`, `axes` (for
variable fonts), and `variable: true|false`.

## 6. Register the typeface

- Add a row to the **Typefaces** table in the top-level `README.md`.
- Add an entry to `fonts.json` under `typefaces`.
- (Optional) add a section to `index.html` so the preview page showcases it.

## 7. Commit, tag, push — under the maxlair1 account

This is a **personal** repo on the `maxlair1` GitHub account, but the machine's active
`gh` account is usually the work account (`Max-Lair_rey`) and pushes go over SSH for
both. To push without SSH-key ambiguity and without leaking the work email:

```bash
cd /Users/lairmaxx/Code/open-typefaces
# commit attributed to maxlair1's GitHub no-reply (local config only — set once)
git config user.name "maxlair1"
git config user.email "75456507+maxlair1@users.noreply.github.com"
git add -A && git commit -m "Add <Name> (<license>)"

# push as maxlair1 over HTTPS via gh's credential helper, then restore the work account
gh auth switch --user maxlair1
git push origin main
NEXT=v1.<n>.0   # bump from the latest tag
git tag -a "$NEXT" -m "Add <Name>"
git push origin "$NEXT"
gh auth switch --user Max-Lair_rey   # ALWAYS restore the work account
```

The repo already has a **local-only** credential helper (`credential.https://github.com.helper`
→ `gh auth git-credential`) and an HTTPS `origin`, so the push uses whichever account is
active in `gh`. Never modify global git config for this.

## 8. Verify on jsDelivr

```bash
REF=v1.<n>.0
curl -sS -o /dev/null -w "css -> %{http_code} %{content_type}\n" "https://cdn.jsdelivr.net/gh/maxlair1/open-typefaces@$REF/fonts/$ID/$ID.css"
curl -sSI "https://cdn.jsdelivr.net/gh/maxlair1/open-typefaces@$REF/fonts/$ID/files/$ID-regular.woff2" | grep -iE "^HTTP|content-type|access-control-allow-origin"
```

Expect `200`, `text/css` for the stylesheet, `font/woff2` + `access-control-allow-origin: *`
for the fonts. Report the ready-to-use `<link>` snippet to the user.
