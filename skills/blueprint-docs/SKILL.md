---
name: blueprint-docs
description: House style for standalone HTML documents — plans, investigations, reports, proposals, specs, RFCs, postmortems, design docs, briefs. Use whenever writing or restyling a single-file HTML document, or when the user says "plan doc", "make it match my usual style", or "the blueprint style".
---

Every HTML document is a **blueprint**: a technical drawing, not a web page. Hairline rules, hatched separators, monospace annotation, nothing rounded, nothing glowing. The page looks measured — like it was drafted, not designed.

Do not improvise the visual system. Inline [`assets/blueprint.css`](assets/blueprint.css) verbatim into a `<style>` block and build on [`assets/skeleton.html`](assets/skeleton.html). Output is always **one self-contained `.html` file** — no build step, no external CSS/JS, with exactly two allowed network dependencies: the Google Fonts `@import` already in the sheet, and the `@pierre/diffs` ESM import when the document contains a code diff (see **Diffs**).

**Build mechanically, don't re-type.** Generate documents by reading the two asset files and splicing programmatically (read `blueprint.css` into the `<style>` block; copy the skeleton's `<script>` blocks whole). Never paste the sheet or the scripts through a template language — the skeleton script is full of `${...}` literals that collide with f-strings and template engines, and one mangled brace silently breaks the rail. To verify light mode, load the page with `?theme=light` — the head script honors it; never ship a patched copy.

The layout is deliberately dense: a 13rem rail, a 56rem content column, and compact vertical rhythm (~1.4rem between blocks). Do not widen the gutters or pad things back out — the previous, airier version of this style was rejected as a pain to use. The width is for **data**, not prose: paragraphs and lists cap at 72ch (the sheet does this) so line length stays readable, while tables, charts, diffs, and `pre` take the full column. Don't remove that cap — full-width prose was rejected as unpleasant to read.

## Invariants

Break one and the document stops reading as a blueprint.

- **Mono carries structure, sans carries prose.** JetBrains Mono for every heading, label, table header, metric, caption, diagram string, and button. Inter for paragraphs and list items only. This split is the single loudest signal of the style.
- **Hairlines, never boxes.** Every division is `1px solid var(--line)`. No `border-radius` beyond the 1–2px on inline `code` and legend chips. No `box-shadow`, ever. No gradient except `--hatch`.
- **One accent.** `--accent` for emphasis, links, active states, the dominant bar in a chart. Semantics ride `--ok` / `--warn` / `--danger` only where a value is genuinely good or bad. Nothing else gets a color.
- **Never hardcode a hex outside the palette block** — inline SVG included. Every fill, stroke, and text color is a token, or the document breaks the moment it re-themes.
- **Full-bleed blocks.** Prose sits inside `--pad`; every block element — `pre`, `.table-wrap`, `.diagram`, `.metric-line`, `h1`, `h2` — escapes it with `margin-inline: calc(-1 * var(--pad))` and rules itself with `border-block`. The eye should see block edges touching the ruled column lines.
- **Hatch marks the seam.** The `-45°` hatch band appears under the topbar and, via `h2::before`, above every section but the first. It is the document's punctuation.
- **Ruled columns.** The 4-column `.page-grid` draws vertical hairlines down the whole page. Topbar, hatch cell, and content all carry `border-inline` so the rules run unbroken from header to footer.
- **Uppercase mono micro-labels** with `letter-spacing: .16em`–`.18em` for kickers, rail labels, metric labels, and top actions.

## Theming

Both palettes ship in the sheet and both are load-bearing — the document is not a dark document that tolerates light.

