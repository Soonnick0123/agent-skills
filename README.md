# Agent skills

English | [Traditional Chinese](README.zh-TW.md)

Skills for AI coding agents (Claude Code, Codex and others that read `SKILL.md`).

This repository is also a Claude Code plugin marketplace, `soonnick-skills`.

## Skills

| Skill | Use it to | Details |
|---|---|---|
| **show-me-html** | Turn any material into one self-contained HTML document that people understand quickly: an explainer, handbook, runbook, decision record, or a summary of a conversation. Diagrams, step-through stages, light/dark mode, PDF export. [Live demo →](https://soonnick0123.github.io/agent-skills/demo/price-lookup-demo.html) | [README](skills/show-me-html/README.md) |
| **ui-design-tokens** | Keep generated UI on one scale for spacing, type, radius, shadow and motion, with rules for buttons, contrast and explanatory text. Also reviews existing UI and reports each violation with a fix. | [README](skills/ui-design-tokens/README.md) |

## Install

**Claude Code**: run in a terminal, then restart Claude Code.

All skills:

```sh
claude plugin marketplace add Soonnick0123/agent-skills
claude plugin install all-skills@soonnick-skills
```

Or only the skills you need:

```sh
claude plugin marketplace add Soonnick0123/agent-skills
claude plugin install show-me-html@soonnick-skills
claude plugin install ui-design-tokens@soonnick-skills
```

Install `all-skills` or single skills, not both, or each skill loads twice.

Update to the latest version:

```sh
claude plugin marketplace update soonnick-skills
claude plugin update all-skills@soonnick-skills
```

**Other agents (Codex and others)**:

```sh
npx skills add Soonnick0123/agent-skills --skill show-me-html
npx skills add Soonnick0123/agent-skills --skill ui-design-tokens
```

## License

MIT. See [LICENSE](LICENSE).
