---
name: ui-design-tokens
description: Enforce an 8pt grid spacing system, a modular type scale, font family and font weight rules (including CJK), button specs, motion timing, restraint in explanatory text (subtitles, section and dialog descriptions), and consistent radius, shadow, icon, sizing and contrast tokens in any UI that gets generated or edited. Use this skill whenever building, restyling, reviewing, auditing, or debugging a user interface — React, Next.js, Tailwind, shadcn/ui, plain CSS, HTML, React Native, or Flutter — including landing pages, dashboards, forms, buttons, tables, cards, modals, navigation, animations, and component libraries. Trigger it even when the user says nothing about spacing, grids, typography, fonts, animation, or design systems, because arbitrary padding, font sizes, unloaded or faux-bold fonts, and transition timings are the most common defects in generated UI and the user will not always think to ask. Also use it to review or audit existing UI code against these tokens.
---

# UI Design Tokens

Generated UI usually fails not because it is ugly, but because it is *inconsistent*: `p-5` here, `p-[13px]` there, `text-[15px]` somewhere else, a 500ms bounce on one button and none on the next. Each value is defensible alone; together they read as noise. This skill fixes that by restricting every spatial, typographic, and motion decision to a small, closed set of values.

The governing idea: **pick from a scale, never invent a number.** If a value is not in a table below, it is not available — adjust the layout instead of inventing a value.

## 1. Spacing — the 8pt grid

Every `padding`, `margin`, `gap`, and fixed dimension comes from this scale. 8 is the base because all common display scale factors (1x, 2x, 3x) divide it cleanly, so nothing lands on a half pixel.

| Token | Value | Use for |
|---|---|---|
| `xs` | 4px | Icon-to-label gaps in small controls, dense table cells, badge padding |
| `sm` | 8px | Icon-to-label gaps in buttons, chip padding, tight stacks |
| `md` | 12px | Input horizontal padding, small-button horizontal padding |
| `lg` | 16px | Card padding, default button horizontal padding, sibling components |
| `xl` | 24px | Spacing between content blocks, large-button horizontal padding |
| `2xl` | 32px | Card-to-card, padding inside a section |
| `3xl` | 48px | Between sections |
| `4xl` | 64px | Major page divisions |
| `5xl` | 96px | Hero and landing-page breathing room |

Rules that make the scale hold:

- **4px is a half step, not a free value.** Use it only *inside* a component, between elements that already read as one unit. Anything separating two distinct components is 8 or larger.
- **Never use 20, 28, 36, or 40px for spacing.** They sit between steps and quietly break the rhythm. (36 and 40 are fine as control *heights* — see §4 — just not as gaps or padding.) If 16 feels tight and 24 feels loose, the problem is usually the element's own size, not the gap.
- **Nested padding decreases inward.** A section at 48px contains cards at 24px containing rows at 12px. Increasing inward makes the hierarchy read backwards.
- **Proximity encodes grouping.** Related items get a smaller gap than the gap separating their group from the next one — ideally at least one full scale step of difference, so the grouping is unambiguous.
- **Vertical rhythm needs asymmetry.** A heading sits closer to the text it introduces than to the block above it (e.g. 32px above, 12px below). Equal spacing above and below makes headings float, orphaned.

## 2. Typography — a different system

### Type scale

This is the rule people most often get wrong: **font sizes do not belong to the 8pt grid.** They follow a modular scale (roughly 1.125–1.25 between steps) tuned for legibility. 14px and 18px are not multiples of 4, and they are among the most useful sizes there are.

What *does* align to the 4px grid is **line-height**, because line-height determines the actual box a text block occupies. Align that and the vertical rhythm holds regardless of font size.

| Role | Size | Line-height | Weight |
|---|---|---|---|
| `caption` | 12px | 16px | 400–500 |
| `body-sm` | 14px | 20px | 400 |
| `body` | 16px | 24px | 400 |
| `body-lg` | 18px | 28px | 400 |
| `h4` | 20px | 28px | 600 |
| `h3` | 24px | 32px | 600 |
| `h2` | 32px | 40px | 600–700 |
| `h1` | 40px | 48px | 700 |
| `display` | 56px | 64px | 700 |

Supporting rules:

