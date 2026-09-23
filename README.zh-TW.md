# Agent Governance Bootstrap

[English](./README.md) | [繁體中文](./README.zh-TW.md)

這是一套可重複使用、以 **Discovery-first（先盤點、再設計）** 為核心的 Prompt，用來在 AI Coding Agent 對既有軟體 Repository 進行實質修改前，建立一層 **Minimum Viable Agent Governance（最小可用 Agent 治理）**。

> 目標不是讓 Agent 記住更多。  
> 目標是讓 Repository 告訴 Agent：去哪裡取得正確背景、被允許做什麼，以及如何證明自己做對了。

## 為什麼需要這套方法

當 Coding Agent 開始能處理長時間、多步驟任務後，真正的問題逐漸從「Prompt 怎麼寫得更好」轉向：

- **Context（背景）** — Agent 現在需要知道什麼
- **Authority（權限）** — Agent 被允許做什麼
- **Evidence（證據）** — 某個事實或決策的依據是什麼
- **Verification（驗證）** — Agent 如何證明修改是正確的

即使模型能力很強，只要 Repository 狀態、歷史決策、權限邊界或驗證規則不清楚，仍然可能失敗。

這個專案提供一個 bootstrap 流程：先讓 Agent 讀懂 Repository、找出治理缺口、提出最小治理方案，並在任何可能影響正式環境的動作前停下來等待人工確認。

## 核心流程

```text
Repository Discovery
        ↓
Governance Gap Analysis
        ↓
Minimum Governance Design
        ↓
Human Approval Gate
        ↓
Implementation
        ↓
Verification
        ↓
Handoff / Complete
```

## 快速開始

1. 用 Coding Agent 開啟目標 Repository。
2. 將 [GOVERNANCE_BOOTSTRAP_PROMPT.zh-TW.md](./GOVERNANCE_BOOTSTRAP_PROMPT.zh-TW.md) 貼給 Agent。
3. 先執行 **Phase 1：只盤點與提出方案**。
4. 審查 Agent 建議的治理架構。
5. 只批准這個 Repository 真正需要的部分。
6. 再讓 Agent 實作核准後的最小集合。
7. 驗證新治理文件是否真的符合 Repository 現況。

這份 Prompt 刻意採取保守預設：第一輪不得 deploy、不得 migration、不得修改 production data、不得 commit、不得 push。

## 可能產生的治理資產

依 Repository 實際狀況，最小集合可能包含：

- `AGENTS.md` — 工作規則、權限邊界、導航與驗證要求
- `PROJECT_STATE.md` — 目前狀態、進行中工作、已知問題
- `DECISIONS.md` — 已確認的重要設計決策與原因
- `REPO_FACTS.md` — 已驗證的 Repository 事實與 source-of-truth
- Handoff template
- 可重複使用的 Skills

**不要機械式全部建立。** 詳見 [MINIMUM_GOVERNANCE.zh-TW.md](./MINIMUM_GOVERNANCE.zh-TW.md)。

## 設計原則

### 1. 先理解，再規範
Agent 必須先檢查真實 Repository，再提出治理方案。

### 2. 最小可用治理
每份文件都必須有清楚且獨立的責任，也必須有維護方式。

### 3. 高風險邊界需要人工核准
具有破壞性或會影響正式環境的操作，需要明確人工批准。

### 4. 證據優先於信心
「看起來沒問題」不是驗證。優先使用 tests、build、API response、query、smoke test、diff inspection 等可重複證據。

### 5. 不確定就保持 UNKNOWN
Repository 沒有足夠證據時，標記為 `UNKNOWN`，不要自行補答案。

### 6. Repository 優先於 Session 記憶
長期需要保存的專案知識，應進入可版本控制的 source of truth，而不是只存在對話裡。

## 建議權限模型

| 層級 | 常見操作 | 預設行為 |
|---|---|---|
| GREEN | 讀取、搜尋、分析、本地測試、bounded edit、文件更新 | Agent 可自行進行 |
| YELLOW | dependency、auth、共用 API contract、CI/CD、大型 refactor | 先揭露風險並縮小範圍 |
| RED | production deploy、破壞性 migration、production data mutation、credential/security 變更 | 必須人工明確批准 |

這只是起點，不是所有 Repository 都適用的固定規則。

## 語言

- English: [README.md](./README.md)
- 繁體中文: [README.zh-TW.md](./README.zh-TW.md)

繁體中文文件：

- [Governance Bootstrap Prompt](./GOVERNANCE_BOOTSTRAP_PROMPT.zh-TW.md)
- [Minimum Governance](./MINIMUM_GOVERNANCE.zh-TW.md)
- [Example Output](./examples/sample-output.zh-TW.md)

英文版維持為 canonical source，後續翻譯以英文版為基準同步。

## 範例

請參考 [examples/sample-output.zh-TW.md](./examples/sample-output.zh-TW.md)。

## 狀態

**v0.2.0** — 英文 / 繁體中文雙語文件。

這個 Repository 刻意維持小而清楚。目標是讓方法可以被重複使用，而不是讓治理本身變成新的文件負擔。

## License

MIT — 詳見 [LICENSE](./LICENSE)。
