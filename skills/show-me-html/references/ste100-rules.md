# ASD-STE100 writing rules for explainer pages

ASD-STE100 (Simplified Technical English) comes from aircraft maintenance manuals. The readers
there are often not native English speakers, and a misread instruction is dangerous. The same
properties help a team read an internal mechanism page quickly and act on it correctly.

## Contents
1. Sentences
2. Procedures
3. Vocabulary
4. Preconditions, cautions and warnings
5. What not to rewrite
6. Before / after examples
7. Self-check
8. Chinese (中文) rules

## 1. Sentences

- **One idea per sentence.** Split "X happens, and then Y, which means Z" into three sentences.
- **Descriptive sentences: 25 words or fewer.** Procedural sentences: 20 words or fewer.
- **Active voice.** "The server deletes the old backups", not "The old backups are deleted".
- **Present tense** for how the system behaves.
- **Say who does it.** Every sentence names its actor: the server, the backup job, an admin.
- **Prefer a list to a paragraph** for three or more related facts.
- No idioms, no metaphors, no "basically", "simply", "just".
- No dashes or parentheses that hide a second idea. Make the second idea its own sentence.

## 2. Procedures

- Number the steps (`<ol>`). One action per step.
- Start each step with the verb: "Click **Save**." "Select the date and the time."
- Put the result of the procedure after the steps, as a separate short note.
- A check is its own step: "Make sure that the status page shows the next run time."
- Use "To do X" as the procedure title.

## 3. Vocabulary

- **One word, one meaning.** Pick a term for each thing and use only that term on the page.
  Do not switch to a synonym for variety. If the page says "server", it never says "host",
  "machine" or "node" for the same thing.
- Put the chosen terms in a **Terms** table at the top of the page.
- Prefer simple, common verbs: use, make, send, keep, start, stop, show, find.
- Avoid words with more than one meaning in context ("run", "fire", "boot", "handle").
- Numbers and units: "10 minutes", "every 12 hours", "approximately 1 minute".

## 4. Preconditions, cautions and warnings

Keep these out of the steps. Each goes in its own box before the steps it applies to
(or immediately after the procedure, for cautions about the result).

- **Before you start**: conditions that must be true before step 1.
- **Caution**: an action or condition that can cause data loss, downtime or wrong behaviour.
  State the effect: "The service stops for approximately 1 minute."
- **Warning**: only for risk to people. Rare in software documentation.

Each item in these boxes is one sentence.

## 5. What not to rewrite

- **UI strings stay verbatim.** Button labels, headings and captions appear exactly as the
  product shows them, even if they break STE rules. The reader must recognise them on screen.
- Code identifiers, file paths, endpoints and config keys stay verbatim, in `<code>`.

## 6. Before / after examples

Before: "The schedule is kept by the backup job itself, so even if the network is down at that
moment, the backup still runs on time."
After:
- The backup job keeps the schedule.
- The backup job starts with its own timer.
- The backup job does not need the network at the start time.

Before: "Once confirmed, the job runs as soon as the upload finishes, without anyone entering
the admin password."
After:
1. On the job page, click **Run now**.
2. In the dialog, click **Run now**.

The backup job starts when the upload is complete. The backup job does not ask for the admin
password.

## 7. Self-check

Before handing over the page, scan it for:
- any sentence with "and", "which" or "so" that joins two ideas
- any step longer than 20 words
- any term used that is not in the Terms table, or two names for one thing
- a precondition or caution hidden inside a step
- passive voice without an actor

## 8. Chinese (中文) rules

ASD-STE100 is an English standard. For a Chinese page, keep its intent (one idea per sentence,
literal wording, one term per thing) and use the rules below. Sections 1–5 still apply where
they are not language-specific (actors, present tense, procedure layout, boxes, verbatim UI).

### 8.1 Length (count characters, not words)

- 描述句不超過 **40 字**。
- 步驟每步不超過 **30 字**。
- 標點、英文 UI 字串和程式碼名稱不計入字數，但一句裡最多放一個英文字串。

### 8.2 Sentences

- **一句只說一件事。** 一個逗號後面接的是另一件事，就改成句號或分點。
- **主詞明確。** 每句說出誰做：「伺服器刪除舊備份」，不寫「進行刪除」。
- **不用「被」字句。** 「系統清除快取」，不寫「快取被清除」。
- **少用虛詞。** 刪掉「進行」、「予以」、「加以」、「做出」、「實施」。
  「進行下載」→「下載」。
- **拆開長定語。** 「的」字超過兩個的名詞片語，拆成兩句。
  - 前：「備份工作在執行時間後十分鐘內啟動時會立即執行的備份」
  - 後：「備份工作在寬限期內啟動。備份工作立即執行備份。」
- **不用模糊程度詞。** 刪掉「基本上」、「一般來說」、「大概」、「稍微」。
  要表達約數時寫「約 1 分鐘」。
- **否定直接寫。** 用「不」、「不會」，不寫「並非不會」這類雙重否定。

### 8.3 Procedures

- 步驟用編號，動詞開頭：「點選 **Save**。」「選擇日期和時間。」
- 一步一個動作。不寫「點選 A 並選擇 B」。
- 介面上的按鈕、標題照原樣用英文並加粗：點選 **Run now**。
- 檢查也是一步：「確認狀態頁面顯示下次執行時間。」
- 程序標題用「如何…」或「要…時」：「如何設定備份排程」。

### 8.4 Vocabulary

- **一個東西只用一個詞。** 在術語表定好，全頁只用那個詞。
  - 例：定為「伺服器」，就不混用「主機」、「機器」、「節點」。
  - 例：定為「執行時間」，就不混用「排定時間」、「預定時間」、「啟動時間」。
- 術語表兩欄：「術語」與「意思」。需要時加第三欄放對應的英文或程式碼名稱。
- 動詞也要一致：選定「點選」就不混用「按」、「點擊」、「按下」。
- 用台灣／香港常見的繁體用詞，全頁統一一種（例如「設定」不混用「設置」）。

### 8.5 Mixed Chinese and English

- UI 字串、程式碼名稱、檔案路徑、設定鍵保留英文原文。程式碼類放在 `<code>`。
- 中文與英文、數字之間加半形空格：「伺服器每 15 秒檢查一次」。
- 全形標點用於中文句子；英文字串內部保留半形標點。
- 不在中文句子裡用英文代替一般中文詞（不寫「這個 job 會被 cancel」）。

### 8.6 Boxes

- 前置條件框標題：「開始之前」。
- 注意事項框標題：「注意」。只有涉及人身安全才用「警告」。
- 框內每項一句。

### 8.7 Before / after

前：「排程是備份工作自己保管的，所以即使當時網路斷了，備份仍然會照時間執行。」
後：
- 備份工作保存排程。
- 備份工作用自己的計時器啟動。
- 執行時間到時，備份工作不需要網路。

前：「確認後上傳完就立刻執行，不需要輸入管理員密碼。」
後：
1. 在工作頁面上點選 **Run now**。
2. 在對話框中點選 **Run now**。

上傳完成後，備份工作開始執行。備份工作不要求輸入管理員密碼。

### 8.8 Self-check (中文)

交付前檢查：
- 有沒有一個逗號串起兩件事的句子
- 有沒有超過 30 字的步驟、超過 40 字的描述句
- 有沒有「被」、「進行」、「予以」
- 有沒有術語表以外的詞，或同一個東西用了兩個名稱
- 動詞是否一致（點選／按／點擊）
- 中英文之間有沒有空格
- 前置條件或注意事項有沒有混在步驟裡