- **Line-height tightens as size grows.** Body text wants ~1.5; large headings want 1.1–1.25. A 40px heading at 1.5 looks like unrelated lines stacked.
- **Round line-height to the nearest multiple of 4.** 18px × 1.5 = 27 → use 28.
- **Never go below 12px** for anything a user is meant to read. Below that, legibility fails before aesthetics do.
- **Cap line length at 45–75 characters** for paragraphs. Long measures are a bigger readability problem than font size.
- **Use weight and color for hierarchy before reaching for size.** Three sizes with two weights beats six sizes.
- **Add negative letter-spacing to large Latin text** (−0.01em to −0.02em at 32px and above). Default tracking is optimized for body copy and looks loose at display sizes. Never apply negative tracking to CJK text — ideographs are designed to sit on a full em square and collide when squeezed.

### Font families

- **At most two families, plus a monospace.** One sans-serif for UI and body; optionally one display or serif face for headings or marketing; a monospace for code, IDs, and hashes. A third expressive family is almost always a mistake.
- **Every declared family must actually be available.** Generated code routinely writes `font-family: "Inter"` without loading Inter, so every user sees a different fallback. Each family must be either loaded (`@font-face`, `next/font`, Google Fonts, bundled asset) or a system font.
- **Always give a full fallback stack ending in a generic family** (`sans-serif`, `serif`, `monospace`). Never a single family name.
- **Order the stack: Latin face → CJK faces → generic.** CJK fonts contain Latin glyphs too, usually weaker ones; if a CJK face comes first, it will render the English text as well.

Reference stacks:

| Purpose | Stack |
|---|---|
| System UI (Latin) | `system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif` |
| + Traditional Chinese | `… "PingFang TC", "Noto Sans TC", "Microsoft JhengHei", sans-serif` |
| + Simplified Chinese | `… "PingFang SC", "Noto Sans SC", "Microsoft YaHei", sans-serif` |
| + Japanese | `… "Hiragino Sans", "Hiragino Kaku Gothic ProN", "Noto Sans JP", "Yu Gothic", Meiryo, sans-serif` |
| + Korean | `… "Apple SD Gothic Neo", "Noto Sans KR", "Malgun Gothic", sans-serif` |
| Monospace | `ui-monospace, SFMono-Regular, Menlo, Consolas, "Liberation Mono", monospace` |

- **Set the `lang` attribute** (`<html lang="zh-Hant">`, `lang="ja"`) and on any mixed-language block. Chinese, Japanese, and Korean share Unicode code points for many characters whose correct glyph shapes differ; the browser uses `lang` to pick the right variant and the right font.
- **Use tabular figures for numbers that line up or change** — table columns, prices, counters, timers, dashboards: `font-variant-numeric: tabular-nums`. Proportional digits make columns ragged and live numbers jitter.
- **Web-font loading:** WOFF2 only, `font-display: swap` (or a framework loader that handles it, like `next/font`), preload only the weights actually used, subset where possible. If you need three or more weights of one family, use its variable font instead.
- **Prefer system fonts for CJK.** A single CJK weight is several megabytes. If a CJK web font is required, use a service or loader that ships unicode-range slices rather than the full file.

### Font weights

| Weight | Name | Use |
|---|---|---|
| 400 | Regular | Body text, inputs, table cells |
| 500 | Medium | Buttons, labels, tabs, nav items, small emphasis |
| 600 | Semibold | Headings `h4`–`h2` |
| 700 | Bold | `h1`, display, strong emphasis |

- **Use at most three weights per screen.** Hierarchy comes from contrast between weights; five weights blur into each other.
- **Never go below 400 for text under 24px.** Light and thin strokes lose contrast in practice even when the color ratio passes. Reserve 300 for display sizes (32px+), if at all.
- **Load every weight you use, and only those.** When a weight is missing, the browser either substitutes the nearest one or synthesizes a fake bold by smearing strokes. After loading the right weights, set `font-synthesis: none` so a missing weight shows up as a visible bug rather than a smeared fake.
- **"Bold" for accessibility means 700.** WCAG's large-text threshold (§7) counts 700+, not 600.

**The Windows CJK trap.** Microsoft YaHei (Simplified) and Microsoft JhengHei (Traditional) ship only Light, Regular, and Bold — no Medium or Semibold. When they are the resolved font, a 500 request falls back to 400 and a 600 request rounds up to 700, so medium-weight buttons and semibold headings silently lose or change their hierarchy for every Windows user reading Chinese. Handle it one of two ways:

1. Load Noto Sans TC/SC as a web font with the 500 and 600 weights, so the system font is never reached; or
2. Design a hierarchy that survives the collapse — use 400 and 700 only for CJK-heavy UI, and lean on size and color for the rest.

