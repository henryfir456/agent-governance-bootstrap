# Governance Bootstrap Prompt（繁體中文）

將這份 Prompt 提供給能存取既有軟體 Repository 的 AI Coding Agent。

---

你現在要替這個既有軟體專案建立一套 **Minimum Viable AI Agent Governance（最小可用 AI Agent 治理）**。

目標不是增加最多文件，而是讓未來任何 Coding Agent——即使來自不同 Session、不同模型或不同開發者——都能可靠理解：

1. 這個 Repository 是什麼、目前狀態如何；
2. 哪些事情可以做、哪些事情不能做；
3. 哪些資訊是已驗證事實，哪些只是推測；
4. 重要設計決策為什麼存在；
5. 修改前必須先確認什麼；
6. 修改後如何用證據驗證；
7. 任務中斷時，下一個 Agent 如何繼續。

## Operating Rule

**Phase 1 只允許 Discovery 與 Proposal。**

在人類明確批准 Phase 2 前：

- 不修改 production code；
- 不 deploy；
- 不執行 migration；
- 不修改 production data；
- 不 rotate 或暴露 secrets；
- 不 commit；
- 不 push。

缺少證據時不要自行推論。任何無法驗證的資訊都標記為 `UNKNOWN`。

---

# A. Repository Discovery

在提出任何治理結構前，先檢查 Repository。

至少檢查可取得的：

- Repository 目錄結構；
- README 與其他文件；
- build / test / lint / type-check / verification 指令；
- dependency 與 package 定義；
- CI/CD；
- deployment 流程；
- branch / worktree 慣例（若可確認）；
- environment / configuration；
- database / migrations；
- API / frontend / backend 邊界；
- authentication / authorization；
- 現有 `AGENTS.md`、`CLAUDE.md`、Copilot instructions、skills、rules 或其他 agent guidance；
- TODO / issue / roadmap / changelog / release notes；
- automation scripts；
- generated / vendor 或不應直接修改的區域；
- 現有 source-of-truth 文件。

不要把「沒有看到證據」當成「確定不存在」。

輸出：

1. **Repository Map**
2. **Current Operating Model**
3. **Existing Governance Mechanisms**
4. **Governance Gaps**
5. **Risk Areas**
6. **需要人類確認的 UNKNOWNs**

---

# B. 提出最小治理層

評估 Repository 是否真的需要下列文件。

**不要機械式全部建立。** 只有在文件具有明確獨立責任時，才建議新增或修改。

## `AGENTS.md`

適合放持久性的 Agent 工作規則，例如：

- Repository 導航；
- allowed / prohibited actions；
- scope boundaries；
- required preflight；
- test / verification expectations；
- escalation conditions；
- source-of-truth priority。

不要把專案歷史全部塞進來。

## `PROJECT_STATE.md`

適合放目前、具時效性的狀態：

- active work；
- recently completed work；
- known issues；
- immediate next work；
- temporary constraints。

它回答的是：**專案現在在哪裡？**

不要放永久政策。

## `DECISIONS.md`

適合保存重要且持久的設計決策。

若有證據，每一項應包含：

- Context
- Decision
- Reason
- Consequence
- Date
- Evidence / source

不要重寫歷史，也不要自行編造 rationale。

## `REPO_FACTS.md`

只放能從 Repository、runtime evidence 或其他 authoritative source 驗證的事實。

例如：

- canonical commands；
- architecture facts；
- deployment targets；
- datastore facts；
- important paths；
- source-of-truth locations。

不要放推測、roadmap 或偏好。

## Optional Artifacts

只有真的解決重複問題時才建議：

- `ROADMAP.md`
- handoff template
- reusable `skills/`
- verification checklist
- release/deployment checklist

---

# C. Agent Lifecycle

設計符合此 Repository 的最小生命週期。

可從以下流程開始：

```text
TASK
  ↓
PREFLIGHT
  ↓
CONTEXT LOAD
  ↓
PLAN
  ↓
EXECUTION
  ↓
VERIFICATION
  ↓
EVIDENCE
  ↓
HANDOFF / COMPLETE
```

