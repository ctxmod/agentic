# Git Commit Execution Objective

Execute commits based on recommendation reports and precommit validation. This objective is decisive: apply recommendations directly without asking for confirmation unless precommit hooks fail.

## Core Behavior

1. **Read**: Consume commit recommendation report
2. **Validate**: Run precommit checks against all repository files
3. **Execute**: Stage files and create commits per recommendations
4. **Report**: Write completion report to the reserved report path

## Methodology

### Phase 1: Recommendation Ingestion

`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/report_ingestion.md`

Read the commit recommendation report provided in parameters:

- Extract commit strategies (primary and alternatives)
- Identify recommended strategy
- Extract file lists, commit messages, and grouping rationale
- Note any work item associations

**Multiple strategies present**: Use the recommended strategy. Do not prompt for selection unless the recommendation report explicitly requires a decision.

### Phase 2: Precommit Validation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Run precommit validation against the recommended files, applying auto-fixes and documenting blocking failures.
`precommit` of the `git_procedures` skill in the ctxmod builtin `commit_discipline` profile
```

Validate before committing:

1. Detect precommit configuration
2. Run hooks against all files in recommendations
3. Apply auto-fixes when available
4. Document blocking failures

**All hooks pass**: Proceed to commit execution.

**Auto-fixes applied**: Re-stage changes and re-validate before committing.

**Manual fixes required**: Document in completion report with file:line references. Do not commit.

### Phase 3: Commit Execution

Brief template for a procedure-depth agent (launched from objective depth):

```text
Stage files and create commits following the recommended strategy and its dependency ordering.
`commits` of the `git_procedures` skill in the ctxmod builtin `commit_discipline` profile
```

Apply the recommended strategy:

1. **Commit each group atomically** with the explicit path list —
   agents never invoke `git add` (the guard denies it); the hook
   rewrites this form to the managed commit runner, which stages the
   named paths internally and restores the index on any failure:
   ```bash
   git commit -m "scope: description" -- <file-list>
   ```

2. **Verify** commit was created:
   ```bash
   git log --oneline -1
   ```

3. **Repeat** for each commit in the strategy

**Commit ordering matters**: Apply in the order specified in recommendations (dependencies before dependents).

### Phase 4: Completion Reporting

Write completion report documenting what was committed:

```markdown
## Commits Created

1. **scope: description** (hash)
   - Files: [list]
   - Work Item: [if applicable]
   - Status: Committed

## Precommit Validation
- Status: [All passed / Auto-fixes applied / Failures blocked]
- Details: [any issues found and resolved]

## Summary
- Commits created: N
- Files modified: N
- Work items completed: [list] (discover associations by searching work-store bodies for the path, e.g. `rg -l <path> <store-dir>`)
```

## Tool Permissions

**Allowed**:
- Bash: git commands, precommit hooks
- Read: Recommendation report, reference files
- Write: reserved report path only

**Forbidden**:
- Modifying file content beyond auto-fixes
- Creating new files
- Deviating from recommended strategy without clear reason
- Launching sub-agents

## Decision Points

**Precommit hooks fail**: Apply auto-fixes and retry. If manual fixes required, stop and document in completion report. Do not use `--no-verify` unless the recommendation report explicitly flags a commit as requiring it.

**Recommendation specifies primary strategy**: Execute primary strategy without confirmation.

**Recommendation asks for selection**: If the report presents 2-3 strategies without a clear recommendation, use the first strategy marked as "recommended" or the first strategy if none marked.

**Isolation**: Worktrees are banned in this repo. If the commit work needs isolation from concurrent writers, it happens in a clean-HEAD clone or archive+init snapshot (see `isolation_by_checkout` in the ctxmod builtin `no_worktrees` profile); commits inside it affect only that checkout's branch, and merge-back is a separate, operator-visible step.

**Unexpected state**: If repository state differs from recommendation (e.g., files changed, conflicts found), document deviation in completion report and stop. Do not commit in unexpected states.

## Response Format

After writing the completion report, return a brief response:

```
Created N commits.

Commits:
- scope: description (hash)
- scope: description (hash)

Completion report: <reserved report path>
```

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Prompting for strategy selection | Adds unnecessary turn | Execute recommended strategy |
| Ignoring precommit failures | Bad commits enter history | Fix or document blockers |
| Committing out of order | Breaks dependencies | Follow recommendation order |
| Modifying commit messages | Defeats recommendation purpose | Use messages from report |
| Skipping validation | Hooks fail later | Always validate first |
| Creating worktree | Banned in this repo — harness isolation params included | Clean-HEAD clone or archive+init snapshot |

## Examples

**Successful Execution**

Recommendation report specifies:
1. `auth: add login and session management`
   - Files: src/auth/login.ts, src/auth/session.ts
2. `auth: add role-based middleware`
   - Files: src/auth/middleware.ts

Precommit validation: All passed

Execution:
```bash
git commit -m "auth: add login and session management" -- src/auth/login.ts src/auth/session.ts
# Created abc123

git commit -m "auth: add role-based middleware" -- src/auth/middleware.ts
# Created def456
```

Completion report: 2 commits created, all precommit checks passed.

**Precommit Failure Blocks Execution**

Recommendation: Single commit for UI fixes

Precommit validation fails:
- Button.tsx:15: unused variable (auto-fix available)
- Form.tsx:42: missing type annotation (manual fix required)

Execution:
- Apply auto-fix for Button.tsx
- Re-run validation
- Form.tsx still fails

Stop and document in completion report:
- Auto-fix applied: Button.tsx
- Blocking issue: Form.tsx:42 missing type annotation
- 0 commits created

**Work Item Completion**

Recommendation links to `feature/user-authentication.story`

Commits created:
1. `auth: add login and session management`
2. `auth: add role-based middleware`

Completion report includes:
- Commits created: 2
- Work item completed: feature/user-authentication.story

## Related

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::git_commit_objective/reference/suggest_commits.md" />
