# File Map — P011 檔案地圖

核對基準：`8bbba51a93d6652535e74d746a3bb5b31f7c1406`。下表列出整理前檔案，大小來自 Git tree；新增管理文件另列於文末。

| 路徑 | 大小（bytes） | 類型 |
| --- | ---: | --- |
| [README.md](../README.md) | 44 | md |
| [index.html](../index.html) | 422954 | html |

## 管理入口

README.md、AGENTS.md、HANDOFF.md、PROJECT.json、CLAUDE.md、GEMINI.md、.github/copilot-instructions.md、docs/VERIFICATION.md。

## 檔內結構（行號索引，2026-10-18 Claude 依基準 `761375c` 整理）

只讀需要的段落；標 ⚠️ 的是超長單行資料，不要整行載入。

| 約略行號 | 內容 |
| ---: | --- |
| 1–9 | `<head>`（Google Fonts） |
| 10–153 | `<style>`（地底考古光塵風格、遺址色 `--site-*`） |
| 155–208 | HTML 介面（開始畫面、左右卡片欄、連線 SVG、結算畫面） |
| 209–523 | 主程式 `<script>` |
| 211 | 設定 `CFG` |
| 212 | ⚠️ `const IMG={…}`：6 張文物 JPEG（base64，約 390 KB，單行） |
| 215–236 | `ARTIFACTS`、`SITE_LIST`、`ROUNDS` |
| 237–255 | 音效 `AudioSynth` |
| 256–339 | 引擎：`initGame()`、生命、回合、卡片、鎖定判定 |
| 340–431 | 座標與貝茲曲線、拖曳＋點擊 |
| 432–490 | 配對判定 `evaluateMatch()` 與連線 |
| 491–523 | 回合切換與結算 |
