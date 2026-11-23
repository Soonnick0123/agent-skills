# Agent skills

[English](README.md) | 繁體中文

給 AI 編程 agent 用的 skill（Claude Code、Codex，以及其他會讀 `SKILL.md` 的 agent）。

這個 repo 同時也是一個 Claude Code plugin marketplace，名稱是 `soonnick-skills`。每個 skill 都是獨立的 plugin，你只需要安裝用得到的。

## Skill 一覽

| Skill | 用途 | 詳情 |
|---|---|---|
| **show-me-html** | 把任何資料整理成一份獨立的 HTML 文件，讓人很快看懂：機制說明、手冊、runbook、決策記錄，或者一段對話的總結。附流程圖、分步驟講解、淺色／深色模式、PDF 匯出。[線上 demo →](https://soonnick0123.github.io/agent-skills/demo/price-lookup-demo.html) | [README](skills/show-me-html/README.zh-TW.md) |
| **ui-design-tokens** | 讓生成的 UI 在間距、字體、圓角、陰影、動畫上都用同一套刻度，另有按鈕、對比度、說明文字的規則。也可以審查現有的 UI，每個問題都附上修正方法。 | [README](skills/ui-design-tokens/README.zh-TW.md) |

## 安裝

把 `<skill>` 換成上表中的名稱。用 Claude Code plugin 安裝時，也可以換成 `all-skills`，把全部 skill 裝成一個 plugin。`all-skills` 和單獨的 skill 只能二選一，兩種都裝的話，每個 skill 都會載入兩次。

**Claude Code（plugin）**：在 Claude Code 對話中執行：

```
/plugin install <skill> --marketplace Soonnick0123/agent-skills
```

這需要 Claude Code 2.1.275 或以上版本。舊版本要先加入 marketplace：

```
/plugin marketplace add Soonnick0123/agent-skills
/plugin install <skill>@soonnick-skills
```

以後要更新，執行 `/plugin marketplace update soonnick-skills`。想自動更新的話，在 `/plugin` 的 **Marketplaces** 裏為 `soonnick-skills` 開啟自動更新。

**其他 agent（Codex 等），或不用 plugin 的 Claude Code**：

```sh
npx skills add Soonnick0123/agent-skills --skill <skill>
```

也可以把 `skills/<skill>/` 複製到 `~/.claude/skills/`（或項目裏的 `.claude/skills/`）。

每個 skill 的 README 都有各自的安裝說明和使用例子。

## 授權

MIT，詳見 [LICENSE](LICENSE)。
