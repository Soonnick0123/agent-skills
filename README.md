# Agent skills

English | [Traditional Chinese](README.zh-TW.md)

Skills for AI coding agents (Claude Code, Codex and others that read `SKILL.md`).

This repository is also a Claude Code plugin marketplace, `soonnick-skills`. Each skill is its
own plugin, so you install only the skills you need.

## Skills

| Skill | Use it to | Details |
|---|---|---|
| **show-me-html** | Turn any material into one self-contained HTML document that people understand quickly: an explainer, handbook, runbook, decision record, or a summary of a conversation. Diagrams, step-through stages, light/dark mode, PDF export. [Live demo →](https://soonnick0123.github.io/agent-skills/demo/price-lookup-demo.html) | [README](skills/show-me-html/README.md) |
| **ui-design-tokens** | Keep generated UI on one scale for spacing, type, radius, shadow and motion, with rules for buttons, contrast and explanatory text. Also reviews existing UI and reports each violation with a fix. | [README](skills/ui-design-tokens/README.md) |

## Install

Replace `<skill>` with a name from the table. For the Claude Code plugin, you can also use
`all-skills` to install every skill as one plugin. Install `all-skills` or single skills, not
both: otherwise each skill loads twice.

**Claude Code (plugin)**: in a Claude Code session, run:

```
/plugin install <skill> --marketplace Soonnick0123/agent-skills
```

This needs Claude Code 2.1.275 or later. On an older version, add the marketplace first:

```
/plugin marketplace add Soonnick0123/agent-skills
/plugin install <skill>@soonnick-skills
```

To get newer versions later, run `/plugin marketplace update soonnick-skills`. To update
automatically, turn on auto-update for `soonnick-skills` under **Marketplaces** in `/plugin`.

**Other agents (Codex and others), or Claude Code without the plugin**:

```sh
npx skills add Soonnick0123/agent-skills --skill <skill>
```

Or copy `skills/<skill>/` into `~/.claude/skills/` (or `.claude/skills/` in a project).

Each skill's README has its own install notes and examples.

## License

MIT. See [LICENSE](LICENSE).