### CJK text

- **Line-height around 1.7 for CJK body text**, versus ~1.5 for Latin. Ideographs fill the whole em box and are information-dense, so they need more leading. On the 4px grid: 14px → 24px, 16px → 28px, 18px → 32px. Headings go to ~1.4.
- **No italics.** CJK scripts have no italic form; the browser just slants the glyphs, which looks broken. Emphasize with weight or color.
- **Avoid 12px for running CJK text.** Complex characters lose strokes at that size; keep 12px for short labels only and use 14px as the floor for sentences.
- **No negative tracking** (see the type-scale rules above). Leave `letter-spacing` at 0; a very slight positive value (0.02–0.05em) can help dense headings.

## 3. Radius, shadow, border

| Token | Radius | Use for |
|---|---|---|
| `sm` | 4px | Badges, tags, small inputs |
| `md` | 8px | Buttons, inputs, default |
| `lg` | 12px | Cards, panels |
| `xl` | 16px | Modals, large surfaces |
| `full` | 9999px | Pills, avatars, pill-style buttons |

- **Nested radii need the inner one smaller**, by roughly the padding between them: an outer 16px radius with 8px padding wants an inner radius near 8px. Equal radii make the inner element look like it is bulging out.
- **Pick one elevation system and stay in it.** Two or three shadow levels is plenty: a subtle resting shadow, a raised/hover shadow, and an overlay shadow for modals and popovers. Shadows should be large and soft with low opacity, not tight and dark.
- **Do not combine a visible border and a prominent shadow** on the same element — pick one to carry the edge. A hairline shadow under a bordered input is fine; a large drop shadow on a bordered card is not. Borders suit dense, data-heavy UI; shadows suit spacious, layered UI.
- **Borders stay at 1px** in nearly all cases; 2px is for focus rings and selected states.

## 4. Icon and control sizes

Icons follow their own scale, independent of spacing: **16 / 20 / 24 / 32 / 40px**. On the web, match icon size to adjacent text — 16px icons beside 14px text, 20px beside 16px. Native platforms run one step larger: Material 3 uses 20dp for button icons and 24dp as the standard standalone icon.

Control heights (buttons, inputs, selects) stay on the 8pt grid, with 36px as the accepted exception for dense UI:

| Size | Height | Use |
|---|---|---|
| `sm` | 32px | Dense toolbars, table row actions |
| `md` | 40px (36px in dense UI) | Default buttons and inputs |
| `lg` | 48px | Primary CTAs on mobile, touch-first forms |

**Buttons and inputs that sit in the same row must share a height.** A 36px button next to a 40px input is the most visible inconsistency a form can have.

**Touch targets** have three different standards; use the one that matches the platform:

| Platform | Minimum target |
|---|---|
| Web — WCAG 2.2 AA (2.5.8) | 24×24px |
| Web — recommended / WCAG AAA (2.5.5) | 44×44px |
| iOS | 44×44pt |
| Android / Material | 48×48dp |

The visible control can be smaller than its target — extend the hit area with padding, a pseudo-element, `hitSlop`, or spacing between targets rather than inflating the design.

## 5. Buttons

### Sizing

Set button height explicitly and center the content with flexbox. Do **not** derive height from vertical padding — padding-based height changes with font metrics and icon size, so a text button and an icon button end up different heights.

| Size | Height | Horizontal padding | Label | Icon (web / native) | Icon–label gap |
|---|---|---|---|---|---|
| `sm` | 32px | 12px | 14px, 500 | 16px / 20dp | 4px |
| `md` | 40px (36px dense) | 16px | 14px, 500 | 16px / 20dp | 8px |
| `lg` | 48px | 24px | 16px, 500 | 20px / 24dp | 8px |

Notes on where these come from, so they are applied with the right confidence:

- `sm` and `md` match Material 3 (Expressive XS = 32dp with 12dp padding and a 4dp icon gap; default = 40dp with 16dp padding — M3 explicitly deprecated its old 24dp padding) and shadcn/ui (`h-9 px-4`, `h-8 px-3`).
- `lg` at 48px is a **convention for mobile CTAs, not a platform standard** — Material 3 jumps from 40dp straight to 56dp. Use 56px instead if the project follows M3 Expressive closely.

Further rules:

