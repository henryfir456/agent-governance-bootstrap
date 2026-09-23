# Agent Governance Bootstrap

A reusable, discovery-first prompt for adding a **Minimum Viable Agent Governance** layer to an existing software repository before an AI coding agent makes meaningful changes.

> The goal is not to make the agent remember more.  
> The goal is to make the repository tell the agent where to find the right context, what it is allowed to do, and how to prove that the work is correct.

## Why this exists

As coding agents become capable of longer, multi-step work, the main problem shifts from writing a better prompt to managing:

- **Context** — what the agent needs to know now
- **Authority** — what the agent is allowed to do
- **Evidence** — what supports a claim or decision
- **Verification** — how the agent proves the change works

A strong model can still fail when repository state, decision history, permissions, or verification rules are unclear.

This project provides a bootstrap process that asks the agent to inspect the repository first, identify governance gaps, propose the smallest useful governance layer, and stop for human review before making production-impacting changes.

## Core flow

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

## Quick start

1. Open the target repository with your coding agent.
2. Copy [GOVERNANCE_BOOTSTRAP_PROMPT.md](./GOVERNANCE_BOOTSTRAP_PROMPT.md) into the agent.
3. Let the agent perform **Phase 1: discovery and proposal only**.
4. Review the proposed governance architecture.
5. Approve only the pieces that are useful for that repository.
6. Let the agent implement the approved minimum set.
7. Verify that the new governance documents match the real repository.

The prompt is deliberately conservative: the first pass must not deploy, migrate, modify production data, commit, or push.

## What it may propose

Depending on the repository, the minimum useful set might include:

- `AGENTS.md` — working rules, boundaries, navigation, verification expectations
- `PROJECT_STATE.md` — current state, active work, known issues
- `DECISIONS.md` — durable design decisions and their reasoning
- `REPO_FACTS.md` — verified repository facts and source-of-truth references
- a handoff template
- reusable skills for repeated workflows

It should **not** create all of these mechanically. See [MINIMUM_GOVERNANCE.md](./MINIMUM_GOVERNANCE.md).

## Design principles

### 1. Discovery before prescription
The agent inspects the real repository before proposing governance.

### 2. Minimum viable governance
Every document must have a distinct responsibility and a maintenance path.

### 3. Human approval at risk boundaries
Potentially destructive or production-impacting actions require explicit approval.

### 4. Evidence over confidence
“Looks correct” is not verification. Prefer tests, builds, API responses, queries, smoke tests, and diff inspection.

### 5. Unknown stays unknown
If the repository does not provide enough evidence, mark the fact as `UNKNOWN` instead of inventing an answer.

### 6. Repository over session memory
Durable project knowledge should live in a versioned source of truth, not only inside a chat session.

## Suggested authority model

| Level | Typical actions | Default behavior |
|---|---|---|
| GREEN | Read, search, analyze, local tests, bounded edits, docs | Agent may proceed |
| YELLOW | Dependencies, auth, shared API contracts, CI/CD, large refactors | Surface risk and narrow scope |
| RED | Production deploy, destructive migration, production data mutation, credential/security changes | Require explicit human approval |

These categories are starting points, not universal policy. Adapt them to the repository.

## Example

See [examples/sample-output.md](./examples/sample-output.md) for an intentionally generic Phase 1 result.

## Status

**v0.1.0** — initial public bootstrap.

This repository is intentionally small. The objective is to make the method reusable without turning governance into another documentation burden.

## License

MIT — see [LICENSE](./LICENSE).
