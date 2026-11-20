---
name: show-me-html
description: Turn any material into a self-contained HTML document that people understand quickly — a handbook, user guide, how-it-works explainer, onboarding page, runbook, decision record, or a summary of what a whole conversation discussed and decided. Uses diagrams, small animations and interactive pieces where they explain better than text, and writes in ASD-STE100 Simplified Technical English (one idea per sentence, numbered steps, one term per thing, separate preconditions and cautions). Use this whenever the user asks to write up, document, record, summarise or explain something as a document or HTML file so others can understand it, or wants "pictures or animation to make it easier to understand" — including Chinese requests like 寫成 html、整理成說明文件、做一份 handbook／手冊／指南、把今天討論的東西總結成文件、記錄機制細節、用圖示或動畫解釋、ASD-STE100／簡明風格. Trigger even when the user only says "write it up" or "總結一下寫成文件" at the end of a discussion.
---

# Readable HTML document

The goal is one HTML file that a reader opens by double-clicking and understands without help.
The reader should be able to act on it (operate, decide, onboard, review) and find the sources.
Pictures and small interactive pieces carry what is hard to read as text: flows, timelines,
rules with thresholds, state changes, comparisons. The text is short, literal and consistent,
so nobody misreads it.

Two assets:

- `assets/page-shell.html` — **start every page from this file.** It has the colour tokens
  for light and dark, the page bar (contents menu, page title, theme switch Auto / Light /
  Dark, text size A− / A / A+), the rem-based type scale, and the phone-width rules (section 6).
- `assets/component-reference.html` — every component on the shell, with placeholder text in
  the target style: header with the "Based on" box, key points, terms, the numbered flow
  diagram, procedures, the stepper paired with the diagram, the interactive rule demo, UI state
  previews, condition → result, options, decisions, open items, sources, back to top. An HTML
  comment above each one says when to use it. Copy the components the document needs, replace
  every `[placeholder]`, and delete the rest. The diagram and the stepper are driven by the
  `NODES`, `EDGES` and `STAGES` data in its script: edit the data, not the SVG.

## 1. Decide the document type and the reader

Work out, from the request and the conversation:

- **Who reads it**: teammates, operators, new joiners, management, the user later on.
- **What they must do after reading**: operate something, make a decision, review, learn.
- **Which type it is** — this picks the sections in step 3:

| Type | Typical request |
|---|---|
| Explainer | "how X works", 機制、行為細節 |
| Handbook / guide | 手冊、使用指南、操作說明、onboarding |
| Runbook | deploy, recover, troubleshoot steps |
| Conversation summary | 總結今天討論的、整理會話內容 |
| Decision record | why option A over B, trade-offs |

A document can mix types (a summary that also has a procedure). Name the reader in one line in
your reply, so the user can correct it.

## 2. Collect the facts

The page is only useful if every statement is true.

- **Find the source of truth** for each fact: the code, config, files the user gave, or the
  conversation itself.
- For code-backed content, follow the data in every part (UI, API, jobs, apps). Copy exact
  constants, limits, error codes and UI strings. Note the file for each.
- For a conversation summary, go through the whole conversation in order. Capture what was
  asked, what was decided and why, what was built or changed (commits, files), what was tried
  and rejected, corrections the user made, and what is still open. Keep the user's decisions
  as theirs and your own calls marked as yours.
- **Never write a number, version, name or decision you did not see.** If you need one and
  cannot find it, phrase the statement so it needs none, or ask.
- Note anything surprising you find. Put it in the document and tell the user in your reply.
- **Keep a list of what you used and what you could not check.** It goes into the "Based on"
  box at the top of the page (step 3). A reader sees only the page, not what you read, so the
  page must say where its facts come from and where they stop.

## 3. Pick the sections

Choose from this menu. Use the order shown. Leave out a section with nothing to say.

| Section | Explainer | Handbook | Runbook | Summary | Decision |
|---|---|---|---|---|---|
| Header: pill label, title, 2-sentence lead (the contents menu is in the page bar) | ✓ | ✓ | ✓ | ✓ | ✓ |
| "Based on" box, directly under the lead | ✓ | ✓ | ✓ | ✓ | ✓ |
| Key points / TL;DR (3–6 bullets) | | ✓ | | ✓ | ✓ |
| Terms table | ✓ | ✓ | ✓ | if needed | if needed |
| Overview diagram (parts and flows) | ✓ | ✓ | ✓ | optional | optional |
| Before you start + procedures + cautions | ✓ | ✓ | ✓ | if any | |
| Lifecycle / timeline stepper | ✓ | optional | | ✓ (what happened in order) | |
| Interactive demo of the key rule | ✓ | optional | | | optional |
| UI or state previews | ✓ | ✓ | | optional | |
| Condition → result tables | ✓ | ✓ | ✓ (symptom → action) | | |
| Options compared (table, trade-offs, chosen) | | | | if decided | ✓ |
| Decisions made, with the reason | | | | ✓ | ✓ |
| Changes delivered (commits, files, where) | | | | ✓ | |
| Open items / next steps / known limits | ✓ | ✓ | ✓ | ✓ | ✓ |
| Sources / code locations | ✓ | optional | ✓ | ✓ | optional |

