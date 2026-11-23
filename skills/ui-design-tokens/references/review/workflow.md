# UI Design Review — workflow

Review existing UI code against the rules in `SKILL.md` and report every violation together with a concrete fix. This is a **read-only review** — do not edit, create, or delete any file, even when a fix is obvious.

The rules in `SKILL.md` are the only source of truth for this review; do not substitute personal taste or other design systems. All `§` references below point to sections of `SKILL.md`.

Files in this folder are read **only when their condition applies** — never all of them up front.

| File | Read when |
|---|---|
| `foundations.md` | Always (Step 3) |
| `shadcn.md` | `components.json` exists |
| `patterns-tailwind.md` | Stack uses Tailwind |
| `patterns-css.md` | Stack is plain CSS / SCSS / Less / CSS-in-JS, **or** a Tailwind project that also has component-level stylesheets or CSS modules |
| `patterns-react-native.md` | Stack is React Native |
| `patterns-flutter.md` | Stack is Flutter |
| `report-templates.md` | Step 5 |

## Supported stacks

The design rules apply to any UI, but **this review workflow supports only these stacks**, because each one has grep patterns and a token template behind it:

| Stack | Markup files | Style files |
|---|---|---|
| Tailwind (including shadcn/ui) | `.tsx`, `.jsx`, `.vue`, `.svelte`, `.html` | `tailwind.config.*`, global CSS with `@theme` |
| Plain CSS / SCSS / Less / CSS Modules / CSS-in-JS | `.tsx`, `.jsx`, `.vue`, `.svelte`, `.html` | `.css`, `.scss`, `.less`, `*.module.css`, `*.styles.ts`, `styled.*`, files using `styled-components` / `@emotion` |
| React Native | `.tsx`, `.jsx` | React Native style files, `theme.ts` |
| Flutter | `.dart` widget files | `ThemeData` / `ThemeExtension` files |

Everything else is **not supported** — for example Blazor (`.razor`, `.cshtml`), XAML (WPF, WinUI, MAUI), WinForms, SwiftUI, Android XML / Jetpack Compose. Do not improvise grep patterns or token templates for them.

- **Whole project unsupported** → leave this workflow before Step 2. Say in one line that the full review does not support this stack (name the stack you detected), then review the code directly against the rules in `SKILL.md` §1–§9, as described in its *Reviewing existing UI* section. To add full support later, the stack needs a `patterns-<stack>.md` file in this folder, a token template in `references/other-platforms.md`, and its file extensions in the table above.
- **Mixed project** (supported and unsupported parts) → run this workflow on the supported files only. Review the unsupported files directly against the rules in `SKILL.md` §1–§9 and put those findings under "Unsupported stack — rule-based review" in the report.

## Step 1 — Detect the stack

Check the dependencies (`package.json`, `pubspec.yaml`, `*.csproj`, and similar) and the UI file extensions in the repo. Compare them with *Supported stacks* above and apply its rules before you continue.

## Step 2 — Ask for the scope

Never assume a scope. First do a quick, read-only discovery so the question can show real numbers:

- The UI files in the repo (the supported extensions above) and their count
- UI files changed on this branch: `git diff --name-only`, `git diff --name-only --cached`, and `git diff <main-branch>...HEAD --name-only`
- Frontends in the repo — more than one `package.json` / `pubspec.yaml` with a UI framework dependency means a monorepo

Always exclude `node_modules`, `dist`, `build`, `out`, `.next`, `.nuxt`, `coverage`, `bin`, `obj`, `.git`, git submodules, vendored libraries, generated files, test files (`*.test.*`, `*.spec.*`, `__tests__`), and Storybook stories.

Then ask with **one** AskUserQuestion call, in the user's language:

1. **Scope** (single-select), with the discovered counts in each description:

   | Option | Scope |
   |---|---|
   | Whole project | Every UI file in the repo (`<n>` files) |
   | Branch changes | UI files changed on this branch (`<n>` files). Report only issues on changed lines, but read whole files for context. Leave this option out if nothing changed |
   | A folder | Every UI file under a folder — ask for it next |
   | A file or component | One file, or a component's definition plus its stylesheet — ask for it next |

   Mark the best fit as "(Recommended)": *Branch changes* when there are changed UI files, otherwise *Whole project*. The user can also type a path directly through "Other" — accept it as the target.

2. **Frontend** (only in a monorepo, single-select) — list each frontend, plus "All of them". Review each chosen frontend separately; they usually have different token sources.

If the user already named a scope in the request ("review `src/components/Button.tsx`"), use it and skip the question.

If the user picks *A folder* or *A file or component*, ask one follow-up AskUserQuestion listing the most likely candidates found during discovery (top-level UI folders such as `src/components`, `app/`, `pages/`; or the most-imported components). The user types anything else through "Other". Resolve a component name to its definition and stylesheet with Grep/Glob; if the path or name matches nothing, say so and ask again.

Read **only** the matching platform reference (`references/tailwind.md` or `references/other-platforms.md`) and the matching `patterns-*.md` from this folder.

Count the UI files in scope, then choose the mode for Step 4:

- **≤ 30 files → File mode** (Step 4A)
- **> 30 files → Project mode** (Step 4B)

State the chosen mode and the file count at the top of the report.

## Step 3 — Foundations (always, in both modes)

Review the global setup **before** any component, whatever the scope or mode — even for a single-file or branch-changes review. Every later fix depends on what exists here.

Follow `foundations.md`: locate the foundations, report them as a 🔴 systemic issue with the full file to create if missing, check them if present, and build the value → token map. If `components.json` exists, also follow `shadcn.md`.

### Token policy for all later fixes

Every fix is written against the foundations — the existing ones, or the ones proposed for creation:

- **Design values go through tokens.** Spacing, font size, line-height, weight, radius, shadow, color, duration, easing, and control heights must reference a token (a Tailwind utility from the overridden scale such as `p-4`, a `var(--space-lg)`, `theme.space.lg`, `Theme.of(context)…`). Never fix a hardcoded off-scale value by replacing it with a different hardcoded value.
- **If the needed token does not exist, add it to the foundation** as part of a systemic finding, then use it in the fix. Do not hardcode as a workaround.
- **Raw values stay raw only where a token adds nothing:** `0`, `1px` borders, `100%`, asset-dependent sizes, and commented optical corrections.

## Step 4A — File mode

Read every file in scope and walk through all 18 items of the §9 self-check for each, plus the section rules they summarise (spacing, typography, font families and weights, CJK, radius/shadow/border, icon and control sizes, buttons, motion, color and contrast, explanatory text and notices).

## Step 4B — Project mode

Large projects cannot be read file by file without skipping files or drowning the real causes in repeats. After the foundations, work in two layers.

**Layer 1 — Pattern scan (every file).** Run the *Violation patterns* from the stack's `patterns-*.md` with Grep across all UI files in scope. Count occurrences per file; do not read files in full for this layer. Check each hit before counting it: skip control heights (`h-9`, `h-10`, `height: 40px`), deliberate exceptions with an explaining comment, and excluded paths. Note files with unusually high counts as hotspots.

**Layer 2 — Deep review of key components.** Read in full and run all 18 §9 checks on the components that set the look of the whole app — the rules that need understanding rather than grep (button states, primary-per-region, height alignment in rows, nested padding and radius, touch targets, contrast, explanatory text, notices):

- Primitives: Button, Input, Select, Checkbox/Switch, Dialog/Modal, Dropdown/Popover, Tooltip, Card, Badge, Table, and the shared notice / empty-state component if one exists
- Layout: app shell, header/nav, sidebar, page layout, page header
- The 3 most-used page-level components (by import count — Grep for the import path)

For shadcn projects the primitives are already covered by `shadcn.md` — spend this layer on the app-level wrappers and composites built on top of them (forms, data tables, dialogs with actions, nav).

Cap this layer at about 20 files. If there are more candidates, prefer the most imported and list the rest as not deep-reviewed.

## Step 4C — Token bypass (both modes)

Run this whenever foundations exist, in both modes and on every file in scope — including files that are otherwise on-scale. It catches pages that **ignore the global setup** and hardcode values that Steps 4A/4B miss because they happen to be on-scale (`padding: 16px` is on the 8pt grid, but still bypasses `--space-lg`).

Skip the foundation files themselves (and for shadcn, the `aliases.ui` primitives). Run the *Token-bypass patterns* from the stack's `patterns-*.md`, then look each hit up in the value → token map:

- **Value has a token** → the fix is that exact token (`padding: 16px` → `padding: var(--space-lg)`, `text-gray-500` → `text-muted-foreground`).
- **Value is off-scale** → the fix is the nearest token by the skill's rules (spacing to the nearest step, colors to the closest semantic role). Say which way it rounded.
- **Value has no semantic role** (a one-off brand color on one page) → propose the token to add to the foundation.

Report **token adoption per page**: for each page/route file (and its local stylesheet or CSS module), the number of bypasses. A page with many bypasses and few token uses was built outside the design system — report it as one file-level finding with its bypass count and its top offending values with their token replacements, instead of dozens of line-level findings.

## Rules for findings

- **Only report what you can point to.** Every finding needs a file and line number and the offending value quoted from the source. No speculative "might be" findings.
- **Cite the rule** by `SKILL.md` section (e.g. `§1 Spacing`, `§5 Buttons — States`, `§8 Explanatory text`).
- **Give a concrete fix**, not advice: the exact replacement class, token reference, or snippet, following the token policy. Every value must come from the skill's scales. For §8 findings the fix is the deletion, the redesign (with the snippet), or the notice markup.
- **Respect deliberate exceptions.** If an off-scale value has a comment explaining why (optical alignment, border compensation), do not flag it.
- **Contrast:** only report a failure when you can compute the ratio from actual color values; show the ratio. If colors come from variables you cannot resolve, list it under "Needs manual check" instead.
- **Group repeats.** If the same violation appears many times, report it once with all locations (or, above 10 locations, the count plus the top files).

| Severity | Meaning |
|---|---|
| 🔴 High | Accessibility or functional failure: missing foundations, contrast below minimum, removed focus ring, missing focus-visible/disabled state, touch target below platform minimum, icon-only button without an accessible name, meaning conveyed by color only, font declared but never loaded |
| 🟡 Medium | Visible inconsistency: raw colors bypassing semantic tokens (breaks theming / dark mode), local font stacks, token shadowing, a page built outside the design system, off-scale spacing, arbitrary values, line-height not a multiple of 4, button/input height mismatch in one row, more than one primary per region, `transition: all`, durations outside 100–400ms, no reduced-motion path, CJK weight/italic/tracking issues, a state the user must notice and fix shown as muted grey text instead of a notice, notices hand-rolled per screen instead of one shared component |
| 🔵 Low | Polish: on-scale raw values that bypass an existing token, missing `tabular-nums`, nested radius not decreasing, border plus heavy shadow, too many font sizes or weights, missing negative tracking on large Latin headings, subtitles or descriptions that fail the §8 test (restate the title, explain what structure could show, repeat across levels, or run longer than one sentence) |

## Step 5 — Report

Write the report in the language the user is using in the conversation, using the template for the chosen mode from `report-templates.md`. Omit empty sections. If there are no findings, say so plainly and list which checklist items were verified.

End by asking whether the user wants the fixes applied — do not apply them during the review. If foundations are missing, say that applying fixes will create the foundation files first and then rewrite the component fixes against them.