- Dark is the CSS default. The inline `<head>` script resolves `localStorage` → system preference → dark and sets `data-theme` on `<html>` **before first paint**; keep it in `<head>` or the theme flashes. `:root[data-theme="light"]` is the only override block.
- Light is warm greige paper (`#e8e3d9`), decidedly not white — if a screenshot of light mode could pass for `#fff`, it has drifted. Its separation from white comes from **lightness** (~1.28:1 vs `#fff`), not hue: cool-white-point external monitors flatten subtle off-white tints to pure white, but a genuinely darker page survives any display. Do not "tastefully" lighten it. The accent darkens to `#3849b8` and the verdict colors deepen — the dark values fail contrast on paper.
- Each theme ships a six-step background ramp — `--bg-sunken` · `--bg` · `--bg-tint` · `--bg-inset` · `--bg-hover` · `--bg-raised` — plus `--accent-soft` for accent-tinted fills. Pick from the ramp instead of `color-mix`ing new surfaces: tables sit on `--bg-tint`, pre/diagrams on `--bg-inset`, hover states on `--bg-hover`, table headers and tooltips on `--bg-raised`.
- The topbar carries a `LIGHT`/`DARK` toggle that reads out the theme you'd switch *to*, and persists the choice.
- Both palettes are contrast-validated: every text token clears WCAG AA against its background, and every diagram label clears 3:1 against its bar.
- Adding a color means adding it to **both** blocks. A token defined in only one is a bug.

## Voice

The prose is as much of the style as the CSS.

- **Explain simply.** It takes no skill to explain something in a complicated way; the skill is explaining a complicated thing simply. Plain words, short sentences, one idea per sentence, jargon expanded the first time it appears. Complexity in the subject never excuses complexity in the prose: if a sentence needs a second read, rewrite it.
- Lead with the number. "12.478 s (82.9% of that path) was the eager module graph," not "module loading was slow."
- **No em dashes**, anywhere in the document. Where one tempts you, use a comma, a colon, a period, or parentheses.
- `<strong>` is for load-bearing figures and verdicts, not for enthusiasm.
- Every document opens: `.kicker` (3 dot-separated status words) → `h1` (a full clause, not a noun phrase) → `.lede` (the whole finding in 3–5 sentences, numbers included).
- State confidence and non-goals explicitly. Say what was not measured.
- No exclamation marks, no emoji, no "we're excited to."

## Composition

Reach for these in roughly this order; a document need not use all of them.

- `.metric-line` — 2–4 headline figures, flush, directly under the first `h2`. Values get `.good`/`.bad` when the number carries a verdict.
- `blockquote.callout` — one per document at most, for the recommended decision.
- `.table-wrap > table` — the workhorse. Milestones, findings-with-confidence, before/after. Right-hand columns take `.good`/`.warn`/`.bad`.
- `pre` — commands and config, with a leading `#` comment line naming what it is.
- `.diff-block` — every code change, rendered with Pierre diffs. See **Diffs**.
- `.diagram` — see below.
- `.caption` — environment, method, or caveat under a `pre` or diagram.
- `.source` — one mono line under a table, chart, or `pre` naming where the data came from. See **Provenance**.
- `.chip` (+ `.ok`/`.warn`/`.bad`/`.accent`) — inline uppercase markers for workflow states (<code>DRAFT</code>, <code>VERIFIED</code>, <code>BLOCKED</code>), confidence levels, and data properties (<code>ESTIMATED</code>, <code>LEFT-CENSORED</code>, <code>SAMPLED</code>). A chip states a fact about status or data; it never decorates.
- `details.appendix` — collapsed raw evidence at the document foot. See **Provenance**.
- `.colophon` — the last element of every document.

## Numbers

The style leads with numbers, so render them with discipline:

- Digits align — the sheet applies `tabular-nums` to metrics and table cells; never override it.
- Consistent precision within a column or comparison: `2.091 s` next to `15.053 s`, not `15.1 s`.
- Units always, thousands separators always (`1,380.390 ms`), and no bare percentages when the base is ambiguous — `63.9% of 2.16 s` beats `63.9%`.
- A delta states its direction and base: `+160 px vs before`, not `160`.

## Provenance

A number a reader cannot trace is an opinion.

- Every table, chart, and quoted figure gets a `.source` line naming the system, query, file, or command it came from — specific enough that someone could re-run it.
- Raw evidence — full queries, unabridged output, long tables — goes in `details.appendix` at the foot, collapsed. In the body, show top-N rows and say in the `.source` line what was cut and by what rule.
- Every document ends with a `.colophon`: when it was generated, by whom or what, from which inputs (commit, data window, environment), and its revision.