Component notes:

- **"Based on" box** (依據): a small box in the header, under the lead. Each item is
  one sentence:
  - the sources used, with a version when there is one (repo and commit, file names, the date
    of the conversation, a spec version);
  - the related parts that are **not** covered, or covered only from a secondary source
    (e.g. "The worker behaviour comes from the API description. The worker code was not read.");
  - how it was checked (e.g. "Not tested on a real device.").

  Keep the detailed file list in the Sources section at the end; this box is the summary.

- **Overview diagram**: inline SVG of the parts, labelled wires, small animated dots on the
  wires. Under it, short lists of what moves in each direction.
  - **Show the order, not only the connections.** When one part connects to two or more
    others, a reader cannot tell which call happens first. Number every arrow in execution
    order with a circled number on the wire (①, ②, ③ …), and repeat the numbers in the label
    ("① GET /verify").
  - Draw arrowheads, so the direction is visible. A request and its response are two arrows
    with two numbers.
  - Animate the dots **in the same order, one after another**, as one loop (dot ② starts when
    dot ① arrives). Never animate all wires at the same time; that suggests they run in
    parallel.
  - Things that really do run in parallel, or only sometimes (a cache miss, a fallback), get
    the same number with a letter (③a, ③b) or a dashed wire, and the lists say when.
  - Under the diagram, give the numbered order as a list: "① The client sends … ② The gateway
    asks …". The list and the numbers on the wires must match.
- **Stepper**: 4–7 stages, Back / Next / Play, progress bar, a short list for the selected
  stage. For a summary, the stages are the phases of the discussion.
  - **Pair the stepper with a diagram** when the stages move between parts (requests,
    hand-offs, approvals). Text alone makes the reader rebuild the picture in their head.
  - Use the same parts as the overview diagram. For each stage, highlight the parts and the
    arrow that act in that stage, and dim the others. The selected stage's arrow shows its
    number and an animated dot.
  - Each stage card names the arrows it uses ("6 · arrows 4 5"), so the stage numbers and
    the arrow numbers can differ without confusion.
  - Implement it by drawing the overview and the stepper diagram from **one JS spec** of parts
    and arrows, so the two never drift apart. Each stage lists its parts and arrows; on a
    stage change, highlight those, dim the rest, and run the dot along the stage's arrows.
  - On a phone the diagram scrolls sideways, so on a stage change scroll its box to centre the
    active parts. Otherwise the highlighted parts can be off screen.
  - For a request/response flow with many calls, a sequence diagram (vertical lifelines,
    arrows top to bottom in time order) is often clearer than the box layout. Choose the one
    that makes the order obvious.
  - For a summary or a timeline without parts, a horizontal timeline that marks the current
    phase is enough.
- **Interactive demo**: the one rule people get wrong (a time window, a threshold, a priority)
  as a slider or toggle that shows the result live. When it models real code, use the same
  formula and name the real function in a comment.
- **Previews**: rebuild the real UI states with the real strings, not screenshots.
- **Pills and callouts**: colour plus a word (Cleared, Kept, Caution, Decided, Open). Never
  colour only.

Use a picture or an interaction only where it explains better than a sentence. A page with
only text is fine for short content.

## 4. Writing style: ASD-STE100

Read `references/ste100-rules.md` before writing. The core rules:

- One idea per sentence. Descriptive sentences have 25 words or fewer.
- Procedure steps are numbered. Each step is one action and 20 words or fewer.
- One word for one thing. Never change a term for variety. List the terms in the Terms table.
- Preconditions ("Before you start") and cautions are in their own boxes, not in the steps.
- Active voice. Every sentence names its actor.
- UI strings, code identifiers, file paths and quotes stay verbatim.

Apply the same rules to text that the JS writes (stepper statements, demo results).

## 5. Language

Write in the language the user asks for. If they do not say, ask. A page that goes into a
repository defaults to English. For a Chinese page, follow section 8 of
`references/ste100-rules.md` (character limits, no 被/進行, one term per thing, spacing between
Chinese and English), and use the CJK font stack and line-height from the design rules.

## 6. Visual design

- **One self-contained file.** Inline CSS and JS. No CDN, no external fonts. It works offline.
- **Start from `assets/page-shell.html`** and keep its page bar. Readers open these pages on
  phones, in dark rooms and on projectors; some need larger text, and long pages need a way to
  jump between sections.

### Page bar

- The bar is sticky on every screen. It slides up while the reader scrolls down and comes back
  when they scroll up, near the top of the page, or when it has keyboard focus.
