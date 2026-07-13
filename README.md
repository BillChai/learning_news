# learning_news

每個平日早上自動蒐集的 IT / 後端相關新聞與資訊摘要，由排程 agent 產生並開 PR。

## 結構

- `digests/YYYY-MM-DD.md` — 每天的 15 則精選新聞/資訊摘要。
- `preferences.md` — 累積記錄使用者對過去內容的喜歡/不喜歡回饋，用來調整之後每次挑選的主題與來源權重。

## 流程

1. 排程任務每天平日早上執行，讀取 `preferences.md` 了解目前偏好。
2. 搜尋近期 IT / 後端相關新聞，挑選 15 則，寫成 `digests/YYYY-MM-DD.md`。
3. 開一個 PR（`digest-YYYY-MM-DD` -> `main`），內容為當天的摘要檔案。
4. 使用者在對話中給回饋後，偏好會被追加寫入 `preferences.md`（同一個 PR 或後續 commit）。
5. 使用者自行決定是否 merge 該 PR。