## Preflight

開始有意義的修改前，依情況確認：

- git status；
- branch / worktree；
- relevant governing docs；
- affected components；
- available tests；
- dangerous operations；
- external side effects；
- unknown assumptions。

## Plan Freeze

重大修改前先列出：

- Objective
- In scope
- Out of scope
- Files likely affected
- Risks
- Verification plan

執行期間若 scope 有重大變化，先更新計畫再繼續。

## Verification

不要用「應該沒問題」取代證據。

優先使用可重複驗證的 evidence：

- unit tests；
- integration tests；
- build；
- lint；
- static analysis；
- API response；
- database query；
- smoke test；
- diff inspection；
- 可重現的 command output。

若無法驗證，明確回報：

`NOT VERIFIED`

並說明原因。

## Handoff

任務中斷或需要跨 Agent 接手時，至少保留：

- Goal
- Current state
- Changes made
- Files changed
- Tests / evidence
- Remaining issues
- Risks
- Next recommended action

Handoff 必須讓下一個 Agent 不需要重建整段聊天紀錄，也能繼續工作。

---

# D. Authority / Safety Boundary

依 Repository 實際風險建立權限矩陣。

以下只作為起點。

## GREEN — Agent 通常可自行進行

例如：

- read；
- search；
- analysis；
- local tests；
- bounded code edits；
- documentation updates。

## YELLOW — 先揭露風險並收斂 scope

例如：

- dependency changes；
- authentication logic；
- shared API contracts；
- large refactors；
- CI/CD changes；
- infrastructure configuration；
- 不涉及 production mutation 的 data model changes。

## RED — 必須人工明確批准

例如：

- production deployment；
- production data mutation；
- destructive migration；
- delete production data；
- credential rotation；
- expose secrets；
- disable security controls；
- irreversible external actions。

請依 Repository 實際情況調整，不要照抄。

---

# E. 分離 Context、Authority、Evidence、Verification

治理設計必須清楚區分：

## Context
Agent 現在需要知道什麼？

## Authority
Agent 被允許做什麼？

## Evidence
某個 claim、decision 或 current state 的依據是什麼？

## Verification
Agent 如何證明工作正確？

不要把四種責任全部塞進一個巨大 Prompt。

適合長期保存的資訊，應放入 Repository 可版本控制的來源，讓不同 Session 或模型可以重新取得。

---

# F. 控制文件膨脹

提出任何新治理資產前，回答：

1. 它是否有獨立責任？
2. 是否會被重複使用？
3. 比放進現有文件更清楚嗎？
4. 什麼流程會讓它保持最新？
5. 如果三個月沒人更新，它會不會反而變成危險或誤導資訊？

如果答案不足，合併到既有文件或不要建立。

目標是：

**Minimum Viable Governance，不是 Documentation Maximum。**

---

# G. Phase 1 Output

目前不要修改 Repository。

請輸出：

1. Repository status summary
2. Governance gap analysis
3. Proposed governance architecture
4. Proposed files to add or modify
5. Responsibility of each proposed file
6. Proposed agent lifecycle
7. GREEN / YELLOW / RED authority matrix
8. Expected benefits
9. Over-engineering risks
10. Implementation plan
11. Human questions / UNKNOWNs

最後給出：

- `GO` — 建立最小治理層有價值；
- `NO-GO` — Repository 現有治理已足夠，或新增治理只會增加不必要複雜度。

若為 `GO`，先提出 **不超過 3～5 個核心治理資產的最小實作版本**，除非有明確證據需要更多。

等待人工批准後，才進入 Phase 2。

---

# Core Principle

> 不要讓 Agent 記住更多；要讓系統告訴 Agent 去哪裡取得正確資訊。

可用以下方式思考：

```text
Quality
≈ Model Capability
× Context Quality
× Rule Clarity
× Verification Strength
```

這是一個設計 heuristic，不是經過量測的數學公式。
