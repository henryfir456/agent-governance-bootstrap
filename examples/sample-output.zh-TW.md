# Phase 1 輸出範例（繁體中文）

> 這是一個虛構範例，只示範 discovery-first 治理提案的**結構**，不是所有 Repository 都應照抄的架構。

## 1. Repository 狀態摘要

Repository：`example-orders-service`

已觀察：

- Node.js API 位於 `src/`
- PostgreSQL migrations 位於 `db/migrations/`
- unit tests 位於 `test/`
- GitHub Actions 會執行 tests 與 lint
- production deployment script 位於 `scripts/deploy-prod.sh`
- 沒有找到 agent-specific instructions
- README 有 local startup 說明，但沒有 deployment safety 規則

未知：

- Agent 是否被允許執行 production deployment
- 誰負責 schema migration approval
- staging 是否使用類 production data

## 2. Governance Gap Analysis

| 缺口 | 風險 |
|---|---|
| 沒有 Agent 工作規則 | Agent 可能自行推論不安全的權限 |
| 可看到 deploy script，但沒有 authority rule | 可能誤觸 production |
| Current project state 只存在 issue/chat | 新 Session 必須重新拼湊背景 |
| Migration approval 規則未文件化 | DB 變更可能超出 scope |
| Verification commands 分散 | Agent 可能只跑單一測試就停止 |

## 3. 建議最小架構

```text
AGENTS.md
PROJECT_STATE.md
DECISIONS.md
```

目前**不建議**建立 `REPO_FACTS.md`，因為 README 已涵蓋大部分穩定技術事實。

## 4. 文件責任

### AGENTS.md

- navigation
- local verification commands
- source-of-truth priority
- deployment boundary
- migration boundary
- authority matrix

### PROJECT_STATE.md

- current migration work
- known failing integration test
- active API versioning change

### DECISIONS.md

- 為什麼專案使用 PostgreSQL advisory locks
- 為什麼 migration 期間 API v1 維持 backward compatibility

## 5. 建議 Lifecycle

```text
Task
 ↓
Preflight
 ↓
Load AGENTS.md + relevant state
 ↓
Plan Freeze
 ↓
Bounded implementation
 ↓
Test + lint + targeted integration test
 ↓
Diff review
 ↓
Handoff / complete
```

## 6. Authority Matrix

### GREEN

- read/search
- local tests
- documentation
- bounded application-code edits

### YELLOW

- dependency changes
- auth changes
- API contract changes
- migration file creation

### RED

- running production deployment
- applying production migration
- production data mutation
- credential changes

## 7. 預期效益

- 更安全的 Agent 預設行為；
- 減少每次 Session 重新建立背景；
- DB 邊界更清楚；
- verification 可重複；
- session handoff 更容易。

## 8. Over-engineering Risks

不要建立：

- 與 README 重複的額外 architecture file；
- 只使用一次的 migration skill；
- 如果 issue 已能管理 roadmap，就不要再新增 roadmap。

## 9. Implementation Plan

Phase 1 minimum：

1. 建立 `AGENTS.md`
2. 建立 `PROJECT_STATE.md`
3. 建立 `DECISIONS.md`
4. 在 README 加入連結

不修改 production code。

## 10. Human Questions

1. 誰可以批准 production deployment？
2. 誰可以批准 migration apply？
3. Staging data 是否適合 Agent-driven tests？

## Recommendation

**GO**

這個 Repository 在 deployment 與 migration 的安全邊界上有明顯缺口，而建議治理層仍維持在可維護的最小範圍。