- **Icon-only buttons are square** (width = height) and must have an accessible name (`aria-label`, `accessibilityLabel`, `tooltip`).
- **A leading icon may take 4px off its side's padding** (e.g. 12px left / 16px right at `md`) to balance the icon's visual weight. Optional, but apply it consistently if used.
- **Give text buttons a minimum width** (about 64–80px) so short labels like "OK" don't produce stubby buttons.
- **Labels are single-line and verb-first** ("Save changes", not "Changes"). Never let a button label wrap — shorten it or widen the button.
- **One icon per button at most.** Two icons plus a label is a menu item, not a button.

### Hierarchy

| Variant | Emphasis | Use |
|---|---|---|
| Primary (filled) | Highest | The single main action of a region |
| Secondary / tonal | Medium | Important but not the main action |
| Outline | Medium-low | Alternatives beside a primary |
| Ghost / text | Low | Tertiary actions, toolbars, dense UI |
| Destructive | Special | Delete, remove, irreversible actions |
| Link | Lowest | Navigation that happens to look inline |

- **At most one primary button per region** (a card, a dialog, a form section). Two primaries means neither is primary.
- **Destructive replaces primary, it does not sit beside it.** In a delete-confirmation dialog, the destructive button *is* the primary action; the other is outline or ghost.
- **Order is consistent within a product:** pick either "Cancel · Confirm" (primary on the trailing edge — Material, macOS, most web) or the reverse, and never mix.

### States

Every button needs all six states. Missing states are the most common gap in generated buttons.

| State | Treatment |
|---|---|
| Default | Resting color and elevation |
| Hover | Shift background one tone, or overlay the content color at ~8% (the M3 state-layer approach). Desktop only — hover does not exist on touch. |
| Pressed / active | Overlay ~10–12%, or one tone further than hover. Optionally `scale(0.97–0.98)` — a common convention, not a spec; never go below 0.95. |
| Focus-visible | 2px ring with a 2px offset, ≥3:1 contrast against both the button and the background. Use `:focus-visible`, not `:focus`, so mouse clicks don't show a ring. |
| Disabled | Muted colors or ~50% opacity, no hover/press response, no shadow, `cursor: not-allowed`. Exempt from contrast minimums. If the user needs to know *why* it is disabled, use `aria-disabled` so a tooltip can still receive focus. |
| Loading | Keep the button's width fixed (no layout shift). Put a spinner in the icon slot or replace the label with a spinner of the same footprint. Set `aria-busy="true"` and block repeat clicks. |

### Shadows on buttons

- **Filled buttons are flat or have a subtle resting shadow.** Outline, ghost, text, and link buttons have no shadow.
- **Hover raises one elevation level** (desktop only).
- **Pressed returns to the resting level — it does not drop to zero.** This matches Material 3's elevated button behavior and reads as "pushed back into place" rather than "vanished".
- **Disabled has no shadow.**

## 6. Motion

Motion exists to explain change — where something came from, that an action registered, what state something is in. If an animation does none of those, remove it.

### Duration

| Token | Duration | Use |
|---|---|---|
| `instant` | 100ms | Press feedback, checkbox and switch toggles |
| `fast` | 150ms | Hover color changes, focus rings, tooltips |
| `normal` | 200ms | Elevation and shadow changes, small movements, exits of menus and popovers |
| `moderate` | 250–300ms | Entrances of dropdowns, popovers, dialogs; expand/collapse |
| `slow` | 400ms | Large movement across big screens only |

- **Stay within 100–400ms for UI responses.** Nielsen Norman Group's research puts 100ms at "feels instant" and treats 400ms as already very slow; at 500ms animations start to feel like a drag.
- **Scale duration with distance and size.** A 12px nudge and a full-screen sheet should not share a timing.
- **Exits are shorter than entrances** by about 50–100ms. A popover appears in 250–300ms and disappears in ~200ms — the user has already moved on.

### Easing

Linear motion looks mechanical. Match the curve to what the element is doing:

| Movement | Easing | CSS value |
|---|---|---|
| Entering (appear, expand, slide in) | Decelerate / ease-out | `cubic-bezier(0, 0, 0.2, 1)` |
| Exiting (disappear, collapse, slide out) | Accelerate / ease-in | `cubic-bezier(0.4, 0, 1, 1)` |
| Moving within the screen | Standard / ease-in-out | `cubic-bezier(0.4, 0, 0.2, 1)` |
| State changes (color, hover, press) | Ease-out | `cubic-bezier(0, 0, 0.2, 1)` |
| Indeterminate spinners | Linear | `linear` |

