# ui-design-tokens

[English](README.md) | 繁體中文

讓生成的 UI 保持一致。所有間距、字體、圓角、陰影和動畫的數值，都來自一套固定的小刻度，agent 只能從表格裏挑選，不能自己發明數字。

- **間距**：8pt 網格（4 到 96 px），附巢狀、分組和標題節奏的規則。
- **字體**：模組化字級，行高對齊 4px 網格，最多兩種字體、三種字重，而且全部都要確實載入。包含中日韓文字（CJK）的規則。
- **元件**：圓角、陰影、圖示和控制項尺寸、按鈕高度、主次層級和各種狀態、觸控範圍。
- **動畫和顏色**：動畫時長 100 到 400 ms、緩動曲線、減少動態效果、WCAG 對比度、語意化顏色 token。
- **說明文字**：刪掉只是重複標題的副標題。需要用戶處理的狀態，用帶修正按鈕的提示元件顯示，不用灰色小字。
- **自我檢查**：一份 18 項的清單，agent 交出 UI 代碼之前會逐項檢查。
- **審查模式**：只讀地審查現有的 UI。先問審查範圍，再檢查全局設定，然後列出每個問題的檔案、行號，以及用 token 寫的修正方法。完整審查支援 Tailwind（含 shadcn/ui）、CSS／SCSS／Less／CSS-in-JS、React Native 和 Flutter。其他技術則由 agent 直接按規則審查代碼。

## 安裝

**Claude Code（plugin）**：

```
/plugin install ui-design-tokens --marketplace Soonnick0123/agent-skills
```

Claude Code 2.1.275 以下的版本：

```
/plugin marketplace add Soonnick0123/agent-skills
/plugin install ui-design-tokens@soonnick-skills
```

以 plugin 安裝時，skill 的名稱是 `ui-design-tokens:ui-design-tokens`。

**其他 agent，或不用 plugin 的 Claude Code**：

```sh
npx skills add Soonnick0123/agent-skills --skill ui-design-tokens
```

也可以把 `skills/ui-design-tokens/` 複製到 `~/.claude/skills/`（或項目裏的 `.claude/skills/`）。

## 使用

agent 建立或修改 UI 時，這個 skill 會自動啟用。要審查現有的代碼，直接提出要求，例如：

- 「按照 ui-design-tokens 審查這個頁面。」
- 「幫我 review `src/components` 裏的 UI。」
- "Review the UI in `src/components` against the design tokens."

## 檔案

| 檔案 | 用途 |
|---|---|
| `SKILL.md` | 規則、自我檢查清單，以及何時使用審查模式 |
| `references/tailwind.md` | Tailwind 和 shadcn/ui：utility 對照、標準化的 `buttonVariants`、設定檔 |
| `references/other-platforms.md` | 純 CSS、React Native 和 Flutter 的 token 設定 |
| `references/review/workflow.md` | 審查步驟、嚴重程度分級、支援的技術 |
| `references/review/*.md` | 全局設定檢查、shadcn 檢查、各技術的 grep 規則、報告模板 |

## 授權

MIT，詳見 [LICENSE](../../LICENSE)。
