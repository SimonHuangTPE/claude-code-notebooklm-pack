# Claude Code × NotebookLM 極限使用包

> 補完 [MathRuffian lazy-pack #01](https://github.com/mathruffian-dot/claude-code-lazy-packs/blob/master/01-%E9%80%A3%E6%8E%A5-NotebookLM.md) 沒提的 70% NotebookLM 能力。

## 內容

| 檔案 | 用途 |
|------|------|
| [`01-連接-NotebookLM-極限版.md`](./01-連接-NotebookLM-極限版.md) | 極限版懶人包文檔（給人看 / 教學）|
| [`skill/`](./skill/) | `notebooklm-master` Claude Code Skill（給 Claude 看 / 自然語言觸發）|

## 補了什麼（vs 原版懶人包）

| 功能 | 原版 | 本極限版 |
|------|------|---------|
| MCP tools 數量 | 9 種基本 | **29 個全列** |
| Pipeline（一句話自動化）| ❌ | ✅ `ingest-and-podcast` / `research-and-report` / `multi-format` |
| Batch（跨 notebook 批次）| ❌ | ✅ |
| Cross-notebook query | ❌ | ✅ |
| Tag 系統 | ❌ | ✅ 打造私人 RAG |
| Studio 視覺風格 | 只說「多種風格可選」 | ✅ **完整列 16 種**（影片 8 + 圖表 8）|
| Audio 4 種格式 | 只說 podcast | ✅ deep_dive / brief / **critique** / **debate** |
| 多 Profile（個人/工作）| ❌ | ✅ `nlm login --profile X` |
| 跨 Skill 鏈路 | ❌ | ✅ 5 個 Recipe（graphify / pptx / draw / draw-batch）|
| 配額管理建議 | ❌ | ✅ batch + pipeline 省 quota |

## 安裝極限 Skill

```bash
mkdir -p ~/.claude/skills
cd ~/.claude/skills
git clone https://github.com/SimonHuangTPE/claude-code-notebooklm-pack.git tmp
mv tmp/skill notebooklm-master
rm -rf tmp
```

開新 Claude Code 對話 → 對 Claude 說「跑 multi-format 把這 URL 變全套教材」即可。

## 先備

- 已跑過原版懶人包（裝好 `nlm` + `nlm login` + `nlm setup add claude-code`）
- 沒跑的話先去原版：https://github.com/mathruffian-dot/claude-code-lazy-packs

## 相關

- 我另一份 image gen skill：[claude-code-image-skills](https://github.com/SimonHuangTPE/claude-code-image-skills)（draw + draw-batch，gpt-image-2）

## License
MIT
