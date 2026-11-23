# Grep patterns — Flutter

Every hit must be checked before it counts (see Step 4B / 4C in `workflow.md`).

## Violation patterns (Step 4B Layer 1)

| Rule | Pattern | Notes |
|---|---|---|
| §1 Off-scale spacing | `EdgeInsets[^)]*\b(5\|7\|9\|10\|14\|18\|20\|28)\b` | |
| §2 Font size below 12px | `fontSize:\s*(\d\|1[01])\b` | |
| §2 Light weights | `FontWeight\.w[123]00` | Allowed only at display sizes (32px+) |
| §2 Italics on CJK | `FontStyle\.italic` | Only a finding on CJK text |
| §2 Line-height off grid | `height:\s*[\d.]+` inside `TextStyle` | `fontSize × height` must be a multiple of 4 |
| §6 Duration out of range | `Duration\(milliseconds:\s*\d+` | Flag values outside 100–400 |
| §4–5 Icon-only buttons | `IconButton(` without `tooltip:` | |

## Token-bypass patterns (Step 4C)

| Bypass | Pattern | Notes |
|---|---|---|
| Raw colors | `Color\(0x`, `Colors\.\w+` | Outside the theme file |
| Raw design values | `EdgeInsets\.\w+\(\s*\d`, `BorderRadius\.circular\(\d`, `TextStyle\([^)]*fontSize:\s*\d`, `SizedBox\((height\|width):\s*\d` | Includes on-scale values |
| Local font stacks | `fontFamily:` | Outside the theme file |
| Token shadowing | local `ThemeData(` / `Theme(` overriding the app theme | |
