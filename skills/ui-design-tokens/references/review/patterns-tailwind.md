# Grep patterns — Tailwind

Every hit must be checked before it counts (see Step 4B / 4C in `workflow.md`). If the project also has component-level `.css` / `.module.css` files, run `patterns-css.md` on those as well.

## Violation patterns (Step 4B Layer 1)

| Rule | Pattern | Notes |
|---|---|---|
| §1 Off-scale spacing | `\b-?(p\|px\|py\|pt\|pr\|pb\|pl\|ps\|pe\|m\|mx\|my\|mt\|mr\|mb\|ml\|ms\|me\|gap\|gap-x\|gap-y\|space-x\|space-y\|inset\|top\|right\|bottom\|left)-(5\|7\|9\|10\|11\|14\|\d+\.5)\b` | `h-9` / `h-10` are control heights, not spacing |
| §1–2 Arbitrary values | `-\[\d+(\.\d+)?(px\|rem\|em\|ms)\]` | |
| §2 Font size below 12px | `text-\[(\d\|1[01])px\]` | |
| §2 Light weights | `font-(thin\|extralight\|light)\b` | Allowed only at display sizes (32px+) |
| §2 Italics on CJK | `\bitalic\b` | Only a finding on CJK text |
| §2 Line-height off grid | `leading-\[` | Check the value is a multiple of 4 |
| §6 `transition: all` | `transition-all` | |
| §6 Duration out of range | `duration-(75\|500\|700\|1000)\b`, `duration-\[` | |
| §7 Removed focus ring | `\boutline-none\b` | Fine when the same element has a `focus-visible:ring-*` |
| §4–5 Icon-only buttons | `<button` / `<Button` whose only child is an icon | Check for `aria-label` |

## Token-bypass patterns (Step 4C)

| Bypass | Pattern | Notes |
|---|---|---|
| Raw colors (arbitrary) | `(bg\|text\|border\|ring\|outline\|fill\|stroke\|from\|via\|to\|divide\|shadow)-\[(#\|rgb\|hsl\|oklch)` | |
| Raw palette colors | `(bg\|text\|border\|ring\|divide\|fill\|stroke)-(white\|black\|(slate\|gray\|zinc\|neutral\|stone\|red\|orange\|amber\|yellow\|lime\|green\|emerald\|teal\|cyan\|sky\|blue\|indigo\|violet\|purple\|fuchsia\|pink\|rose)-\d{2,3})\b` | Only when the project defines semantic color tokens |
| Raw design values | `-\[` arbitrary values of any kind | Includes on-scale ones like `p-[16px]` |
| Inline styles | `style=\{\{` with design properties | |
| Local font stacks | `font-\[` | |
