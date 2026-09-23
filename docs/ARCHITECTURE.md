# FateGrid｜工程架構與雛型接收 v0.1
更新：2026-09-23。範圍僅限 FateGrid，不與 cheng-tea-shop / ERP 或 ChickNest 的程式、任務或 Git 歷史混用。

## 1. Claude 交接包狀態
原始輸入：bazi-trigger-engine-handoff.zip，含 HANDOFF.md（4750 bytes）及 bazi-trigger-engine.html（50347 bytes）。
單頁 HTML/CSS/Vanilla JS，直接瀏覽器開啟；尚無後端或持久化。
已有：近似四柱／大運、干與地支本氣十神、干合與支合沖刑害、歲運、九宮與固定子山午向、流年星、五級事件標籤、事件簿與遊走事件鏈。
已知不足：節氣以日期近似、起運估算、未處理次氣／真太陽時、未提供任意 24 山／流月飛星、無存檔、單角色、未匯出。
原始包視為 **Prototype v0.1 / 未驗證**；不要把任何自訂吉凶分數當實證。

## 2. 目標分層（規格，不表示已實作）
- adapters/calendar: 曆法／節氣服務；輸出附時區、版本與精度。
- core/chart: 四柱與十神、藏干、旺衰資料；純函式。
- core/relations: 分開「關係檢出」、「力量評估」、「成化判定」，留下證據鏈。
- core/time: 大運／歲運／月日與時層；依規則版本回算。
- core/space: 屋宅九宮、坐向、運／山／向／年／月星與環境事件。
- engine/rules: 可配置規則、權重、衝突解決與未知狀態；規則不得暗中覆蓋。
- engine/triggers: 接收時空狀態，輸出候選事件及來源與不確定性。
- narrative: 將候選事件轉化為故事，不回寫成推演事實。
- persistence: JSON schema_version、遷移、匯入匯出；未來才考慮資料庫。
- ui: 目前 HTML 雛型可保留視覺與互動，但不得作唯一真值來源。

## 3. 單一輸出契約
每個結果必含 input_id, engine_version, rule_set_version, calendar_version, timestamp, facts[], hypotheses[], conflicts[], unknowns[], trigger_candidates[], narrative_only[]。
每條觸發需含來源柱／運歲／宮位、關係類型、計算明細及啟用規則 ID；支援重播與人工檢核。

## 4. 測試閘門
T0 對照至少兩份獨立可信萬年曆／節氣資料；邊界日期、跨時區、子時、缺時辰測試。
T1 十神、藏干、關係檢出表格測試；合而不化與沖後剩餘力量列為反例。
T2 自訂權重可重播；缺輸入及規則衝突時只輸出 UNKNOWN，不臆測。
T3 24 山、元運、流年／流月盤先做獨立範例核對，再接入敘事。
T4 JSON round-trip、版本遷移、不同角色／日期不串資料。
任何未通過項目標記 BLOCKED；不以畫面漂亮代替通過。

## 5. 不混線與版本控制
Codex 目前專注茶行網站／ERP 到可交付，不指派 FateGrid；Claude 交接包先封存。
FateGrid 的需求、決策、測試與提交獨立；不可覆寫原包。日後再安排 Codex 時，先讀本文件和 KNOWLEDGE_BASE.md，列差異後再動手。
