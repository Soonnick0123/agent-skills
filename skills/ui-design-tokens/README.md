# ui-design-tokens

English | [Traditional Chinese](README.zh-TW.md)

Keeps generated UI consistent. Every spacing, type, radius, shadow and motion value comes from
a small, closed scale, so the agent picks from a table instead of inventing a number.

- **Spacing**: an 8pt grid (4 to 96 px), with rules for nesting, grouping and heading rhythm.
- **Typography**: a modular type scale, line-heights on the 4px grid, at most two font
  families and three weights, all of them loaded. CJK rules included.
- **Components**: radius, shadow, icon and control sizes, button heights, hierarchy and states,
  touch targets.
- **Motion and color**: durations from 100 to 400 ms, easing, reduced motion, WCAG contrast,
  semantic color tokens.
- **Explanatory text**: removes subtitles that only restate the title. A state that the user
  must fix gets a notice with the fix as a button, not grey text.
- **Self-check**: an 18-item list that the agent runs before it returns UI code.
- **Review mode**: a read-only audit of existing UI. It asks for the scope, checks the global
  setup first, and reports each violation with its file, line and a token-based fix. The full
  review supports Tailwind (including shadcn/ui), CSS / SCSS / Less / CSS-in-JS, React Native
  and Flutter. For other stacks, the agent reviews the code against the rules directly.

## Install

**Claude Code (plugin)**:

```
/plugin install ui-design-tokens --marketplace Soonnick0123/agent-skills
```

On Claude Code older than 2.1.275:

```
/plugin marketplace add Soonnick0123/agent-skills
/plugin install ui-design-tokens@soonnick-skills
```

As a plugin, the skill is named `ui-design-tokens:ui-design-tokens`.

**Other agents, or Claude Code without the plugin**:

```sh
npx skills add Soonnick0123/agent-skills --skill ui-design-tokens
```

Or copy `skills/ui-design-tokens/` into `~/.claude/skills/` (or `.claude/skills/` in a project).

## Use

The skill starts by itself when the agent builds or changes UI. To review existing code, ask
for it, for example:

- "Review the UI in `src/components` against the design tokens."

## Files

| File | Purpose |
|---|---|
| `SKILL.md` | The rules, the self-check, and when to use review mode |
| `references/tailwind.md` | Tailwind and shadcn/ui: utility mapping, normalized `buttonVariants`, config |
| `references/other-platforms.md` | Plain CSS, React Native and Flutter token setups |
| `references/review/workflow.md` | The review steps, severity levels and supported stacks |
| `references/review/*.md` | Global-setup checks, shadcn checks, grep patterns per stack, report templates |

## License

MIT. See [LICENSE](../../LICENSE).
