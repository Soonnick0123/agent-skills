# Agent skills

[English](README.md) | 繁體中文

給 AI 編程 agent 用的 skill（Claude Code、Codex，以及其他會讀 `SKILL.md` 的 agent）。

這個 repo 同時也是一個 Claude Code plugin marketplace，名稱是 `soonnick-skills`。

## Skill 一覽

| Skill | 用途 | 詳情 |
|---|---|---|
| **show-me-html** | 把任何資料整理成一份獨立的 HTML 文件，讓人很快看懂：機制說明、手冊、runbook、決策記錄，或者一段對話的總結。附流程圖、分步驟講解、淺色／深色模式、PDF 匯出。[線上 demo →](https://soonnick0123.github.io/agent-skills/demo/price-lookup-demo.html) | [README](skills/show-me-html/README.zh-TW.md) |
| **ui-design-tokens** | 讓生成的 UI 在間距、字體、圓角、陰影、動畫上都用同一套刻度，另有按鈕、對比度、說明文字的規則。也可以審查現有的 UI，每個問題都附上修正方法。 | [README](skills/ui-design-tokens/README.zh-TW.md) |

## 安裝

**Claude Code**：在終端機執行，然後重新啟動 Claude Code。

安裝全部 skill：

```sh
claude plugin marketplace add Soonnick0123/agent-skills
claude plugin install all-skills@soonnick-skills
```

或者只裝需要的 skill：

```sh
claude plugin marketplace add Soonnick0123/agent-skills
claude plugin install show-me-html@soonnick-skills
claude plugin install ui-design-tokens@soonnick-skills
```

`all-skills` 和單獨的 skill 只能二選一，兩種都裝的話，每個 skill 都會載入兩次。

更新到最新版本：

```sh
claude plugin marketplace update soonnick-skills
claude plugin update all-skills@soonnick-skills
```

**其他 agent（Codex 等）**：

```sh
npx skills add Soonnick0123/agent-skills --skill show-me-html
npx skills add Soonnick0123/agent-skills --skill ui-design-tokens
```

## 授權

MIT，詳見 [LICENSE](LICENSE)。
