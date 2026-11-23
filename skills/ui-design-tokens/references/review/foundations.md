# Foundations

The global setup every other fix depends on. Search the whole project, not just the review scope.

## 1. Locate

| Foundation | Where to look |
|---|---|
| Token source | `tailwind.config.*`, `@theme` blocks (Tailwind v4), `:root { --… }` custom properties, `tokens.*`, `theme.*`, `_variables.scss`, RN `theme.ts`, Flutter `ThemeData` / `ThemeExtension` |
| Global stylesheet | `globals.css`, `index.css`, `app.css`, `main.css`, `styles.css`, `_base.scss` |
| Font loading | `next/font`, `@font-face`, Google Fonts `<link>`, `expo-font`, `pubspec.yaml` fonts |
| Document root | Root layout, `index.html`, `app.html`, `_document.*` (for `lang`) |
| shadcn/ui | `components.json` — if present, read it first and follow `shadcn.md` |

Note the Tailwind major version (`tailwindcss` in `package.json`): v3 keeps the scale in `tailwind.config.*`; v4 keeps it in `@theme` / `@theme inline` inside the global CSS and may have no config file at all — that is not "missing".

## 2. If missing

A missing token source or global stylesheet is itself a 🔴 **systemic issue** — report it first, because without it every fix in the review is a one-off that the next component will undo. For each missing piece, the finding's **Fix** is the complete file to create. Build it from the `ui-design-tokens` tables, starting from the ready-made examples in its platform reference rather than writing one from scratch:

| Stack | What to create | Start from (`ui-design-tokens`) |
|---|---|---|
| Tailwind v3 | `tailwind.config.*` that **overrides** (not extends) `spacing`, `fontSize` as `[size, lineHeight]` tuples, `fontWeight`, `borderRadius`, `boxShadow`, `transitionDuration`, plus semantic colors | `references/tailwind.md` — *Making violations impossible*, *Typography* |
| Tailwind v4 | `@theme` block in the global CSS with the same scales. v4 cannot block `p-5` through config, so also propose the audit grep as a CI step or a lint rule | `references/tailwind.md` — *Tailwind v4*, *Quick audit command* |
| Plain CSS / SCSS | `tokens.css` of `:root` custom properties (`--space-*`, `--text-*`, `--leading-*`, `--weight-*`, `--radius-*`, `--shadow-*`, `--duration-*`, `--ease-*`, semantic `--color-*`) plus `base.css` with the font stack, `font-synthesis: none`, a `:focus-visible` ring, and the `prefers-reduced-motion` rule | `references/other-platforms.md` — *Plain CSS* |
| React Native | `theme.ts` exporting `space`, `type`, `radius`, `duration`, `easing`, semantic `colors` | `references/other-platforms.md` — *React Native* |
| Flutter | `ThemeData` / `ThemeExtension` with the same scales | `references/other-platforms.md` — *Flutter* |

Also give the path to create it at and the one-line change that wires it in (import in the root layout, `content` paths, etc.). Keep the project's existing colors — map them to semantic names rather than inventing a palette. If there are no colors to map, leave clearly marked placeholders and list them under "Needs manual check".

## 3. If present, check

Read them in full:

- Does the spacing, font-size, radius, shadow, and duration scale match the skill? Is `spacing` overridden or only extended (off-grid steps still available)?
- Are font-size tokens `[size, lineHeight]` pairs with line-heights on the 4px grid?
- Is every declared font family actually loaded, with only the weights used, and does each stack end in a generic family with CJK faces after the Latin face?
- Is `lang` set on `<html>`? Is `font-synthesis: none` set?
- Is there a global `prefers-reduced-motion` rule?
- Are there global `outline: none` / `*:focus { outline: 0 }` resets?
- Are colors semantic tokens? Compute text/background contrast for each token pair used together.
- Is a token the skill requires missing from an otherwise good system (e.g. no duration tokens)? Report the tokens to add.

A problem here is a **systemic issue**: one fix removes it everywhere. Report it once, at the foundation file, and note roughly how many usages it affects.

## 4. Build the value → token map

List every token the foundations define with its resolved value, for Step 4C to look up:

```
16px     → p-4 / --space-lg
#ef4444  → text-destructive / --color-danger
150ms    → duration-150 / --duration-fast
```

If foundations are missing, build the map from the files proposed in section 2 instead, so every fix in the report already uses the new token names.
