# Minimum Viable Agent Governance（繁體中文）

Agent Governance 只有在小到能持續保持正確時，才真正有效。

這份文件說明常見治理資產什麼時候值得建立、哪些會互相重疊，以及哪些情況下**不該建立**。

## 四個核心問題

新增任何治理文件前，先確認 Repository 是否能讓 Agent 清楚回答：

| 面向 | 問題 |
|---|---|
| Context | 我現在需要知道什麼？ |
| Authority | 我被允許做什麼？ |
| Evidence | 哪些事情已知？依據在哪裡？ |
| Verification | 我如何證明工作正確？ |

Repository 不需要「一個問題對應一份文件」，但需要清楚的責任歸屬。

## 建議核心治理資產

### AGENTS.md

**用途：** Coding Agent 的持久工作規則。

適合放：

- Repository navigation；
- source-of-truth priority；
- preflight requirements；
- scope boundaries；
- allowed / prohibited actions；
- required verification；
- escalation rules。

避免：

- 短期 project state；
- 冗長 decision history；
- 複製 README；
- 巨型萬用 Prompt。

當你需要讓不同 Session 的 Agent 保持一致工作方式時，才值得建立。

### PROJECT_STATE.md

**用途：** 目前的運作狀態。

適合放：

- active work；
- known issues；
- 會影響下一個任務的 recent changes；
- temporary constraints；
- near-term next steps。

避免：

- 永久 engineering policy；
- 推測性的 roadmap；
- 不會隨時間改變的架構說明。

當 Agent 經常浪費時間重新拼湊「現在做到哪裡」時，才值得建立。

### DECISIONS.md

**用途：** 保存重要決策與 rationale。

適合放：

- context；
- decision；
- reason；
- consequence；
- evidence。

避免：

- 每個小修改都記；
- 自行補 rationale；
- 當成 changelog 使用。

當未來 Agent 很可能重新挑戰已經定案的設計問題時，才值得建立。

### REPO_FACTS.md

**用途：** 集中保存已驗證的 Repository facts。

適合放：

- canonical commands；
- important paths；
- architecture facts；
- deployment/runtime facts；
- source-of-truth references。

避免：

- opinions；
- plans；
- assumptions；
- 很快就過期的狀態。

當重要事實散落各處、常被重複查找或記錯時，才值得建立。

## Optional Artifacts

### Handoff Template

適合任務經常跨 Session、人員或 Agent 時使用。

最小 handoff 至少包含：

- goal；
- current state；
- changes；
- evidence；
- unresolved risks；
- next action。

### Skills

只有當 workflow 同時具備：

- repeated；
- bounded；
- teachable；
- verifiable；
- 足夠穩定可重用；

才值得抽成 Skill。

不要把每一條 instruction 都變成 Skill。

### ROADMAP.md

只有當未來方向真的需要與 current state 分開管理時才建立。

不要把 roadmap 與已驗證 facts 混在一起。

## 實務上的最小集合

很多 Repository 可能只需要：

```text
AGENTS.md
PROJECT_STATE.md
```

若有重要歷史 trade-off，再加入：

```text
DECISIONS.md
```

若技術事實很分散或常被誤記，再加入：

```text
REPO_FACTS.md
```

從小開始。

## Staleness Test

每一份治理文件都應該回答：

> 什麼事件會觸發這份文件更新？

如果沒有 trigger、owner 或 workflow 能讓它保持最新，這份文件最後可能比沒有文件更危險。

可能的更新觸發點：

- release；
- architecture decision；
- migration；
- incident；
- major task completion；
- handoff；
- deployment workflow change。

## 常見失敗模式

### Documentation Maximum
文件太多，導航成本反而高於它解決的問題。

### Session-shaped Documentation
文件只是把某次對話重新整理，沒有形成持久專案結構。

### False Source of Truth
文件聲稱自己是權威來源，但沒有人維護。

### Rules Without Verification
Agent 有照流程走，卻無法證明結果正確。

### Verification Without Authority
Agent 能驗證修改，卻仍可能執行本來不該有權限執行的操作。

## 最小成功條件

一個治理層真正有價值，是當另一個有能力的 Agent 進入 Repository、沒有原始聊天紀錄時，仍然能：

1. 找到相關 context；
2. 理解自己的 authority；
3. 分辨 facts 與 assumptions；
4. 在 bounded scope 內規劃；
5. 驗證結果；
6. 留下有用的 handoff。

超過這些的內容，都應該證明它值得付出維護成本。
