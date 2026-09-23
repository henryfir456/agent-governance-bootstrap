# Example Phase 1 Output

> This example is fictional. It demonstrates the expected **shape** of a discovery-first governance proposal, not a universal architecture.

## 1. Repository status summary

Repository: `example-orders-service`

Observed:

- Node.js API under `src/`
- PostgreSQL migrations under `db/migrations/`
- unit tests under `test/`
- GitHub Actions runs tests and lint
- production deployment script exists under `scripts/deploy-prod.sh`
- no agent-specific instructions found
- README documents local startup but not deployment safety

Unknown:

- whether agents are allowed to run production deployment
- who owns schema migration approval
- whether staging uses production-like data

## 2. Governance gap analysis

| Gap | Risk |
|---|---|
| No agent operating rules | Agent may infer unsafe permissions |
| Deployment script discoverable but no authority rule | Accidental production action |
| Current project state exists only in issues/chat | New session must reconstruct context |
| Migration approval rule undocumented | Database changes may exceed scope |
| Verification commands are scattered | Agent may stop after only one test |

## 3. Proposed minimum architecture

```text
AGENTS.md
PROJECT_STATE.md
DECISIONS.md
```

`REPO_FACTS.md` is **not proposed yet** because the README already contains most stable technical facts.

## 4. File responsibilities

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

- why the project uses PostgreSQL advisory locks
- why API v1 remains backward compatible during the migration

## 5. Proposed lifecycle

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

## 6. Authority matrix

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

## 7. Expected benefits

- safer agent defaults,
- less context reconstruction,
- clearer database boundaries,
- repeatable verification,
- easier session handoff.

## 8. Over-engineering risks

Do not create:

- a separate architecture file that duplicates the README,
- a skill for one-off migration work,
- a new roadmap when issues already serve that role.

## 9. Implementation plan

Phase 1 minimum:

1. create `AGENTS.md`,
2. create `PROJECT_STATE.md`,
3. create `DECISIONS.md`,
4. add links from README.

No production code changes.

## 10. Human questions

1. Who may approve a production deployment?
2. Who may approve applying a migration?
3. Is staging data safe for agent-driven tests?

## Recommendation

**GO**

The repository has a clear safety gap around deployment and migrations, while the proposed governance layer remains small enough to maintain.
