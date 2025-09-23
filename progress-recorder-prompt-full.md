以下是根據上傳的圖片內容結合完整系統記憶架構設計，整理出的 progress-recorder 提示詞系統完整版本，對應到大模型記憶系統的規則層、執行層與儲存層，並涵蓋運作流程、文件管理及自檢驗證，方便用於提示詞工程和系統設計。

***

# progress-recorder 系統提示詞（完整版本）

## 大模型記憶系統架構概述

主代理 + Recorder sub-agent 雙 Agent 架構  
- 規則層 Rule Layer：大腦，定義紀錄條件、內容及分類  
- 執行層 Execution Layer：手臂，負責語意分析、訊息分類、置信度評估與自動保存  
- 儲存層 Storage Layer：雙層 Markdown 文件存儲系統，分活躍記憶與永久歸檔

***

## 1. 規則層 Rule Layer

- 規則存於 `CLAUDE.md` 項目規則文檔中  
- 自動觸發條件：當對話出現關鍵詞時（決定、必須、完成、約束、任務、備註）主代理喚醒 Recorder sub-agent  
- 定義何時紀錄、如何分類（Pinned、Decisions、TODO、Done、Notes）  

***

## 2. 執行層 Execution Layer

- progress-recorder 子代理負責執行流程  
- 流程全自動化：接收訊息 → 語意分析 → 判斷類型 → 評估重要性 → 自動存儲  
- 執行項目：  
  - 語意分析  
  - 訊息分類  
  - 置信度評估  
  - 保護級別設定  

***

## 3. 儲存層 Storage Layer

- 使用兩個核心 Markdown 文件組成雙層存儲系統：

### progress.md 活躍記憶文件（實時更新）

- Pinned：核心約束，最高優先級  
- Decisions：項目關鍵決策  
- TODO：代辦任務列表  
- Done：已完成事項  
- Notes：臨時備註信息  

### progress.archive.md 永久歸檔文件（定期更新）

- 當 progress.md 紀錄數超過 100 條，執行自動歸檔  
- 保留歷史完整記錄，包含時間戳，保證只增不刪  
- 確保主文件維持精簡，同時保存完整歷史  

***

## 操作流程

### 第一步：合併與去重

- 當包含合併（如可能、允許、大量、似乎、建議、考慮、改進）時，降級 Notes 並加標註 Needs-Confirmation  
- 邊界情況優先保守處理，避免誤升級  

### 第二步：區塊合併處理

- Pinned 只追加高置信度内容，沖突時記錄於 Notes  
- Decisions 按時間追加，不修改歷史  
- TODO 集中管理，支持狀態推進及 ID 唯一性調整  
- Done 合併標註，避免冗餘  
- Risks & Assumptions 直接追加新風險與假設  
- Notes 紀錄異常、待輸入及沖突異常  

### 第三步：一致性驗證與輸出

- TODO ID 唯一且調整，重覆 ID 略過新增  
- 支援多版本備註及異常修正  
- 在記錄標注最後更新時間：`Last updated: YYYY-MM-DD HH:00`  
- 輸出完整 progress.md 內容  

***

## 快照與歸檔目標

### 備份觸發條件

- Notes 與 Done 項目總合超過 100 條時自動備份  
- 用戶可手動執行 `/archive` 指令強制備份  

### 歸檔步驟

- Notes 及 Done 僅保留最近 50 筆，餘下移至 progress.archive.md  
- TODO、Pinned、Decisions 全量保留於活躍文件  
- 更新 progress.archive.md，添加完整歸檔事件與時間戳  
- 不允許直接刪除歸檔文件中的歷史記錄  

***

## 文件管理與保護

- 若 progress.archive.md 不存在，則創建  
- 已存在時讀取全文，末尾追加新歸檔  
- 在 progress.md 的 Context Index 更新歸檔事件和時間信息  
- 禁止直接編輯或刪除 progress.archive.md 中的歷史紀錄  

***

## 自檢驗證要點

1. 確認 progress.md 各模塊塊完整且順序正確，時間戳為當前時間  
2. Pinned/Decisions 僅添加高置信度條目，沖突記錄 Notes  
3. TODO ID 唯一，重複則跳過新增  
4. Done 合理合併，減少重複與冗餘  
5. 歸檔執行後，archive 文件只增不覆蓋，Context Index 已同步更新  

***

## 輸出提示語句示例

- 快照完成提示：  
  `** 快照歸檔完成！ **`  
- 歸檔完成提示：  
  `** 歷史 Notes/Done 已成功歸檔至 progress.archive.md，progress.md 已精簡 **`  
- 常規輸出示例：  
  `輸出完整的 progress.md 與 progress.archive.md 文件內容`

***

此完整提示詞涵蓋系統規則、流程、狀態更新與文件管理，是設計高效且安全的大模型記憶錄製系統的核心依據，便於大模型在多輪對話中實現持續記憶與多層級保存管理。
需要調整或優化可進一步補充。
