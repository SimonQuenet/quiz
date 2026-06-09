# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single-page, viral "Quel personnage de Dragon Ball es-tu ?" quiz. The user takes 8 questions
and gets matched to one of 7 characters, with a generated shareable result card (image).
It is a personal learning project (the user is learning programming — explain clearly).

Everything lives in **`index.html`**: markup, CSS, and JS are all inline. There is **no build
step, no package.json, no dependencies, no tests, and no lint**. The only external runtime
dependency is the Snapchat Creative Kit Web SDK (`<script src="https://sdk.snapkit.com/...">`).

## Run / deploy / check

```bash
# Run locally — a static server is needed for navigator.share + Snapchat to work.
# (Double-clicking the file works for the quiz itself, but sharing needs http/https.)
npx serve            # then open the printed URL, e.g. http://localhost:3000

# Deploy: push to main. GitHub Pages serves it from the repo root.
git push             # live at https://simonquenet.github.io/quiz/

# There is no test/lint tooling. To sanity-check the inline JS syntax:
awk '/<script>/{f=1;next} /<\/script>/{f=0} f' index.html > /tmp/quiz.js && node --check /tmp/quiz.js
```

`navigator.share` (sharing the actual card image) and Snapchat require **HTTPS** — they silently
do nothing over plain `file://`. Use the deployed Pages URL or a local server to test sharing.

## Architecture (the parts that span multiple sections of index.html)

- **Three screens** (`#screen-start`, `#screen-quiz`, `#screen-result`) are plain `<section>`s
  toggled by `show(id)`, which hides the others. There is no router/framework.
- **Quiz data drives everything.** `QUESTIONS` is an array; each answer carries a score map `s`
  like `{goku:2, vegeta:1}`. `choose()` accumulates these into `scores`. `computeResult()` picks
  the highest-scoring character, breaking ties by the fixed `PRIORITY` order (heroes first).
  `CHARACTERS` holds per-character display data (name, emoji, image path, theme colors, tagline,
  description).
- **The shareable card is drawn on a hidden `<canvas id="cardCanvas">` (1080×1920, story format).**
  `drawCard()` paints the gradient/glow/text and the character portrait, then calls
  `canvas.toDataURL()` for the on-screen preview and `canvas.toBlob()` to build the `File` used by
  native sharing. `drawCard` and `computeResult` are **async**: they await image loading before the
  result screen is shown, so the preview and share file are ready.
- **Sharing has two paths** (`shareNative` and the Snapchat flow). `shareNative` prefers Web Share
  Level 2 (shares the PNG `File` directly) and falls back to download + copy-link on desktop. The
  Snapchat button uses Creative Kit Web via `mountSnapButton()`; if `SNAP_CLIENT_ID` is unset or the
  SDK is unavailable it falls back to `snapFallback()`.

## Critical gotchas

- **Canvas images must be same-origin.** Character portraits are loaded from `img/` (same origin as
  the page). Drawing an image from a cross-origin URL *taints* the canvas and makes `toBlob`/
  `toDataURL` throw — which breaks the entire share feature. Do **not** switch portraits to external
  URLs; keep them in `img/` (or a CORS-enabled host).
- **Portraits are optional.** If a file in `img/` is missing, `getImg()` resolves to `null` and the
  card falls back to the character's emoji. The quiz always works without the images.
- **Portrait framing:** portraits are full-body transparent PNGs; `coverDraw(...)` takes an `alignY`
  (0 = top) and the card uses `~0.04` to frame head + bust rather than the body center.
- **Config lives at the top of the `<script>`:** `SHARE_URL` (public URL used for the viral link)
  and `SNAP_CLIENT_ID` (must be a real Snap app id with the domain whitelisted for the Snapchat
  button to function).

## Adding a character

Add an entry to `CHARACTERS`, reference its key in the relevant `QUESTIONS[*].a[*].s` score maps,
add it to `PRIORITY`, and drop a portrait at the `img/...png` path named in its `img` field.

## Image assets

`img/` portraits were fetched from the public **dragonball-api.com** dev API and converted to PNG
(see `img/README.md` for the source URLs and the re-download recipe). These are copyrighted artwork;
treat them accordingly for any public/commercial use.
