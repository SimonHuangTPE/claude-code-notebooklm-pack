---
name: notebooklm-master
description: NotebookLM 極限使用 — 透過 nlm CLI / MCP 操控 Google NotebookLM。涵蓋 29 個 MCP tool、3 個內建 pipeline（ingest-and-podcast / research-and-report / multi-format）、16 種視覺風格、tag 系統批次操作、cross-notebook 比較查詢、4 種 audio 格式（deep_dive/brief/critique/debate）、以及跟其他 skill（graphify / pptx-generator / draw / draw-batch）的串接 recipe。當使用者要求「建 notebook」、「上傳 PDF / URL / YouTube」、「生 podcast / 簡報 / 心智圖 / 影片 / infographic / 測驗 / 閃卡」、「跨 notebook 比較」、「批次處理多本 notebook」、「自動化研究報告」時觸發。
---

# NotebookLM Master — 極限使用 nlm

> 補完 MathRuffian 懶人包 #01 沒提的 70% 能力。前提：已跑過 `nlm setup add claude-code` + `nlm login` 設好 MCP。

## 觸發情境總覽

| 客人說 | 對應動作 |
|--------|---------|
| 「建一個 notebook，把這個 PDF / URL 傳上去」 | `notebook_create` + `source_add` |
| 「幫我做 podcast / 音訊概覽」 | `studio_create kind=audio` |
| 「我要辯論版的 podcast」 | `studio_create kind=audio format=debate` |
| 「做教學簡報 / slides」 | `studio_create kind=slides` |
| 「做心智圖」 | `studio_create kind=mindmap` |
| 「做測驗 / 閃卡」 | `studio_create kind=quiz/flashcards` |
| 「做白板教學影片」 | `studio_create kind=video style=whiteboard` |
| 「做可愛動漫風 infographic」 | `studio_create kind=infographic style=kawaii` |
| 「給我所有 4wheels 相關 notebook 一起回答 X」 | `tag select` + `batch query` |
| 「比較這幾本 notebook 對 X 的看法」 | `cross_notebook_query` |
| 「跑 multi-format 一次出全部素材」 | `pipeline run multi-format` |
| 「自動找 source 寫一份 X 的研究報告」 | `pipeline run research-and-report` |
| 「URL → 全自動 podcast」 | `pipeline run ingest-and-podcast` |

---

## 29 個 MCP tool 完整對照

### 📔 Notebook 管理（9）
- `notebook_list` — 列所有 notebook
- `notebook_create` — 建新 notebook（必先有，才能加 source）
- `notebook_get` — 取單一 notebook 詳情
- `notebook_describe` — 取 metadata + source 清單
- `notebook_rename` — 改名
- `notebook_delete` — 刪除（**會問 confirm**）
- `notebook_share_status` — 看分享狀態
- `notebook_share_public` — 設公開連結
- `notebook_share_invite` — 邀人 email

### 📥 Source（資料來源，6）
- `source_add` — 加 source（URL / YouTube / PDF / Drive / 純文字 / 會議筆記）
- `source_list_drive` — 列出 Drive 可用文件
- `source_sync_drive` — 同步 Drive source（更新內容）
- `source_describe` — 取單一 source 的描述
- `source_get_content` — 抓 source 全文
- `source_rename` / `source_delete`

### 💬 對話（2）
- `notebook_query` — 在 notebook 內問問題（RAG）
- `chat_configure` — 設對話風格（goal / tone）

### 🔍 研究（3）
- `research_start` — 給主題，自動找 web/Drive source
- `research_status` — 查進度
- `research_import` — 把找到的 source 導入 notebook

### 🎨 Studio Artifact（5）
- `studio_create` — 生產素材（audio/video/slides/infographic/report/sheet/mindmap/quiz/flashcards）
- `studio_status` — 看生成進度
- `studio_revise` — 對已生成的東西下修改指令（slides 特有）
- `studio_delete`
- `download_artifact` / `export_artifact` — 下載到本機 / 匯出 Drive

### 📝 Notes 筆記（4）
- `note_create` / `note_list` / `note_update` / `note_delete`
- 用來在 notebook 裡留「自己的觀察」（會被 RAG 一起讀）

---

## 3 個內建 Pipeline（極限使用首選）

```bash
# 1. 一條龍：URL → notebook → audio + report + 公開連結
nlm pipeline run ingest-and-podcast --input-url "https://..." --notebook-name "X"

# 2. 主題研究 → 自動找 source → 寫報告
nlm pipeline run research-and-report --topic "BMW 320 G20 鋁圈推薦"

# 3. 從 URL 同時生成 audio/video/slides/mindmap/quiz/infographic 全套
nlm pipeline run multi-format --input-url "https://..." --notebook-name "X"
```

**對 Claude 說「跑 multi-format」就會走第 3 條，是給你最多素材的單次調用。**

---

## Studio 16 種視覺風格（懶人包完全沒提）

### 🎬 Video（8 種）
| style | 適用情境 |
|-------|---------|
| `classic` | 預設，平衡 |
| `whiteboard` | 教學（手繪白板感）|
| `kawaii` | 可愛、給小孩、社群輕鬆內容 |
| `anime` | 日漫風 |
| `watercolor` | 文藝、童書、品牌軟性 |
| `retro_print` | 60-70 年代復古海報感 |
| `heritage` | 歷史、文化、博物館感 |
| `paper_craft` | 紙工藝動畫，stop-motion 感 |

