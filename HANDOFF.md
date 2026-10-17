# Handoff — P011 第七關・時光對對碰

- 更新時間：2026-10-18T03:25+08:00
- 更新者：Claude（逐檔讀原始碼，撰寫專案交接）
- 倉庫：https://github.com/jackeytw886/time_match
- 分支：main
- 本次基準 SHA：`761375cc689050ebff6d95dc8dab72f40c7a0406`（Codex 管理整理提交）
- 核對層級：已讀全部原始碼；已執行語法檢查與無頭瀏覽器冒煙測試（見 docs/VERIFICATION.md）；**未在展場觸控螢幕實測**。

## 1. 這是什麼

宜蘭考古主題闖關活動的「第七關」：觸控連連看遊戲。左邊是六件宜蘭史前文物照片，玩家用手指把文物「拉線」連到右邊正確的答案，共三段：

1. **名稱**（例：幾何印紋陶罐）
2. **代表意義**（例：良好的製陶工藝）
3. **發現遺址**（淇武蘭、Blihun 漢本、丸山三處，一個遺址可接多件文物）

錯 5 次「探測超載」失敗；三段全對獲得稱號「考古小博士」，在 120 秒內完成另有「特獎」（畫面圖示是冰淇淋）。據作者先前說明，此遊戲用於宜蘭博物館的 65 吋觸控電視。

- 線上網址：https://jackeytw886.github.io/time_match/ （GitHub Pages）

## 2. 檔案

只有 `index.html`（約 423 KB、526 行）。其中**第 212 行**是 `const IMG = {...}`，內嵌 6 張文物 JPEG（base64，約 390 KB），**不要整行讀入**；行號索引見 docs/FILE-MAP.md。

外部依賴只有 Google Fonts（Noto Sans TC、Noto Serif TC、Orbitron…），離線時會改用系統字型，功能不受影響。

## 3. 程式怎麼運作

| 部分 | 位置／名稱 | 說明 |
| --- | --- | --- |
| 設定 | `CFG = { TITLE:'考古小博士', MAX_WRONG:5, BONUS_SEC:120 }` | 稱號、容錯次數、特獎時限 |
| 文物 | `ARTIFACTS`（id、name、meaning、siteType、siteName、img） | 6 件：pot、fish（淇武蘭）、bone、bronze（漢本）、jade、curl（丸山） |
| 遺址 | `SITE_LIST` | kiwulan／hanben／wanshan，各有代表色（CSS 變數 `--site-*`） |
| 回合 | `ROUNDS` | `name` → `meaning` → `site`，每段都要配完 6 件才進下一段 |
| 狀態 | `state`（round、wrongAttempts、secondsPassed、matches…） | `initGame()` 建立，計時每秒 +1（正數計時，不是倒數） |
| 操作 | `registerDragAndDrop()` | 拖曳拉線或「先點左再點右」都可以；連線用 SVG 貝茲曲線（`drawBezierPath()`） |
| 鎖定 | `isLocked(side, id)` | 名稱、意義是一對一；遺址節點可接多條線 |
| 判定 | `evaluateMatch(leftId, rightId)` | 對：金色光線＋音效；錯：紅線閃一下、卡片搖晃、扣一格生命 |
| 結算 | `finishGame()`、`endGameFail()` | 顯示稱號、用時、特獎是否達成 |
| 音效 | `AudioSynth`（Web Audio 合成） | 成功、錯誤、勝利 |

## 4. 換內容（最常見的修改）

- 換文物：改 `ARTIFACTS`，圖片放進 `IMG`（建議先壓成小尺寸 JPEG/WebP 再轉 base64，維持單檔）。
- 換遺址：改 `SITE_LIST`、`ARTIFACTS[].siteType` 與 CSS 的 `--site-*` 顏色變數。
- 改難度：`CFG.MAX_WRONG`、`CFG.BONUS_SEC`。

## 5. 已知問題與待辦

- 頁面沒有閒置自動重置；展場若無人看管，結算畫面會一直停著，需要人按「再玩一次」。
- 文物說明文字（意義、遺址名稱）的考據來源未記錄在倉庫。
- 未記錄展場使用的瀏覽器、螢幕解析度與觸控設定。

## 6. 測試方式

直接開 `index.html`，按開始。自動化測試可呼叫 `startGame()`，讀 `state.round`、`state.matches`、`state.wrongAttempts`；卡片 id 為 `card-left-<文物id>`、`card-right-<答案id>`。

## 7. 版本沿革

- 2026-06-10：最後一次程式更新（經 EZPage 部署）；先前在對話中迭代過 v11～v15（修正 iframe 注入、判定邏輯與結算畫面）。
- 2026-10-17：Codex 管理整理（代號 P011、AI 入口文件）。
- 2026-10-18：Claude 補寫本交接。

## 版本定位

基準 SHA 定位本次文件修改前的程式；本次文件提交由此檔所在的 Git 提交與倉庫歷史定位，不預填同一提交自己的 SHA。