## Anchors

The skeleton's script appends a hover-visible `§` to every `h2`/`h3`; clicking it navigates and copies the absolute link. This is why every heading must have an `id` — sections get shared in chat, so their links must be grabbable.

## Rail

The rail must earn its 13rem. The skeleton's script already does two things — keep them working:

- It mirrors the first `.metric-line` into a **Key figures** rail block (verdict colors included), so the headline numbers stay visible while the reader scrolls. Documents with metrics get this for free; never duplicate it by hand.
- It collects every file the document references into an **Artifacts** rail block of download links — links and images are picked up automatically by extension (zip, csv, parquet, png, pdf, log, …); force-include anything else with `data-artifact` (empty on an `<a>` to use its href, or `data-artifact="url"` on any element), and override the shown name with `data-artifact-name`. Every dataset, image, or archive the prose mentions should therefore be a real relative link, uploaded alongside the document — an artifact named but not linked is invisible to the rail and undownloadable for the reader.
- It hides the outline when the document has fewer than 3 headings — a two-line outline is wasted space.

If a document has neither metrics nor 3+ headings, the rail carries only the Document block; that is fine, it stays slim.

## Diffs

Never present a code change as two side-by-side `pre` blocks or a plain unified patch dumped in a `pre`.

- **Default: `@pierre/diffs`** ([diffs.com](https://diffs.com)) — import at runtime from a CDN, render into an empty `.diff-block`. Pierre has two layout styles, `split` (side by side) and `unified` (stacked); default to `split` unless the change is trivially short, and give every Pierre diff a `.diff-toggle` in its title bar so the reader can switch. Always set `overflow: "wrap"` — a diff that scrolls sideways hides the change.

  ```html
  <div class="diff-block" id="diff-1">
    <p class="diff-title">path/to/file.ts · what changed, in one clause
      <button class="diff-toggle" type="button">STACKED</button></p>
  </div>
  <script type="module">
    import { FileDiff } from "https://esm.sh/@pierre/diffs@1";
    const pageTheme = () => document.documentElement.dataset.theme === "light" ? "light" : "dark";
    const diff = new FileDiff({
      theme: { dark: "pierre-dark", light: "pierre-light" },
      themeType: pageTheme(),
      diffStyle: "split",
      overflow: "wrap",
    });
    diff.render({
      oldFile: { name: "path/to/file.ts", contents: OLD },
      newFile: { name: "path/to/file.ts", contents: NEW },
      containerWrapper: document.getElementById("diff-1"),
    });
    new MutationObserver(() => {
      if (diff.options.themeType === pageTheme()) return;
      diff.setOptions({ ...diff.options, themeType: pageTheme() });
      diff.rerender();
    }).observe(document.documentElement, { attributes: true, attributeFilter: ["data-theme"] });
    document.querySelector("#diff-1 .diff-toggle").addEventListener("click", (e) => {
      const next = diff.options.diffStyle === "split" ? "unified" : "split";
      diff.setOptions({ ...diff.options, diffStyle: next });
      diff.rerender();
      e.currentTarget.textContent = next === "split" ? "STACKED" : "SPLIT";
    });
  </script>
  ```

  Like the theme toggle, the button reads out the layout you'd switch *to*. The exact option names move between versions — if this errors, check the vanilla-JS docs at diffs.com rather than hand-rolling a workaround. **`themeType` and the observer are mandatory**: Pierre's `themeType` defaults to `"system"` (the OS `prefers-color-scheme`), so without them the diff ignores the page's `data-theme` toggle and renders in whatever mode the reader's OS is in.
- **Annotations** — Pierre renders a comment block against a specific line, and that is where "why this line changed" prose belongs: on the line, not in a paragraph three blocks away. However it is optional, not every diff needs one; reach for it when the explanation is about one line rather than the whole change. Pass `lineAnnotations` to `render()` and a `renderAnnotation` callback in the options that returns a `.diff-note` element (the sheet styles it as a `//` code comment):

  ```js
  const diff = new FileDiff({
    /* ...options as above... */
    renderAnnotation: (a) =>
      Object.assign(document.createElement("p"), { className: "diff-note", textContent: a.metadata }),
  });
  diff.render({
    /* ...files as above... */
    lineAnnotations: [{ side: "additions", lineNumber: 12, metadata: "why this line changed, in one clause" }],
  });
  ```
- **Fallback: `pre.diff`** — only when the document must open with no network (air-gapped review, email attachment). One `<span>` per line, classed `add` / `del` / `hunk`, exactly as in the skeleton's Evidence section. Say in the caption that the diff is abridged if it is.

## Charts

Prefer a chart over prose or a table whenever the point *is* a comparison, trend, distribution, or share — three or more related numbers usually deserve one. A table answers "what are the values"; a chart answers "which one dominates" — pick by the question the section is asking, and don't do both for the same numbers unless the table adds columns the chart can't carry (interpretation, confidence).

## Diagrams

Hand-author inline `<svg>`. Never a chart library, never a raster image — charts stay hand-drawn so every mark is a theme token.

**The style constrains how marks are drawn — never which chart form to use.** Form follows the question the data answers, chosen per chart on merit: a dense time series (more than ~20 points) wants a line; a handful of periods or categories wants columns/bars; composition wants stacked bars; relationship wants a scatter. If the form fights the data — sixty unreadable skinny columns, a line through unordered categories — the form is wrong, not the data. When an existing document already displays the data in a form that works, keep the form and restyle it; a redesign earns a form change only when the new form answers the question better. When you do change a chart's form, keep its viewBox and scale — annotations, gridlines, and axis marks then transfer without recomputation.

- `viewBox="0 0 760 H"`, height to fit. Plot area x≈16–718.
- Wrap in `.diagram` with a `.diagram-title` above and, when the encoding needs decoding, a `.legend` below. `role="img"` + `aria-label` on the wrapper; `aria-hidden="true"` on the `<svg>`.
- **Tokens only, no hex.** `fill="var(--series-2)"`, `stroke="var(--line-strong)"`. Axis text `var(--fg-muted)`, data labels `var(--fg)`, labels sitting on a series bar `var(--on-fill)`, on the accent bar `var(--on-accent)`. Legend chips take `style="--legend:var(--series-2)"`.
- Series ramp toward the accent: `--series-1` → `--series-4`, with `--ok` / `--warn` / `--danger` reserved for verdicts. `--series-1` is the neutral/baseline comparison series (the peers, the control, the before); the flagged or focal series takes `--series-3` — or `--danger` when it is genuinely the bad actor.
- The accent rule generalizes to charts: annotations (median/threshold lines, event markers — dashed) and the one element the document is about take `var(--accent)`.
- **Time axes** (whatever the mark): gridlines `var(--line)`, the zero baseline `var(--line-strong)`, axis tick labels in `var(--fg-muted)` at 10px, ticks at round intervals (every 5–10 min, not every point). Annotate events as dashed `var(--accent)` verticals labelled in place. Label the peak on the mark — above the dot; when the document argues about the floor, annotate the minimum too, label below the dot, both kept ~7–13 units clear of the stroke. Place a median/threshold label at the end of the axis where the series is absent or furthest from the line; if the series occupies both ends, break the dash under the label rather than exiling it to the legend. When a bar is too short to hold its label, place the label outside it in `var(--fg)`.
- Lines are plain 2px solid `stroke` polylines: no curve smoothing, no area fill, dots only at annotated extremes. For tooltips over a line, lay invisible hit `<rect>`s per interval carrying the `data-tip` — `fill="transparent"` (`fill="none"` is not hoverable), full plot height, emitted last in the SVG so they sit above the stroke, and stopped short of the zero baseline so a flat-series band underneath keeps its own tip.
- Give each hit rect `data-guide` with the series' y at that interval (viewBox units; space-separate several ys when several series share the interval) and optionally `data-guide-fill="var(--danger)"` to match the series. The skeleton then draws the hover indicator — dashed vertical guide, dot(s) on the line, soft column highlight — for free; a line chart without `data-guide` reads as broken to the pointer. The guide sits at the hit rect's horizontal center, so center each rect on its data point; when rects must tile edge-to-edge instead, set `data-guide-x` to the point's x. For a flat-series band, use the band's center y so the dot straddles it. Omit a series' y where it has no datum for that interval — never substitute the baseline, which reads as a zero measurement rather than absent data; the lone dot's default accent then correctly reads as annotation, not measurement.
- Hit rects cover the full axis, not just the span the series covers — intervals where the series is absent get a tip saying so; absence is usually the finding.
- **A legitimately zero/flat series is a finding, not an omission**: draw it as a thin (~2px) band on the axis with a `data-tip` saying flatness is the point — never silently drop it.
- Annotate directly on the mark — a dashed accent line labelled `median 2.091 s` beats a legend entry.
- Bars are plain `<rect>`: no rounding, no stroke, no opacity.
- **Every data mark carries `data-tip`** with the full figure the on-mark label abbreviates — name, exact value, share, caveat — using `&#10;` for line breaks. The skeleton's tooltip script and the sheet's hover highlight do the rest; a chart whose bars ignore the pointer reads as broken.

For structure rather than quantity — request paths, topologies, before/after architectures — the same `.diagram` wrapper holds a boxes-and-arrows SVG:

- Nodes are plain `<rect>` (no rounding), `fill="var(--bg-raised)"` `stroke="var(--line-strong)"`, with a centered mono label in `var(--fg)`.
- Edges are 1px `var(--line-strong)` straight or right-angled lines with small triangular arrowheads (a `<path>` filled the same — `<marker>` can't take CSS variables in all engines).
- The path or component the document is about takes `var(--accent)` (edge and node stroke); a genuinely failing element takes `var(--danger)`. Everything else stays neutral — an architecture diagram with five colors is a legend, not a drawing.
- Nodes carry `data-tip` with what the reader would otherwise have to ask: role, scale, the number that matters.

## Checklist

- [ ] Single file, no external assets beyond the font `@import` and (diffs only) the `@pierre/diffs` import.
- [ ] Every code change rendered through `.diff-block` — Pierre by default (split layout, `overflow: "wrap"`, `.diff-toggle` in the title bar), `pre.diff` fallback only when offline is required. If Pierre is used, open the file and confirm the diff rendered in both themes, the toggle switches between split and stacked, and long lines wrap instead of scrolling.
- [ ] Comparisons/trends of 3+ numbers are charted, not left as prose.
- [ ] Every chart mark has a `data-tip`; hover one and confirm the tooltip follows the pointer. On a line chart, also confirm the hover guide draws and its dot lands on the polyline (`data-guide` y matches the vertex).
- [ ] Every table/chart has a `.source` line; the document ends with a `.colophon`.
- [ ] Raw evidence is present but collapsed in `details.appendix`, not dumped in the body or dropped.
- [ ] Every artifact the document mentions is linked, appears in the rail's Artifacts block, and resolves at the hosted location (upload the files with the document).
- [ ] Numbers carry units and consistent precision; deltas state direction and base.
- [ ] Every `h2`/`h3` has an `id` — the rail outline and reading progress are generated from them by the skeleton's script.
- [ ] `<title>` set; `.kicker` present; rail auto-fills.
- [ ] Zero `border-radius` on blocks, zero `box-shadow`, zero non-hatch gradients.
- [ ] Every block element bleeds to the column rules.
- [ ] **No hex outside the two palette blocks** — grep the file for `="#` and `:#` (not bare `#`, which drowns in HTML entities like `&#10;`).
- [ ] **Read the document in both themes.** Toggle, reload, confirm the choice persisted and nothing flashed.
- [ ] Narrow viewport: rail collapses, `border-inline` drops, nothing overflows.