**Never use the same curve for entering and exiting.** Entering elements arrive fast and settle; exiting elements start gently and leave quickly. Reserve bounce and elastic curves for deliberately playful moments — never routine UI.

### What to animate

- **Movement uses only `transform` and `opacity`.** These are compositor-only and stay smooth. Never animate `width`, `height`, `top`, `left`, `margin`, or `padding` — they force layout on every frame.
- **Color, background-color, border-color, and box-shadow are fine for short state transitions** (≤200ms) like hover and press. Avoid animating large blurry shadows continuously.
- **Never write `transition: all`.** List the properties explicitly. `all` animates things you did not intend (including layout properties on resize) and makes performance unpredictable.
- **Don't hide essential content behind hover-only reveals** — they don't exist on touch.

### Reduced motion

Respect the user's OS setting. Under `prefers-reduced-motion: reduce`, remove movement and scaling (slides, parallax, zooms, press-scale) but keep short opacity and color fades, which do not trigger vestibular discomfort:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

Use the blanket rule above as a safety net, then selectively re-enable opacity fades where they aid comprehension.

## 7. Color and contrast

Contrast is a hard requirement, not a style preference. Check it rather than guessing — a color that looks fine on your screen routinely fails.

| Content | Minimum ratio (WCAG AA) |
|---|---|
| Body text | 4.5:1 |
| Large text: ≥24px regular, or ≥18.66px bold (700+) | 3:1 |
| Icons, borders, control boundaries | 3:1 |
| Focus indicators | 3:1 against both the control and its background |

WCAG defines large text in *points* (18pt / 14pt bold), and 1pt = 1.333px — which is why the thresholds are 24px and ~18.66px, not 18px and 14px. Against the type scale in §2: `h3` (24px) and up qualify as large; `h4` (20px at weight 600) does **not**, because 600 is not bold — it needs 4.5:1.

- **Never encode meaning in color alone.** Pair status color with an icon, a label, or a shape.
- **Never remove focus outlines.** Restyle them if the default is ugly, but every interactive element needs a visible keyboard focus state.
- **Placeholder text is not a label.** It disappears on input and usually fails contrast.
- **Disabled elements are exempt from contrast minimums** but should still be distinguishable.
- **Build semantic tokens, not raw colors.** `--color-danger`, `--color-text-muted`, `--color-surface` — so theming and dark mode are a token swap rather than a find-and-replace.

## 8. Explanatory text — let the layout explain

Generated UI reflexively puts a muted sentence under every page title, section heading, and dialog title. Each one looks helpful alone; together they double the text on screen, and the user learns to skip the grey lines, including the one that mattered. Explanatory copy is a cost, not a default. **The structure should explain the screen; text explains only what the structure cannot.**

The test for every subtitle, description, or helper line: **if it were deleted, would the user make a mistake?**

| Answer | What the text is | Do this |
|---|---|---|
| No | Restates the title ("Devices — every device across your sites") or describes what is plainly visible | **Delete it.** If it carried live data (a date, a scope, a count), move that into a compact meta line or chip beside the title, not a sentence. |
| Yes, but structure can prevent it | Ordering or dependency instructions ("Pick the site first, then…"), units, formats, what a toggle does | **Redesign instead of explaining:** put the dependent field after the one it depends on, use a cascading select, split into steps, add a unit suffix (`[ 5 ] min`), write a more precise label, show the current state's effect next to a switch, use inline validation and empty states. |
| Yes, and only words can prevent it | Consequences of destructive or irreversible actions ("Attendance records are kept"), counter-intuitive rules ("Admin edits aren't limited") | **Keep it, at one sentence.** Consequences stay visible in confirmation dialogs. Background rules on forms go behind an info icon (tooltip/popover) beside the heading. |

Further rules:

- **Page headers have no description by default.** A page title plus its content is self-explanatory; add a line only when it passes the test above.
- **One sentence, never a paragraph.** If it needs more, it is documentation — link to it or move it to a help tab.
- **Write for the user, not the developer.** Implementation details ("assigned in the same transaction", "the backend only trusts…") never appear in UI copy.
- **Don't repeat the same explanation at two levels** (page description, then section description, then field help saying the same thing).
- **Prefer state over instruction.** "Off: holiday pay is recorded, no leave banked" next to a switch beats a paragraph explaining what the switch does.

### Notices — states that need the user's attention

Muted grey text says "you can skip this". That is right for a plain empty list ("No active alerts") and wrong for a state the user should notice and usually fix: "No holidays left on this calendar", "Not in a group — can't check in anywhere", "No devices yet". Those get a **notice**, not a grey sentence:

