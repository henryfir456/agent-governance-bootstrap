# Governance Bootstrap Prompt

Use this prompt with an AI coding agent that has access to an existing software repository.

---

You are going to establish a **Minimum Viable AI Agent Governance** layer for this existing software repository.

The objective is not to maximize documentation. The objective is to let future coding agents — across different sessions, models, or developers — reliably understand:

1. what this repository is and its current state,
2. what they may and may not change,
3. which information is verified fact versus assumption,
4. why important design decisions exist,
5. what must be checked before making changes,
6. how work must be verified with evidence,
7. how another agent can continue if the task is interrupted.

## Operating rule

**Phase 1 is discovery and proposal only.**

Until a human explicitly approves Phase 2:

- do not modify production code,
- do not deploy,
- do not run migrations,
- do not mutate production data,
- do not rotate or expose secrets,
- do not commit,
- do not push.

Do not infer missing facts. Mark anything that cannot be verified as `UNKNOWN`.

---

# A. Repository Discovery

Inspect the repository before proposing any governance structure.

At minimum, inspect what is available for:

- repository directory structure,
- README and documentation,
- build, test, lint, type-check, and verification commands,
- dependency and package definitions,
- CI/CD,
- deployment flow,
- branch or worktree conventions if discoverable,
- environment and configuration,
- database and migrations,
- API / frontend / backend boundaries,
- authentication and authorization,
- existing `AGENTS.md`, `CLAUDE.md`, Copilot instructions, skills, rules, or agent guidance,
- TODOs, issues, roadmap, changelog, or release notes present in the repo,
- automation scripts,
- generated/vendor files or areas that should not be edited directly,
- existing source-of-truth documents.

Do not treat absence of evidence as evidence of absence.

Produce:

1. **Repository Map**
2. **Current Operating Model**
3. **Existing Governance Mechanisms**
4. **Governance Gaps**
5. **Risk Areas**
6. **UNKNOWNs that require human clarification**

---

# B. Propose the Minimum Governance Layer

Evaluate whether this repository needs any of the following.

Do **not** create all files mechanically. Create or recommend a file only when it has a distinct responsibility.

## `AGENTS.md`

Use for durable agent working rules such as:

- repository navigation,
- allowed and prohibited actions,
- scope boundaries,
- required preflight,
- test and verification expectations,
- escalation conditions,
- source-of-truth priority.

Do not use it as a dumping ground for project history.

## `PROJECT_STATE.md`

Use for current, time-sensitive state:

- active work,
- recently completed work,
- known issues,
- immediate next work,
- temporary constraints.

It describes **where the project is now**.

Do not put permanent policy here.

## `DECISIONS.md`

Use for important durable design decisions.

Each entry should contain, when evidence is available:

- Context
- Decision
- Reason
- Consequence
- Date
- Evidence / source

Do not rewrite history or invent rationale.

## `REPO_FACTS.md`

Use only for facts that can be verified from the repository, runtime evidence, or another authoritative source.

It may include:

- canonical commands,
- architecture facts,
- deployment targets,
- datastore facts,
- important paths,
- source-of-truth locations.

Do not put guesses, roadmap items, or preferences here.

## Optional artifacts

Only recommend these if they solve a real recurring problem:

- `ROADMAP.md`
- handoff template
- reusable `skills/`
- verification checklist
- release/deployment checklist

---

# C. Agent Lifecycle

Design the smallest lifecycle that fits this repository.

Use this as a starting point:

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

Before meaningful modification, the agent should establish as applicable:

- git status,
- branch / worktree,
- relevant governing docs,
- affected components,
- available tests,
- dangerous operations,
- external side effects,
- unknown assumptions.

## Plan Freeze

Before a significant change, state:

- Objective
- In scope
- Out of scope
- Files likely affected
- Risks
- Verification plan

If the scope materially changes during execution, update the plan before continuing.

## Verification

Never use confidence language as a substitute for evidence.

Prefer repeatable evidence such as:

- unit tests,
- integration tests,
- build,
- lint,
- static analysis,
- API response,
- database query,
- smoke test,
- diff inspection,
- reproducible command output.

If something cannot be verified, explicitly report:

`NOT VERIFIED`

and state why.

## Handoff

For an interrupted or multi-agent task, capture:

- Goal
- Current state
- Changes made
- Files changed
- Tests / evidence
- Remaining issues
- Risks
- Next recommended action

The handoff must be sufficient for another agent to continue without reconstructing the entire chat history.

---

# D. Authority and Safety Boundaries

Adapt an authority matrix to the actual repository.

Use these levels only as a starting point.

## GREEN — agent may normally proceed

Examples:

- read,
- search,
- analysis,
- local tests,
- bounded code edits,
- documentation updates.

## YELLOW — proceed only after surfacing risk and tightening scope

Examples:

- dependency changes,
- authentication logic,
- shared API contracts,
- large refactors,
- CI/CD changes,
- infrastructure configuration,
- data model changes without production mutation.

## RED — require explicit human approval

Examples:

- production deployment,
- production data mutation,
- destructive migration,
- deleting production data,
- credential rotation,
- exposing secrets,
- disabling security controls,
- irreversible external actions.

Adjust these categories to repository-specific risk.

---

# E. Separate Context, Authority, Evidence, and Verification

The governance design must distinguish:

## Context
What does the agent need to know?

## Authority
What is the agent allowed to do?

## Evidence
What supports a claim, decision, or current state?

## Verification
How does the agent prove that its work is correct?

Do not place all four responsibilities into one giant prompt.

Durable information should live in repository-controlled sources when appropriate so it can be recovered by another session or model.

---

# F. Control Documentation Growth

Before proposing any new governance artifact, answer:

1. Does it have a distinct responsibility?
2. Will it be reused?
3. Is it clearer than placing the information in an existing document?
4. What process keeps it current?
5. If nobody updates it for three months, could it become dangerous or misleading?

If the answers are weak, merge the information into an existing artifact or do not create it.

The target is:

**Minimum Viable Governance, not Documentation Maximum.**

---

# G. Phase 1 Output

Do not modify the repository yet.

Return:

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

Finish with one of:

- `GO` — a minimum governance layer is useful,
- `NO-GO` — the current repository already has sufficient governance or the change would add unnecessary complexity.

If `GO`, propose a **Phase 1 minimum implementation of no more than 3–5 core artifacts** unless there is strong evidence that more are necessary.

Wait for human approval before Phase 2.

---

# Core principle

> Do not make the agent remember more. Make the system tell the agent where to find the right information.

A useful mental model is:

```text
Quality
≈ Model Capability
× Context Quality
× Rule Clarity
× Verification Strength
```

Treat this as a design heuristic, not a measured formula.
