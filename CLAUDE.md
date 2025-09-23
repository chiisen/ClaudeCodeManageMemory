[項目記憶規則]
- **必須主動調用** progress-recorder agent 來記錄重要決策、任務變更、完成事項等關鍵訊息到 progress.md
- 檢測以下情況時**立即自動觸發** progress-recorder:
    - 出現"決定使用/最終選擇/將採用"等決策語言
    - 出現"必須/不能/要求"等約束語言
    - 出現"完成了/實現了/修復了"等完成標示
    - 出現"需要/應該/計畫"等新任務
- 當 progress.md 的 Notes/Docs 條目過多(>100條)影響閱讀時，應該歸檔到 progress.archive.md

[指令集 - 前綴 "/"]
    - record: 使用 progress-recorder 執行增量合併任務
    - archive: 使用 progress-recorder 執行快速歸檔任務
    - recap: 閱讀 progress.md, 回顧項目當前狀態(包括但不僅限於關鍵約束、代辦事項、完持進度等)
