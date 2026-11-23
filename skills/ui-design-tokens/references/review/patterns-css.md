# Grep patterns — CSS / SCSS / CSS-in-JS

Applies to `.css`, `.scss`, `.less`, CSS modules, and CSS-in-JS (`styled-components`, `@emotion`) template literals. Every hit must be checked before it counts (see Step 4B / 4C in `workflow.md`).

## Violation patterns (Step 4B Layer 1)

| Rule | Pattern | Notes |
|---|---|---|
| §1 Off-scale spacing | `(padding\|margin\|gap\|inset)[^;]*\b(5\|7\|9\|10\|13\|14\|15\|18\|20\|22\|28\|36\|40)px` | |
| §2 Font size below 12px | `font-size:\s*(\d\|1[01])px` | |
| §2 Light weights | `font-weight:\s*[123]00` | Allowed only at display sizes (32px+) |
| §2 Italics on CJK | `font-style:\s*italic` | Only a finding on CJK text |
| §2 Line-height off grid | `line-height:\s*\d+px` | Check the value is a multiple of 4 |
| §6 `transition: all` | `transition:\s*all` | |
| §6 Duration out of range | `\b(\d{1,2}\|[5-9]\d\d\|\d{4,})ms\b`, `\b0?\.[5-9]\d*s\b` | |
| §6 Animating layout props | `transition[^;]*(width\|height\|top\|left\|margin\|padding)` | |
| §7 Removed focus ring | `outline:\s*(none\|0)` | Fine when a `:focus-visible` style replaces it |
| §4–5 Icon-only buttons | `<button` whose only child is an icon (in the markup) | Check for `aria-label` |

## Token-bypass patterns (Step 4C)

| Bypass | Pattern | Notes |
|---|---|---|
| Raw colors | `#[0-9a-fA-F]{3,8}\b`, `\b(rgba?\|hsla?\|oklch)\(` | Not when wrapped in `var(` or defining a token |
| Raw design values | `(padding\|margin\|gap\|font-size\|line-height\|font-weight\|border-radius\|box-shadow\|transition\|animation)[^;]*\d+(\.\d+)?(px\|rem\|em\|ms\|s)\b` | Only on lines without `var(`; includes on-scale values |
| Inline styles | `style="` in HTML / Vue / Svelte templates | |
| Local font stacks | `font-family:` | |
| Token shadowing | `--[\w-]+:` redefining a foundation token | Outside the foundation files only |
