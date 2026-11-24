# AMPM Agent Operations Governance Framework (AOGF)

<img src="logo.jpeg" alt="AM&PM AIOPS CO., LTD. logo" width="180">

**一套可重用的 AI 代理營運治理文件框架。** AOGF 提供工作單、品質閘門、核准、停止條件與執行收據等治理素材，供各組織依自身環境評估及調整。

> **「AI 做了什麼改動？是否留下可覆核的紀錄？」**
>
> **版本：AOGF 治理文件框架 v0.1。** 本框架不是代理執行平台，也不構成第三方認證或商業服務承諾。

## 目的與範圍

當代理工作流程會讀取資料、呼叫工具或造成外部影響時，明確的任務範圍、責任人、核准條件與結果紀錄，有助於團隊檢視執行過程。AOGF 將這些治理要素整理成可自行採用的文件與空白模板，不綁定特定模型或供應商。

## 參考流程

`工作單 → 權限與品質檢查 → 受限執行及必要人工核准 → 結果收據 → 覆核`

這是治理參考流程，不代表本儲存庫會執行部署或強制任何控制。實際執行與防護能力取決於採用者的系統、權限設定及作業程序。

## 框架內容

- **工作單模板**：記錄目的、範圍、負責人、允許工具、停止條件及驗收方式。
- **品質閘門**：檢查任務完整性、身分與授權、資料處理、工具限制及結果驗收。
- **核准與停止指引**：說明高影響動作、越界要求、失敗及撤銷時的處理考量。
- **收據與稽核指引**：列出可供覆核的紀錄欄位與完整性限制。SHA-256 雜湊不是數位簽章；附加式紀錄也不等同絕對防竄改。

這些文件提供治理結構，不保證能防止金鑰外洩、未授權變更或其他事故；相關控制必須在實際執行環境中設計、實作及驗證。

## AM&PM 營運案例

AM&PM 網站 [ampm-aiops.com](https://ampm-aiops.com/) 是目前運作中的 MVP／使用案例；其[即時透明櫥窗](https://ampm-aiops.com/transparency/live/)提供公開的營運流程與相關證據。此案例展示 AM&PM 自身的運作情況，不代表第三方認證，也不表示 AOGF 的每項控制都已獨立驗證。AOGF 儲存庫是可重用的治理文件框架，不是該網站的生產執行程式。

## 商業模式提案

[商業模式提案](COMMERCIAL-MODEL.md)僅供評估，尚非目前提供的商品或服務；沒有核准價格、SLA 或效果保證。

## 文件與模板

- [架構與治理原則](docs/architecture.md)
- [工作單生命週期](docs/work-order-lifecycle.md)
- [品質閘門](docs/quality-gates.md)
- [收據與稽核紀錄](docs/receipts-and-audit.md)
- [工作單空白模板](examples/work-order.example.yaml)
- [收據空白模板](examples/receipt.example.json)
- [安全性政策](SECURITY.md)
- [貢獻指南](CONTRIBUTING.md)

## 授權與安全

- 程式碼（目前尚未包含）：Apache-2.0，詳見 [LICENSE-CODE.md](LICENSE-CODE.md)。
- 文件、空白模板及商業模式提案：CC BY-SA 4.0，詳見 [LICENSE-DOCS.md](LICENSE-DOCS.md)。
- 品牌名稱、商標、標誌及第三方素材不因上述文件授權而取得使用權。

安全性問題處理方式請參閱 [SECURITY.md](SECURITY.md)。

---

黑曜智體有限公司 · AM&PM AIOPS CO., LTD.