### 📊 Infographic（8 種）
| style | 適用情境 |
|-------|---------|
| `sketch_note` | 手繪筆記、教學 |
| `professional` | 商業簡報、年報 |
| `bento_grid` | iPhone 新品發表會風（區塊網格）|
| `editorial` | 雜誌風 |
| `instructional` | 操作步驟、How-To |
| `bricks` | 樂高方塊風 |
| `clay` | 黏土 3D 質感 |
| `scientific` | 科學論文圖表風 |

### 🎙️ Audio（4 種 format，不算視覺風格但同樣常被忽略）
| format | 內容 |
|--------|------|
| `deep_dive` | 預設，30+ 分鐘深度討論 |
| `brief` | 5-10 分鐘速覽 |
| `critique` | 主持人質疑 source、辯護式分析 |
| `debate` | 兩方對立辯論（最有戲劇性）|

---

## Tag 系統 + Batch（建你的私人 RAG）

### 設定 tag
```bash
nlm tag add --notebook-id N1 --tags "tire,inventory,2026"
nlm tag add --notebook-id N2 --tags "wheel,brand,2026"
nlm tag add --notebook-id N3 --tags "tire,supplier"
```

### 批次操作
```bash
# 跨所有 tag=tire 的 notebook 同時問問題
nlm batch query --tags "tire" --query "哪些供應商有 BC Forged？"

# 跨所有 tag=2026 的 notebook 同時生 audio
nlm batch create --tags "2026" --artifact-type audio --format brief
```

### Cross-notebook 直接點名比較
```bash
nlm cross query \
  --query "V1 vs V2 BOT 客戶轉換率比較" \
  --notebook-names "V1-LINE-logs, V2-Meta-logs"
```

---

## 多 Profile（個人 / 工作分帳號）

```bash
nlm login --profile work       # 公司 Google
nlm login --profile personal   # 個人 Google

# 後續用：
nlm --profile work notebook list
```

避免私人 notebook 跟工作 notebook 混在同一帳號。

---

## 跨 Skill 黃金 Recipe

### Recipe 1：知識 → 二腦
```
nlm studio create kind=mindmap → download → 存 ~/second-brain-vault/notes/
↓
graphify --update（自動把心智圖節點併進你的二腦圖）
```

### Recipe 2：簡報品牌化
```
nlm studio create kind=slides → export pptx → 存當前資料夾
↓
對 Claude 說：「用 4wheels 品牌套這份 pptx」
↓ 觸發 pptx-generator skill 套你的字型/配色
```

### Recipe 3：圖不滿意自製
```
nlm studio create kind=infographic style=bento_grid 
↓ 看了不滿意
draw skill 補做：「畫一張 X 用 cyberpunk 風」
```

### Recipe 4：圖系列 → 教材 source
```
draw-batch 出 8 張同主題不同視角圖
↓
全部上傳到 NotebookLM 當 source
↓
nlm studio create kind=video style=whiteboard
得到「跨圖總結教學影片」
```

### Recipe 5：研究報告自動化
```
nlm pipeline run research-and-report --topic "競品分析"
↓ 自動生 report
↓ 對 Claude 說「把這個 report 做成簡報 + 心智圖」
↓ 觸發 studio_create kind=slides + kind=mindmap
↓ 全套素材到手
```

---

## 配額管理

| 限制 | 說明 |
|------|------|
| Free tier 查詢 | ~50/day（chat + studio 共用）|
| Cookie 過期 | 2-4 週需重新 `nlm login` |
| Studio 並發 | 同一 notebook 同類型 1 個 active |

**省查詢策略：**
- 大批問題用 `batch query`（一次 N 本，但只算 N 次而不是 N²）
- pipeline 比連續手動省 ~30%（內部最佳化）
- mindmap / slides 一次到位，避免 `studio_revise` 不斷小改

---

## 預設輸出資料夾（沿用懶人包 #01 結構）

```
~/Documents/NotebookLM/
  ├── slides/        ← .pptx
  ├── infographics/  ← .png（依 style 命名）
  ├── audio/         ← .mp3
  ├── video/         ← .mp4
  ├── docs/          ← Google Docs export
  ├── sheets/        ← Google Sheets export
  ├── mindmaps/      ← .png + .md
  └── quizzes/       ← .json + .md
```

---

## 故障排查

| 錯誤 | 解法 |
|------|------|
| `nlm doctor` 顯示未認證 | `nlm login` 重新登入 |
| Cookie 過期 | 同上 |
| `studio_create` 卡住 | 用 `studio_status` 查；超過 5 分鐘 `studio_delete` 再重生 |
| 跨 notebook query 結果空 | 確認所有 notebook 都有 source 且 indexed 完成 |
| pipeline 中斷 | 用 `pipeline list` 找狀態，可重跑或從中斷點接續 |

---

## 延伸資源

- nlm CLI repo: https://github.com/jacob-bd/notebooklm-mcp-cli
- NotebookLM 官方：https://notebooklm.google.com
- 原始懶人包 #01: https://github.com/mathruffian-dot/claude-code-lazy-packs/blob/master/01-連接-NotebookLM.md
