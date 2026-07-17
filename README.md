# learning_news

每個平日早上自動蒐集的 IT / 後端相關新聞與資訊摘要，由排程 agent 產生並開 PR。

## 結構

- `AGENTS.md` — 這個 repo 的**唯一事實來源**：完整的選文/開 PR 指令，廠商中立，任何具備網路
  搜尋與 GitHub 讀寫能力的 AI agent 都能照它獨立執行。
- `digests/YYYY-MM-DD.md` — 每天的 15 則精選新聞/資訊摘要。
- `preferences.md` — 累積記錄使用者對過去內容的喜歡/不喜歡回饋，用來調整之後每次挑選的主題與來源權重。

## 流程

1. 讀取 `preferences.md` 了解目前偏好。
2. 依 `AGENTS.md` 的規則搜尋近期 IT / 後端相關新聞，挑選 15 則，寫成 `digests/YYYY-MM-DD.md`。
3. 開一個 PR（`digest-YYYY-MM-DD` -> `main`），內容為當天的摘要檔案。
4. 使用者在對話中給回饋後，偏好會被追加寫入 `preferences.md`（同一個 PR 或後續 commit）。
5. 使用者自行決定是否 merge 該 PR。

## 如何用任何 AI 執行

這個 repo 是自足的：`git clone` 之後，把下面這句話貼給任何一個具備「網路搜尋」與「對這個
repo 的 GitHub 讀寫/開 PR」能力的 AI（Claude、Gemini、ChatGPT/Codex 等皆可），就能重現同樣的
行為：

> 請 clone 或開啟 GitHub repo `BillChai/learning_news`，讀取根目錄的 `AGENTS.md` 並完整照做，
> 今天日期是 YYYY-MM-DD。

**注意事項**：

- 每個 AI 帳號**第一次**使用前，需要人工在該 App/服務的設定裡做一次性 GitHub 授權（例如
  Claude 的 Connectors、ChatGPT Codex 的 repo 綁定、Jules 的 repo 選擇）。這一步無法寫進上面
  那句 kickoff prompt 裡，是純人工設定，且是「每個帳號一次」而不是「每次執行都要做」。
- 避免同一天讓兩個 agent 各自跑一次——`AGENTS.md` 裡雖然有處理分支撞名的規則（自動改用
  `-2`、`-3` 尾碼），但 `preferences.md` 的回饋追加順序仍可能因此變得混亂，建議一天只執行一次。

## 手機執行

每日排程本身就是無人值守執行的，不需要手機介入。以下路徑是給「臨時手動觸發」或「想換一個
AI 跑」時使用：

- **Claude App**（iOS/Android）：帳號在 Settings → Connectors 設定一次 GitHub Connector 後，
  手機 app 的行為與桌面/網頁版一致，直接貼上面的 kickoff prompt 即可。
- **ChatGPT App**（Codex cloud）：建立一個綁定 `BillChai/learning_news` 的 Codex cloud
  task（一次性設定），Codex 會原生讀取 repo 根目錄的 `AGENTS.md`，手機上開新 task 貼 kickoff
  prompt 即可。
- **Google Jules**（jules.google）：可從手機瀏覽器開啟，選好 repo/branch 後貼上同樣的 kickoff
  prompt；體驗比前兩者卡一些，官方目前沒有原生手機 app。

**未來可選項（目前未實作）**：若以上三條路徑都不夠用，可以考慮蓋一個 GitHub Actions
`workflow_dispatch` fallback——手機上在 GitHub App 裡點一下「Run workflow」就能觸發，完全不
依賴任何聊天 App。但這等於是另外做一個小後端專案（需要自己接網路搜尋 API、管理 secrets、
負擔 API 費用），先不做，只在文件裡留這個選項備查。
