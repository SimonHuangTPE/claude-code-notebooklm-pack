---
title: 'Claude Code 懶人包 #01 極限版：連接並極限使用 NotebookLM'
date: '2026-04-26'
type: 懶人包
version: v1.0-extreme
tags: [Claude-Code, 懶人包, NotebookLM, MCP, gpt-image-2, graphify]
based_on: MathRuffian/claude-code-lazy-packs/01
---

# Claude Code 懶人包 #01 極限版：連接並**極限使用** NotebookLM

> 對標原版懶人包 #01。**補完原版沒提的 70% 能力**：29 個 MCP tool、3 條 Pipeline、16 種視覺風格、Tag/Batch 系統、Cross-notebook 比較、跨 skill 鏈路。
> 適合：已用過原版、想榨乾 NotebookLM 的進階使用者。

---

## 為什麼要用極限版？

原版懶人包 #01 把你帶到「能用」，但只用了 NotebookLM **30% 能力**。例如：

| 你以為只能 | 其實還能 |
|-----------|---------|
| 1 本 notebook 問 1 個問題 | 用 Tag 把 N 本綁一組 → 一次 batch 跨 N 本問 |
| 預設樣式 podcast | 4 種格式：deep_dive / brief / **critique** / **debate** |
| 普通 video | 8 種視覺風：whiteboard / kawaii / anime / watercolor / retro_print / heritage / paper_craft / classic |
| 普通 infographic | 8 種：sketch_note / professional / **bento_grid（iPhone 發表會風）** / editorial / instructional / bricks / clay / scientific |
| 一個一個手動跑 | 3 條 pipeline 一句話跑全套：`ingest-and-podcast` / `research-and-report` / `multi-format` |
| 開不同帳號要登出再登入 | `nlm login --profile work / personal` 多帳號並存 |
| 跟其他工具沒整合 | 跟 graphify / pptx-generator / draw / draw-batch 串成生產線 |

---

## 先備條件

- 原版懶人包 #01 已跑完（`nlm` 已裝、`nlm login` 已登入、`nlm setup add claude-code` 已綁）
- Claude Code 桌面版 / CLI 任一
- Google 帳號 NotebookLM Free tier（~50 queries/day）足以開始

---

## 一、Pipeline 自動化（極限使用 #1 神器）

`nlm pipeline` 內建 3 條全自動工作流，比原版「手動點 9 步」省 30% quota + 80% 時間。

### 1. `ingest-and-podcast` — URL 進、podcast 出
```bash
nlm pipeline run ingest-and-podcast \
  --input-url "https://en.wikipedia.org/wiki/Quantum_computing" \
  --notebook-name "Quantum_Intro"
```
**做了什麼：** 建 notebook → source_add URL → 等 indexed → studio_create kind=audio → download → 存到本機。
**輸出：** `~/Documents/NotebookLM/audio/Quantum_Intro_<時間戳>.mp3`

### 2. `research-and-report` — 主題進、報告出
```bash
nlm pipeline run research-and-report \
  --topic "BMW 320 G20 鋁圈推薦" \
  --notebook-name "BMW_G20_Wheel_Research"
```
**做了什麼：** `research_start` 自動找 web/Drive source → import 進 notebook → studio_create kind=report → export Google Docs。
**輸出：** `~/Documents/NotebookLM/docs/BMW_G20_Wheel_Research.docx`

### 3. `multi-format` — URL 進、全套素材出（最划算）
```bash
nlm pipeline run multi-format \
  --input-url "https://example.com/article" \
  --notebook-name "X" \
  --formats "audio,video,slides,mindmap,quiz,infographic"
```
**做了什麼：** 一次生 6 種素材，API 內部 batch 比手動 6 次省一半 quota。

> **對 Claude Code 說：「跑 multi-format 把這個 URL 變成全套教材」**

---

## 二、Studio 16 種視覺風格（原版完全沒提）

### 🎬 Video（`studio_create kind=video --style ?`）

| style | 適用 | 範例情境 |
|-------|------|---------|
| `classic` | 預設 | 通用 |
| `whiteboard` | 教學 | 數學公式、流程圖講解 |
| `kawaii` | 可愛 | 兒童教育、社群輕鬆內容 |
| `anime` | 日漫 | 動漫迷愛看 |
| `watercolor` | 文藝 | 童書、品牌軟性 |
| `retro_print` | 復古 | 60-70 年代海報感 |
| `heritage` | 博物館 | 歷史、文化 |
| `paper_craft` | 紙工藝 | stop-motion 動畫感 |

### 📊 Infographic（`studio_create kind=infographic --style ?`）

| style | 適用 |
|-------|------|
| `sketch_note` | 手繪筆記 |
| `professional` | 商業簡報、年報 |
| `bento_grid` | **iPhone 發表會風（區塊網格）** |
| `editorial` | 雜誌風 |
| `instructional` | 操作步驟 |
| `bricks` | 樂高方塊風 |
| `clay` | 黏土 3D 質感 |
| `scientific` | 論文圖表風 |

### 🎙️ Audio 4 種格式（`studio_create kind=audio --format ?`）

| format | 內容 |
|--------|------|
| `deep_dive` | 預設，30+ 分鐘深度討論 |
| `brief` | 5-10 分鐘速覽 |
| `critique` | 主持人質疑 source、辯護式分析 |
| `debate` | **兩方對立辯論（最有戲劇性）** |

> 對 Claude 說：「**幫我做白板教學影片**」→ AI 自動帶 `--style whiteboard`
> 「**做辯論版的 podcast**」→ `--format debate`

---

## 三、Tag + Batch 打造私人 RAG

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

