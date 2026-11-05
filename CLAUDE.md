# Action Editorial Library

Static reference site for Action Network editors and writers: approved article components (live preview + copyable code), CTA insert steps, a style guide and a CTA placement playbook. Audience is non-technical — every interaction should be "find it → check it on mobile → copy it." See README.md for the full brief, open placeholders and roadmap.

## Stack
- Single `index.html`, no build step. Open it directly in a browser, or serve with `python3 -m http.server` (needed once content is split into fetched JSON/HTML files).
- Hosted on GitHub Pages from `main` / root.
- All content lives in three arrays at the top of the `<script>` (marked `EDIT HERE`): `CATEGORIES`, `COMPONENTS`, `PLAYBOOK`. Style guide and CTA principles are hand-written HTML in `<main>`.
- Previews render in a Shadow DOM host so site CSS can't leak in; stage is always white. Desktop/Mobile toggle = 720px / 375px column.
- External scripts only from cdnjs (pin exact versions). Fonts from Google Fonts.

## Hard rules for components (never break)
1. **Inline styles only** in component `code` — no `<style>`, classes or media queries (snippets are pasted into CMS HTML blocks).
2. **Mobile-safe at 375px** without media queries: `flex-wrap` + `min-width` on flex children; every table wrapped in an `overflow-x:auto` container.
3. **Tables:** ≤5 columns, numbers right-aligned with `font-variant-numeric:tabular-nums`, source/timestamp line underneath.
4. **CTAs are never `kind:"html"`.** Every affiliate/PRO/app CTA is `kind:"cms"`: preview + insert steps, never copyable CTA HTML.
5. **No Infogram.** Charts are native HTML (`chart-image`) or template images.
6. **Brand:** green `#00c358` (text on light bg: `#00793a`), navy `#0a1628`, DM Sans. Oswald for display on the library site only.
7. **Odds style:** `+150` / `-110`; `Bills +3.5 (-110)`; `Over 47.5` in prose, `O 47.5` in tables; `8:20 p.m. ET`.
8. Every `[bracket]` is a writer placeholder. Keep the convention; don't fill in real values unless Steve provides them.

## Acceptance checks for any change
- Every component preview looks right at 375px and 720px.
- Copy produces exactly the `code` string, and it renders the same pasted into a blank HTML file.
- No horizontal page scroll at 375px; works in light and dark.
- Search still finds each component by name.