- **One shared component**, not hand-rolled per screen: a tinted container + an icon in a circle + one short sentence + the fix as an action.
- **Stand out by colour, not size.** Use the theme's container pairs: a primary/brand tint for "missing, and fixable" (a nudge), a warning tint for "behaves differently than expected until fixed". No dark borders, no bigger font.
- **The fix replaces the instruction.** "Add them under Holidays" becomes a "Go to Holidays" button inside the notice. Omit the action only when the fix is already right beside it (the field above, a Change button on the same card) or there is none the user can take.
- **Not for errors or plain empties.** Failures stay an error alert; empty lists that need nothing done stay quiet muted text — making them loud turns every empty screen into noise.

## 9. Self-check before returning UI code

Run through this before handing back any interface. Most violations show up in the first three lines.

1. Does every spacing value come from the scale in §1? Search the output for `5`, `7`, `9`, `10`, `14`, `18`, `20`, `28`, `36` used as spacing.
2. Are there any arbitrary or magic values — `p-[13px]`, `margin: 22px`, `text-[15px]`, `duration-[350ms]`? Each one needs a stated reason.
3. Is every line-height a multiple of 4?
4. Are font sizes from §2, and is the number of distinct sizes small (ideally ≤5 per screen)?
5. Are there at most two font families plus a monospace, is each one loaded or a system font, and does each stack end in a generic family (with CJK faces after the Latin face where CJK content appears)?
6. Are at most three weights used, is every weight actually loaded, and is nothing under 24px lighter than 400?
7. For CJK content: is `lang` set, is line-height ~1.7, and are italics and negative tracking absent? If the stack can fall back to Microsoft YaHei/JhengHei, does the hierarchy survive 500→400 and 600→700?
8. Do numeric columns, prices, and live values use `tabular-nums`?
9. Do nested containers decrease padding inward, and nested radii decrease inward?
10. Do buttons use explicit heights from §5, and do buttons and inputs in the same row match?
11. Is there at most one primary button per region?
12. Does every button have hover, pressed, focus-visible, disabled, and (where async) loading states?
13. Are all durations between 100 and 400ms, with entrances using ease-out and exits using ease-in?
14. Is `transition: all` absent, and is there a `prefers-reduced-motion` path?
15. Do interactive elements meet the platform's touch-target minimum from §4?
16. Does text contrast meet 4.5:1 (or 3:1 only for text that is genuinely ≥24px, or ≥18.66px bold)?
17. Does every page subtitle, section description, and dialog description pass the §8 test — would deleting it cause a mistake? If not, delete it; if structure could prevent the mistake, redesign instead.
18. Is every state the user must notice and fix ("none left", "not set up yet", "no access until…") a shared notice with colour, icon and the fix as an action — not muted grey text? And are plain empty lists still quiet?

When a rule genuinely needs to be broken — optical alignment, a 1px border eating into padding, a font with unusual metrics — do it deliberately and say why in a comment. Optical correctness beats mathematical correctness; unexplained values do not.

## Platform specifics

Read the file that matches the target stack. Do not read both.

- **`references/tailwind.md`** — Tailwind and shadcn/ui: which utility steps map to the scale, which to avoid, a normalized shadcn `buttonVariants`, motion utilities, and the config that makes violations impossible to write.
- **`references/other-platforms.md`** — Plain CSS custom properties, React Native, and Flutter, including motion tokens and reduced-motion handling on each.

## Reviewing existing UI

When the user explicitly asks to **review or audit** existing UI code against these rules ("review this UI", "按照 ui-design-tokens 審查", `/ui-design-tokens review`), read `references/review/workflow.md` and follow it step by step. It is a read-only review: it asks for the scope, checks the global setup first, and reports every violation with a token-based fix, without editing files.

Do not start the review workflow on your own while building or editing UI — there, run the §9 self-check on your own output instead.

The review workflow supports only Tailwind (including shadcn/ui), plain CSS / SCSS / Less / CSS-in-JS, React Native, and Flutter. If the code to review is in any other stack (for example Blazor, XAML, WinForms, SwiftUI, Jetpack Compose), **do not use the review workflow.** Say in one line that the full review does not support this stack, then review the code directly against the rules in §1–§9: read the files, and for each problem give the location, the rule, and a fix that uses the stack's own way of defining tokens. The rules above apply to every stack.
