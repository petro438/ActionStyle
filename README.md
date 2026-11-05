# Action Editorial Library

A public, free-hosted reference site for Action Network editors and writers. One place to:
1. **See and copy** approved article components (tables, visual elements, tool embeds), each with a live preview and paste-ready code.
2. **Look up** the approved CTAs and how to insert them as tracked CMS blocks.
3. **Read** a short style guide and a CTA placement playbook organized by story type.

Why it exists: articles ship inconsistently. Tables are too wide on mobile, Infogram embeds are slow and off-brand, writers hand-build CTAs with no tracking, stories go out without images, and there's no shared internal-linking plan. The library is the single source of truth that fixes this.

Audience: non-technical editors and writers. Every interaction should be "find it → check it on mobile → copy it."

## Current state

`index.html` is a working v1, a single static file with no build step. Open it in a browser to see it. It's data-driven: all content lives in three arrays at the top of the `<script>` (marked `EDIT HERE`):

- `CATEGORIES`: id, name, blurb (tables, visuals, embeds, ctas, images)
- `COMPONENTS`: one object per component (fields below)
- `PLAYBOOK`: rows of `[story type, primary CTA, placement, secondary, avoid]`

The style guide and the CTA principles are hand-written HTML in `<main>`.

### Component object

```js
{
  id: "odds-table",        // anchor + DOM id, kebab-case, unique
  cat: "tables",           // CATEGORIES id
  name: "Odds table",
  kind: "html" | "cms",    // html = paste-ready code; cms = insert as CMS block (code holds steps)
  sample: true,            // preview uses example numbers → shows "Sample data" tag
  use: "When to use it…",  // one or two sentences
  notes: ["…"],            // short usage rules
  code: `…`,               // what the Copy button copies
  preview: `…`             // optional; overrides what the stage renders (CTAs, iframes, images)
}
```

### How the page renders

- Each preview renders inside a **Shadow DOM** host, so the site's CSS can't leak in. The stage is always white, to look like an article.
- The Desktop/Mobile toggle constrains the article column to 720px or 375px.
- The Copy button uses `navigator.clipboard`, with a select-text fallback.
- The left index has search (filters cards by name, use, category and notes) and scroll-spy. On mobile it collapses to a horizontal nav.
- The site theme follows light/dark via CSS tokens on `:root`. Article previews stay light.

## Hard rules for components (do not break)

1. **Inline styles only** in component `code`. No `<style>` blocks, classes or media queries, because the snippets get pasted into CMS HTML blocks.
2. **Mobile-safe at 375px** without media queries: use `flex-wrap` and `min-width` on flex children, and wrap every table in an `overflow-x:auto` container.
3. **Tables:** 5 columns max, numbers right-aligned with `font-variant-numeric:tabular-nums`, plus a source/timestamp line underneath.
4. **CTAs are never `kind:"html"`.** Every affiliate, PRO or app CTA is a CMS block, so clicks are tracked. The library shows a preview and the insert steps, never copyable CTA HTML.
5. **No Infogram.** Charts are native HTML (see `chart-image`) or images from our template.
6. **Brand tokens:** green `#00c358` (on light backgrounds, use `#00793a` for text), navy `#0a1628`, DM Sans. Oswald for display on the library site only.
7. **Odds style:** American odds with a sign (`+150`, `-110`), spread with the price (`Bills +3.5 (-110)`), totals as `Over 47.5` in prose and `O 47.5` in tables, times as `8:20 p.m. ET`.
8. Every `[bracket]` is a placeholder for the writer to replace. Keep that convention.

## Placeholders still to fill (from Steve)

- Real CMS block names and insert steps for: Sportsbook CTA, Inline sportsbook link, PRO CTA, App CTA, Prediction market widget, Featured image
- CTA placement tags (`under-pick`, `under-odds-table`, `end-of-story`, `inline`: confirm the real values)
- Responsible gaming line (exact text from compliance)
- Approved image sources (Getty / Imagn / AP?) and graphics tool (Canva / Figma?)
- Style questions: city vs. nickname on first reference; links per article (3–5?)
- Slack channel for component requests, owner, last-updated date
- Links to Storybook, TCL, the approved tool URL list and the weekly priority-link list
- CTA playbook needs a pass from the revenue team (currently a draft)

## Hosting

GitHub Pages from a public repo (`petro438/<repo>`), served from `main` / root. No build step. Optionally add a custom domain later.

On GitHub Pages, iframes work. That means the tool-embed preview can load a real tool instead of the striped placeholder, which was only needed in the earlier sandbox.

## Suggested next iterations

Pick and order as needed:

1. **Split content out of the HTML** so non-devs can edit it:
   - `components/<id>.html` holds the snippet code
   - `data/components.json` holds the metadata
   - `index.html` fetches both on load
   - Keep a `components.json` schema check in a GitHub Action.
2. **Syntax highlighting** for code blocks, using a small library (Prism or highlight.js from cdnjs).
3. **Pull in real components from Storybook and TCL.** Link to each one's Storybook page, or embed Storybook iframes for the ones that exist there.
4. **More components:** best-bets card (several picks), injury report table, props table (player / prop / line / book), futures odds table, live line-movement chart, betting-percentages (public %) block, author/expert bio box, newsletter CTA.
5. **"Story templates" section:** full skeletons per story type (game preview, props, news) showing component order. This ties into the CTA playbook.
6. **Pre-publish checklist** as an interactive checklist that resets per article, using localStorage.
7. **Changelog** section, so teams see what's new.
8. **Analytics:** a simple, privacy-friendly counter (e.g. GoatCounter, free) to see which components get copied. Fire an event on Copy.
9. **Lint helper:** paste an article's HTML and flag tables wider than 5 columns, hand-built CTA links (sportsbook domains not from a block), missing alt text and Infogram embeds.

## Acceptance checks for any change

- Every component's preview looks right at 375px and 720px.
- Copy produces exactly the `code` string, and it renders the same when pasted into a blank HTML file.
- No horizontal page scroll at 375px, and the site works in both light and dark.
- Search still finds each component by name.
