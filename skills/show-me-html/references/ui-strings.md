# UI strings of the shell and the components

The page bar, the menus, the PDF help, the buttons and the text that the scripts write are
part of the page. Write them in the page language, like the content. A Chinese page with an
English "Contents" button reads as unfinished.

How to use this table:

- Replace every string in the "Where" column when the page is not in English. Change the
  string only; keep the markup, the attributes and the script logic.
- A row with `{…}` is a template. Replace the whole sentence, and keep the value in it
  (`{title}`, `{date}`, `{n}`). In the script, rebuild the sentence around the value; do not
  join English words to the value.
- Browser names in the PDF help (Destination, Save as PDF, Save) must match what the browser
  shows in that language. The table uses the names in Chrome, Edge and Safari.
- For a language that is not in the table, translate with the same meaning, and use the
  browser's own names for its print window.

## Page bar and contents (`assets/page-shell.html`)

| Where | English | 繁體中文 | 简体中文 |
|---|---|---|---|
| `.nav-btn` text, both contents `<nav aria-label>` | Contents | 目錄 | 目录 |
| side list label `.pop-label` | On this page | 本頁內容 | 本页内容 |
| `.pdf-btn` text | PDF | PDF | PDF |
| `.pdf-btn` `aria-label` | Save as PDF | 另存為 PDF | 另存为 PDF |
| `.pdf-btn` `title` | Save as PDF: in the print window, choose Save as PDF | 另存為 PDF：在列印視窗選擇「另存為 PDF」 | 另存为 PDF：在打印窗口选择“另存为 PDF” |
| `.set-btn` `aria-label` and `title`, `#docSettings` `aria-label` | Display settings | 顯示設定 | 显示设置 |
| settings label | Theme | 主題 | 主题 |
| theme buttons | Auto / Light / Dark | 自動 / 淺色 / 深色 | 自动 / 浅色 / 深色 |
| Auto button `title` | Follow the system | 跟隨系統 | 跟随系统 |
| settings label | Text size | 字級 | 字号 |
| A− `aria-label` | Smaller text | 縮小文字 | 缩小文字 |
| A+ `aria-label` | Larger text | 放大文字 | 放大文字 |
| "#" link `aria-label` (script) | Copy the link to {title} | 複製「{title}」的連結 | 复制“{title}”的链接 |
| "#" link after a click (script) | Link copied | 已複製連結 | 已复制链接 |
| top line on paper (script) | Exported on {date}. / Exported on {date} from {url}. | 匯出日期：{date}。 / 匯出日期：{date}，來源：{url}。 | 导出日期：{date}。 / 导出日期：{date}，来源：{url}。 |
| `.scroll-hint` | Swipe sideways to see all of it. | 左右滑動可看到全部內容。 | 左右滑动可看到全部内容。 |

Keep the date as `YYYY-MM-DD` in every language.

## PDF help (`.pdf-dialog`)

| Where | English | 繁體中文 | 简体中文 |
|---|---|---|---|
| title | Save this page as a PDF | 把這一頁存成 PDF | 把这一页存成 PDF |
| step 1 | Click **Open print window**. | 按「**開啟列印視窗**」。 | 点击“**打开打印窗口**”。 |
| step 2 | In **Printer** or **Destination**, select **Save as PDF**. | 在「**印表機**」或「**目的地**」選擇「**另存為 PDF**」。 | 在“**打印机**”或“**目标打印机**”中选择“**另存为 PDF**”。 |
| step 3 | Click **Save**. | 按「**儲存**」。 | 点击“**保存**”。 |
| Safari note | In Safari, click **PDF** at the bottom left, then **Save as PDF**. | 在 Safari 中，按左下角的「**PDF**」，再選擇「**儲存為 PDF**」。 | 在 Safari 中，点击左下角的“**PDF**”，再选择“**存储为 PDF**”。 |
| check box | Do not show this again | 不再顯示 | 不再显示 |
| buttons | Cancel / Open print window | 取消 / 開啟列印視窗 | 取消 / 打开打印窗口 |

Step 1 and the button must use the same words.

## Components (`assets/component-reference.html`)

| Where | English | 繁體中文 | 简体中文 |
|---|---|---|---|
| "Based on" label | Based on | 依據 | 依据 |
| callout labels | Before you start / Cautions / Rule | 開始之前 / 注意事項 / 規則 | 开始之前 / 注意事项 / 规则 |
| stepper buttons | Back / Next / Play / Pause | 上一步 / 下一步 / 播放 / 暫停 | 上一步 / 下一步 / 播放 / 暂停 |
| stage card (script) | {n} · arrow {ids} / {n} · arrows {ids} / {n} · no arrow | {n} · 箭頭 {ids} / {n} · 無箭頭 | {n} · 箭头 {ids} / {n} · 无箭头 |
| stage on paper (script) | Arrow {ids} / Arrows {ids} / No arrow | 箭頭 {ids} / 無箭頭 | 箭头 {ids} / 无箭头 |
| stage diagram on paper `aria-label` (script) | Stage {n}, {title}: the parts and arrows of this stage are highlighted. | 第 {n} 階段「{title}」：圖中標出這個階段的元件和箭頭。 | 第 {n} 阶段“{title}”：图中标出这个阶段的组件和箭头。 |
| demo card heading | Try it: … | 試試看：… | 试一试：… |

The words "stage", "arrow" and "part" are terms of the page. Use the same Chinese word in
the stepper, in the diagram labels and in the text, and list them in the Terms table when the
page has one.
