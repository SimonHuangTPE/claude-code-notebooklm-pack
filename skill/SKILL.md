---
name: notebooklm-recipes
description: NotebookLM 跨 skill 串接 recipe — 把 nlm 產的素材（mindmap / slides / infographic / audio）跟其他 skill（graphify / pptx-generator / draw / draw-batch）串成生產線。當使用者完成 nlm 操作後想做後續加工（mindmap → graphify、slides → 套品牌、infographic 不夠看 → 補製、用生成圖當 source 等）時觸發。**前置依賴 nlm-skill（已自動裝）。本 skill 只負責 cross-skill 鏈路，不重複 nlm 基礎用法。**
---

# NotebookLM 跨 Skill 串接 Recipe

> **這份 skill 是 `nlm-skill` 的補充。** 基本 nlm 操作（建 notebook / 加 source / studio_create / 16 風格 / pipeline / batch / cross-notebook 等）請看官方 `nlm-skill`。
> 本 skill 只解決「**nlm 產出的東西要怎麼接下一個 skill 做加工**」。

## 5 個黃金 Recipe

### Recipe 1：NotebookLM mindmap → graphify 二腦
```
nlm mindmap create <notebook> --confirm
↓ 等 status completed
nlm download mind-map <notebook> --output ~/second-brain-vault/notes/<topic>.txt
↓
graphify --update（自動把心智圖節點併進語意圖）
```
**價值**：NotebookLM 的 RAG 結果 → 你個人二腦永久知識庫

### Recipe 2：NotebookLM slides → pptx-generator 套品牌
```
nlm slides create <notebook> --format detailed_deck --confirm
↓ 等 status completed  
nlm download slide-deck <notebook> --format pptx --output ./slides.pptx
↓
對 Claude 說：「用 4wheels 品牌套這份 pptx」
↓ 觸發 pptx-generator skill 套字型/配色/logo
```
**價值**：NotebookLM 內容好但版面通用 → 你的品牌風

### Recipe 3：NotebookLM infographic 不夠看 → draw 補製
```
nlm infographic create <notebook> --orientation landscape --detail detailed --confirm
↓ 看了不滿意（NotebookLM 8 種風偏教學感）
draw skill：「畫一張 X 用 cyberpunk / studio_product / vintage_poster 風」
↓ gpt-image-2 補出更獨特的視覺
```
**價值**：NotebookLM infographic 8 種偏教學風 + draw 14 種偏設計風 = 涵蓋更廣

### Recipe 4：draw-batch 出系列圖 → 全部當 source
```
draw-batch "BMW 320 G20 鋁圈 4 個視角" --vary angle --n 4 --style studio_product
↓ 出 4 張到 ./generated/batch_xxx/
↓
nlm source add <notebook> --file ./generated/batch_xxx/01_xxx.jpeg
nlm source add <notebook> --file ./generated/batch_xxx/02_xxx.jpeg
（重複 4 張）
↓
nlm studio create kind=video style=whiteboard
↓ NotebookLM 跨圖總結 → 教學影片
```
**價值**：你自製圖庫變 NotebookLM 的素材池

### Recipe 5：研究 → 全套教材
```
nlm pipeline run <notebook> research-and-report --topic "BMW 320 G20 鋁圈推薦"
↓ 自動 web 找 source + 寫 report
↓
對 Claude 說：「把這 report 同時做成 slides + mindmap + bento_grid infographic」
↓ 觸發 nlm slides + nlm mindmap + nlm infographic 三條
↓
（要再深入）對 Claude 說：「mindmap 抽進二腦」
↓ Recipe 1 接力
```
**價值**：一條 topic → 全套素材 + 永久知識庫

---

## 觸發詞對應

| 客人說 | 走 Recipe |
|--------|---------|
| 「mindmap 進二腦 / 抽到 graphify」 | 1 |
| 「這份 slides 套品牌 / 套 4wheels 風」 | 2 |
| 「infographic 不夠 / 換更酷風格」 | 3 |
| 「用我剛畫的圖當 source」 | 4 |
| 「研究 X 主題 + 全套素材 + 進腦」 | 5 |

---

## 已知限制

- nlm download 路徑要絕對路徑，不然存到 `~`
- 上傳 source 等 indexed 才能 query（用 `nlm notebook describe` 確認）
- pptx 套品牌需要先有 brand.json（用 brand-voice-generator skill 建）
- graphify --update 需在 `~/graphify-memory/` 等對應 repo 內跑

---

## 不在這裡的東西（去 nlm-skill 找）

- 怎麼建 notebook / 加 source / login
- 16 種視覺風格參數
- pipeline / batch / cross-notebook 用法
- alias / multi-profile / 配額管理

那些都是基礎，官方 nlm-skill 有完整文件。