- **Contents** opens a menu of the sections. The shell builds it from each `main section[id]`
  and its `<h2>`, so give every section an `id` and an `<h2>`, and do not write a contents
  list in the page. The menu marks the section that the reader is in, and closes on a link,
  on Escape and on a click outside.
- The bar shows the page title from the `<h1>`. On a phone the theme and text-size buttons move
  into the menu, so the bar keeps room for the title.
- The page's own header is `main > header`; style it with that selector, because the bar is a
  `<header>` too.

### Light and dark

- Colours are tokens on `:root`. Dark values are defined twice: under
  `@media (prefers-color-scheme: dark)` with `:root:not([data-theme="light"])` (Auto), and
  under `:root[data-theme="dark"]` (an explicit choice). `body` has an explicit background.
- Every colour on the page comes from a token, **including inside SVG** (`fill: var(--text)`,
  `stroke: var(--line)`) and in colours that JS sets. A hard-coded hex colour breaks one theme.
- Check both themes before hand-over: text on every tinted box, pills, callouts and the
  diagram lines must stay readable.

### Text size

- Every font size, line height, padding and gap that should grow with the text is in `rem`
  (16 px = 1 rem). The shell's A− / A / A+ buttons change one root value (`--scale`, 87.5 % to
  150 %), so anything in px does not scale. Borders and hairlines stay in px.
- SVG diagrams scale with their width, not with the text size. Keep SVG label sizes in SVG
  units, and make sure the text next to the diagram still lines up at 150 %.
- Layouts must survive 150 %: let rows wrap (`flex-wrap`), use `minmax()` grids, and never fix a
  height on a box that holds text.

### Phone width

- The page works at 360 px wide with no horizontal page scroll. Side gutter 16 px.
- The last item of `<main>` is a **Back to top** link (`href="#top"`, the page bar has
  `id="top"`), on desktop and on phone. It scrolls smoothly unless the reader asks for reduced
  motion, and it is hidden in print.
- Multi-column grids become one column below 40 rem (`repeat(auto-fit, minmax(16rem, 1fr))`
  does this by itself). A stepper of 5–7 cards becomes 2 columns, then 1.
- Wide tables and SVGs sit in a `.scroll` wrapper (`overflow-x: auto`), followed by a
  `.scroll-hint` line ("Swipe sideways to see all of it."), which the shell shows only on
  phones. Give a wide SVG a `min-width` so its labels stay readable instead of shrinking.
- Every control is at least 44 × 44 px to touch. Nothing important is only on hover.
- Sliders and buttons in demos work with touch (`input` events, not `mousemove`).

### Other rules

- Spacing scale 4/8/12/16/24/32/48 px (0.25–3 rem). Type sizes 12/14/16/18/24/32 px
  (0.75–2 rem), line-heights on the 4 px grid. Times and counts use
  `font-variant-numeric: tabular-nums`.
- Use the project's own palette when it has one. The shell uses a light violet theme.
- Before hand-over, search the page for `[` to make sure no placeholder from the reference is
  left.
- Set `<html lang>` to the page language. Chinese pages add a CJK font stack and roomier
  line-height.
- Animate only `transform`, `opacity` and `offset-distance`. Add a `prefers-reduced-motion`
  block that stops the motion.
- Every SVG has `role="img"` and an `aria-label` that says the content in words. Live results
  have `aria-live="polite"`.

## 7. Where it goes, and hand-over

- In a repository, save it beside the related docs (e.g. `docs/<topic>.html`). Otherwise use a
  durable folder the user names, or ask. Tell the user the full path.
- Do not commit unless the user asks. Offer to commit, or to publish it as a shareable page.
- In the reply: name the reader in one line, list the sections, report surprises you found,
  and say whether you opened it in a browser.

## 8. Self-check before hand-over

- Every fact matches its source. Every file path exists. Every decision is attributed right.
- The "Based on" box names every source used and every related part that is not covered.
- In every flow diagram, a reader can tell the order of the calls without reading the text.
  The numbers on the arrows match the numbered list, and each stepper stage names its arrows.
- The contents menu lists every section, and each link lands on its heading below the bar.
- The page has the theme switch and the text-size buttons, and both work. No hard-coded colour
  is left (search the file for `#` colours outside the token blocks).
- The page reads well in Light, in Dark, at 150 % text, and at 360 px wide. If a browser is
  available, take screenshots of these cases. Headless Chrome does not go below a 504 px
  viewport, so `--window-size=360,…` only crops the picture. To test 360 px, load the page in
  a `<iframe style="width:360px">` on a small harness page and screenshot that. To test Dark or
  150 %, add a script to a copy of the page that clicks the shell's buttons. To screenshot a
  part further down, add `html{scroll-behavior:auto!important}` to the copy before you scroll,
  or the smooth scroll is still moving when the screenshot is taken.
- An interactive demo gives the same answer as the real rule at the edges (on the boundary,
  just before, just after, several periods later).
- Run the STE self-check in `references/ste100-rules.md`.
- Every term in the text is in the Terms table, with no synonyms.
