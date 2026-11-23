# Grep patterns — React Native

Every hit must be checked before it counts (see Step 4B / 4C in `workflow.md`).

## Violation patterns (Step 4B Layer 1)

| Rule | Pattern | Notes |
|---|---|---|
| §1 Off-scale spacing | `(padding\|margin\|gap)\w*:\s*(5\|7\|9\|10\|13\|14\|15\|18\|20\|22\|28\|36\|40)\b` | |
| §2 Font size below 12px | `fontSize:\s*(\d\|1[01])\b` | |
| §2 Light weights | `fontWeight:\s*['"][123]00['"]` | Allowed only at display sizes (32px+) |
| §2 Italics on CJK | `fontStyle:\s*['"]italic['"]` | Only a finding on CJK text |
| §2 Line-height off grid | `lineHeight:\s*\d+` | Check the value is a multiple of 4 |
| §6 Duration out of range | `duration:\s*\d+` | Flag values outside 100–400 |
| §4–5 Icon-only buttons | `Pressable` / `TouchableOpacity` whose only child is an icon | Check for `accessibilityLabel` |
| §4 Touch targets | `hitSlop` missing on controls under 44×44 / 48×48 | |

## Token-bypass patterns (Step 4C)

| Bypass | Pattern | Notes |
|---|---|---|
| Raw colors | `color\w*:\s*['"](#\|rgb)` | |
| Raw design values | numeric `padding\|margin\|gap\|fontSize\|lineHeight\|borderRadius` inside `StyleSheet.create` | Only when not taken from the theme; includes on-scale values |
| Inline styles | `style=\{\{` with design properties | |
| Local font stacks | `fontFamily:` | Outside the theme file |