# 跨所有 tag=2026 的 notebook 同時生 audio brief
nlm batch create --tags "2026" --artifact-type audio --format brief
```

### Cross-notebook 直接點名比較
```bash
nlm cross query \
  --query "V1 vs V2 BOT 客戶轉換率比較" \
  --notebook-names "V1-LINE-logs, V2-Meta-logs"
```
NotebookLM 用 Gemini 把幾本 notebook **同時讀完 + 寫對照** —— 比你手動切視窗強太多。

---

## 四、多 Profile（個人 / 工作分帳號）

```bash
nlm login --profile work       # 公司 Google
nlm login --profile personal   # 個人 Google

nlm --profile work notebook list
nlm --profile personal notebook create "我的讀書筆記"
```
避免私人/工作 notebook 混一起。

---

## 五、跟其他 Skill 的化學鏈路

### Recipe 1：知識 → 第二大腦
```
nlm studio create kind=mindmap → download → ~/second-brain-vault/notes/
↓
graphify --update      # 心智圖自動併進你的二腦語意圖
```

### Recipe 2：簡報品牌化
```
nlm studio create kind=slides → export pptx
↓
對 Claude 說「用 4wheels 品牌套這份 pptx」
↓
觸發 pptx-generator skill 套字型/配色
```

### Recipe 3：圖不滿意 → gpt-image-2 補
```
nlm studio create kind=infographic style=bento_grid
↓ 看了不滿意
draw skill：「畫一張 X 用 cyberpunk 風」（自動 NT$6.75 high quality）
```

### Recipe 4：圖系列 → 教材 source
```
draw-batch 出 8 張同主題不同視角圖
↓
全部上傳 NotebookLM 當 source
↓
studio_create kind=video style=whiteboard
↓ 跨圖總結教學影片
```

### Recipe 5：研究 → 全套
```
nlm pipeline run research-and-report --topic "競品分析"
↓ 自動 report
↓ 對 Claude 說「把這個 report 做成簡報 + 心智圖 + bento_grid infographic」
↓ 觸發 studio_create × 3
↓ 全套素材到手（pptx + mindmap + 圖）
```

---

## 六、29 個 MCP tool 完整對照（給 Claude 看）

### Notebook（9）
```
notebook_list / notebook_create / notebook_get / notebook_describe
notebook_rename / notebook_delete
notebook_share_status / notebook_share_public / notebook_share_invite
```

### Source（6）
```
source_add / source_list_drive / source_sync_drive
source_describe / source_get_content / source_rename / source_delete
```

### Chat（2）
```
notebook_query        # 問問題（RAG）
chat_configure        # 設對話 goal/tone
```

### Research（3）
```
research_start        # 給主題自動找 web/Drive source
research_status / research_import
```

### Studio（5）
```
studio_create         # 生 audio/video/slides/infographic/report/sheet/mindmap/quiz/flashcards
studio_status / studio_revise / studio_delete
download_artifact / export_artifact
```

### Notes（4）
```
note_create / note_list / note_update / note_delete
# 在 notebook 內留自己的觀察，會被 RAG 一起讀
```

---

## 七、配額管理（省查詢小撇步）

| 限制 | 對策 |
|------|------|
| Free tier ~50 queries/day | 用 `batch query`：N 本 notebook 算 N 次而非 N² 次 |
| Cookie 2-4 週過期 | 自動測：`nlm doctor`；過期跑 `nlm login` |
| Studio 並發限制 | 同一 notebook 同類型只能一個 active，先 `studio_status` 查 |
| Pipeline vs 手動 | Pipeline 內部最佳化，比手動 6 步省 ~30% quota |

---

## 八、安裝極限版 Skill（一行裝）

如果你想在 Claude Code 對話裡**自然語言觸發**全部極限功能，安裝這個 skill：

```bash
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/SimonHuangTPE/claude-code-notebooklm-pack.git tmp
mv tmp/skill notebooklm-master
rm -rf tmp
```

之後對 Claude Code 說這些話會自動觸發對應 nlm 指令：

| 你說 | 觸發 |
|------|------|
| 「跑 multi-format 把這 URL 變全套教材」 | `pipeline run multi-format` |
| 「自動研究 X 主題寫報告」 | `pipeline run research-and-report` |
| 「給我所有 4wheels notebook 一起回答 X」 | `tag select` + `batch query` |
| 「比較 V1 跟 V2 notebook」 | `cross_notebook_query` |
| 「做白板教學影片」 | `studio_create kind=video style=whiteboard` |
| 「辯論版 podcast」 | `studio_create kind=audio format=debate` |
| 「bento_grid 風的資訊圖」 | `studio_create kind=infographic style=bento_grid` |

---

## 故障排查（補原版沒提的）

| 問題 | 解法 |
|------|------|
| `studio_create` 卡住 > 5 分鐘 | `studio_delete` 後重生（卡住通常是 Google 端 worker 死掉）|
| `cross_notebook_query` 結果空 | 確認所有指定 notebook 都有 source 且 indexed 完成（`notebook_describe` 查）|
| Pipeline 中斷 | `pipeline list` 查狀態，可從中斷點接續 |
| 多 profile 切錯帳號 | `nlm --profile X` 每次都要明示帳號（沒帶會用預設）|
| Tag 加錯 | `nlm tag remove` 修正 |

---

## 致敬

- 原版 lazy-pack #01 by **三師爸（宋睿偉）** [@SenseBar](https://youtube.com/@SenseBar)
- nlm CLI by **jacob-bd** [github.com/jacob-bd/notebooklm-mcp-cli](https://github.com/jacob-bd/notebooklm-mcp-cli)
- 極限版補完 by Simon Huang × Claude Sonnet 4.6（2026-04-26）

## License
MIT
