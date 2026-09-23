# Minimum Viable Agent Governance

Agent governance works only when it stays small enough to remain true.

This document explains when common governance artifacts are useful, when they overlap, and when **not** to create them.

## The four questions

Before adding any governance file, ask whether the repository gives an agent clear answers to four questions:

| Dimension | Question |
|---|---|
| Context | What do I need to know right now? |
| Authority | What am I allowed to do? |
| Evidence | What is known, and where did it come from? |
| Verification | How do I prove the work is correct? |

A repository does not need one file per question. It needs clear ownership.

## Recommended core artifacts

### AGENTS.md

**Purpose:** durable operating rules for coding agents.

Good content:

- repository navigation,
- source-of-truth priority,
- preflight requirements,
- scope boundaries,
- allowed / prohibited actions,
- required verification,
- escalation rules.

Avoid:

- transient project status,
- long decision history,
- copied README content,
- giant universal prompts.

Create it when agent behavior needs to remain consistent across sessions.

### PROJECT_STATE.md

**Purpose:** current operational state.

Good content:

- active work,
- known issues,
- recent changes that affect the next task,
- temporary constraints,
- near-term next steps.

Avoid:

- permanent engineering policy,
- speculative roadmap,
- timeless architecture documentation.

Create it when agents repeatedly waste time reconstructing "where are we now?"

### DECISIONS.md

**Purpose:** preserve important decisions and rationale.

Good content:

- context,
- decision,
- reason,
- consequence,
- evidence.

Avoid:

- recording every trivial edit,
- invented rationale,
- using it as a changelog.

Create it when future agents are likely to reopen settled design questions.

### REPO_FACTS.md

**Purpose:** compact source of verified repository facts.

Good content:

- canonical commands,
- important paths,
- architecture facts,
- deployment/runtime facts,
- source-of-truth references.

Avoid:

- opinions,
- plans,
- assumptions,
- status that becomes stale quickly.

Create it when facts are scattered across many files or are repeatedly rediscovered.

## Optional artifacts

### Handoff template

Useful when tasks commonly span sessions, people, or agents.

The minimum handoff should capture:

- goal,
- current state,
- changes,
- evidence,
- unresolved risks,
- next action.

### Skills

A skill is justified when a workflow is:

- repeated,
- bounded,
- teachable,
- verifiable,
- stable enough to reuse.

Do not turn every instruction into a skill.

### ROADMAP.md

Useful only when future direction must be explicit and maintained independently from current state.

Do not mix roadmap with verified current facts.

## A practical minimum

Many repositories need only:

```text
AGENTS.md
PROJECT_STATE.md
```

A repository with important historical trade-offs may add:

```text
DECISIONS.md
```

A repository with fragmented or frequently misremembered technical facts may add:

```text
REPO_FACTS.md
```

Start small.

## Staleness test

Every governance artifact should have an answer to:

> What causes this document to be updated?

If there is no trigger, owner, or workflow that keeps it current, the document may eventually become worse than having no document at all.

Possible update triggers:

- release,
- architecture decision,
- migration,
- incident,
- major task completion,
- handoff,
- deployment workflow change.

## Failure modes

### Documentation maximum
Too many files create more navigation work than they remove.

### Session-shaped documentation
A document mirrors one conversation instead of durable project structure.

### False source of truth
A governance document claims authority but is not maintained.

### Rules without verification
The agent follows process but cannot prove the result.

### Verification without authority
The agent can test changes but may still perform actions it should not be allowed to perform.

## Minimum success criteria

A governance layer is useful when another capable agent can enter the repository and, without the original chat:

1. find the relevant context,
2. understand its authority,
3. distinguish facts from assumptions,
4. plan within a bounded scope,
5. verify the result,
6. leave a useful handoff.

Anything beyond that should earn its maintenance cost.
