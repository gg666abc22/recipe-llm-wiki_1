# 食譜 LLM Wiki 維護規範（方案 A）

本知識庫採用 **「Markdown 知識庫 ＋ 純前端互動網頁」** 的雙層架構。所有資料由 AI Agent 依照本規範維護，網頁端零 API Key 即可直接運行。

## 1. 目錄結構
- `raw/`：原始食譜資料（JSON / 文字備份），**唯讀不可修改**。
- `wiki/recipes/`：標準化食譜頁面（含 YAML Frontmatter，供腳本解析）。
- `wiki/ingredients/`：食材百科頁面（保存方式、處理技巧、可替代食材、反向連結）。
- `wiki/techniques/`：烹飪技法頁面（從食譜中萃取的關鍵手法與原理）。
- `wiki/index.md`：全站內容總索引。
- `wiki/log.md`：每次新增、更新或檢查（Lint）的異動紀錄。
- `web/`：純前端互動網頁（`index.html`、`data.js`）。
- `scripts/build_data.py`：將 `wiki/` 的 Markdown 編譯為 `web/data.js`。

## 2. 標準作業流程
### A. 新增食譜（Ingest）
1. 將原始資料存入 `raw/<id>.json`（或 `.md`）。
2. 在 `wiki/recipes/` 建立標準化食譜檔，必須包含結構化 YAML Frontmatter（`id`, `title`, `servings`, `prep_time`, `tags`, `source_url`, `ingredients` 陣列含 `name`, `amount`, `unit`, `note`, `scalable`）。
3. 檢查 `wiki/ingredients/` 與 `wiki/techniques/`：若出現新食材或值得記錄的技法，建立或更新對應頁面，並加上 `[[雙向連結]]`。
4. 更新 `wiki/index.md` 與 `wiki/log.md`。
5. 執行 `python scripts/build_data.py` 同步更新 `web/data.js`。

### B. 查詢與問答（Query）
- 優先讀取 `wiki/index.md` 定位相關食譜、食材或技法頁面，再綜合回答。

### C. 一致性檢查（Lint）
- 定期檢查：單位名稱是否統一（如「顆」、「大匙」、「g」、「cc」）、是否有孤兒頁面或遺漏的雙向連結。
