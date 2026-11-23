# Report templates

## Finding block

Every finding, in every section, uses this block:

````markdown
### 1. <short title>
- **Location:** [Button.tsx:42](src/components/Button.tsx#L42)
- **Rule:** §5 Buttons — States
- **Problem:** <what is wrong and why it matters, one or two sentences>
- **Fix:**
  ```diff
  - className="outline-none"
  + className="focus-visible:ring-2 focus-visible:ring-ring focus-visible:ring-offset-2"
  ```
````

## File mode

```markdown
# UI Design Token Review — <target>

**Mode:** File · **Stack:** <stack> · **Files reviewed:** <n> · **Foundations:** <found: list of files | missing: list> · **Issues:** 🔴 <n> · 🟡 <n> · 🔵 <n>

## Foundations
<Step 3 findings: missing foundations (with the full file to create) or problems in existing ones>

## 🔴 High
## 🟡 Medium
## 🔵 Low

## Token bypass
| File | Bypasses | Top values → token |
|---|---|---|
| [Settings.tsx](…) | 14 | `#6b7280`→`text-muted-foreground`, `16px`→`p-4` |

## Needs manual check
## Unsupported stack — rule-based review
<stack name, then findings for those files from a direct reading against §1–§9 — no pattern scan or token-bypass counts>
## Summary
<2–4 sentences: the biggest patterns, and which fixes to do first — foundations always come first>
```

## Project mode

```markdown
# UI Design Token Review — <project>

**Mode:** Project · **Stack:** <stack> · **UI files:** <n> scanned, <n> deep-reviewed · **Foundations:** <found: list of files | missing: list> · **Issues:** 🔴 <n> · 🟡 <n> · 🔵 <n>

## Systemic issues — fix once, fixes everywhere
<Step 3 findings, missing foundations first (with the full file to create), then the most impactful; each notes how many usages it affects>

## Component issues
<Layer 2 findings, grouped by severity: 🔴 / 🟡 / 🔵>

## Pattern scan
| Rule | Severity | Occurrences | Files | Top files | Fix |
|---|---|---|---|---|---|
| §1 Off-scale spacing (`p-5`, `gap-1.5`, …) | 🟡 | 142 | 38 | [Dashboard.tsx](…) (21), [Settings.tsx](…) (17) | `p-5`→`p-4`/`p-6`, `gap-1.5`→`gap-2` |

## Token bypass — pages not using the global setup
| Page | Bypasses | Raw colors | Raw values | Inline styles | Top values → token |
|---|---|---|---|---|---|
| [Reports.tsx](…) | 37 | 12 | 21 | 4 | `#333`→`text-foreground`, `20px`→`p-6` (rounded up) |

## Hotspots
<files with the most violations overall — good candidates to refactor first>

## Not deep-reviewed
<files covered only by the pattern scan, if Layer 2 was capped>

## Needs manual check

## Unsupported stack — rule-based review
<stack name, then findings for those files from a direct reading against §1–§9 — no pattern scan or token-bypass counts>

## Suggested fix order
<numbered list: foundations / systemic issues first, then 🔴 items, then pages built outside the design system, then the highest-count patterns>
```
