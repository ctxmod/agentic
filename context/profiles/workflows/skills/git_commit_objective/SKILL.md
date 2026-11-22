---
description: "Produce commit-suggestion reports and commit-execution reports — atomic, repository-convention-aligned git commit strategies with work-item context."
binds_to_depth: objective
---

# Git Commit Objective

Produce commit reports that organize repository changes into atomic,
logically-grouped commits following repository conventions. Two
objective variants live here: producing commit recommendations
(suggestion) and executing commits based on prior recommendations
(make-commits). Both write structured reports to report paths
reserved just-in-time (`ctxmod work new report "<title>"`); neither
prompts for confirmation — suggestion presents
options as a report, make-commits executes the recommended strategy
directly.

## Variant Selection

Read the variant matching the assigned task. Both variants share the
core methodology but diverge on output: suggestion produces
recommendations; make-commits produces commit hashes.

| Assigned task | Read |
|---------------|------|
| Analyze changes and recommend commit strategies | `reference/suggest-commits.md` |
| Execute commits based on a recommendation report | `reference/make-commits.md` |

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::git_commit_objective/reference/make_commits.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::git_commit_objective/reference/suggest_commits.md" />

Variants must be read when even somewhat relevant.

## Core Behavior

Both variants share this core sequence:

1. **Analyze**: Examine git status, diff, and recent commit patterns
2. **Discover**: Review recent work items for context (when applicable)
3. **Classify**: Group changes by logical scope, type, and dependencies
4. **Act**: Recommend strategies (suggest-commits) or execute the
   recommended strategy (make-commits)
5. **Report**: Write output to the reserved report path

## Methodology

### Repository Pattern Analysis

Analyze recent commit history to identify repository conventions:

```bash
git log --oneline -20
git log --oneline -- path/to/changed/files
```

Identify:
- Prefix conventions (scope format)
- Case and verb tense
- Subject line length
- Message structure conventions

### Change Discovery

Gather current repository state:

```bash
git status
git diff
git diff --staged
```

Identify modified, created, and deleted files with their change types.

### Work Item Context

When work-item context is available (EPIC_PATH, STORY_PATH,
ISSUE_PATH, TASK_PATH), discover recent work items for semantic
context that can inform commit messages:

`work_store_first` in the ctxmod builtin `work_discipline` profile

Work items provide semantic context for commit messages. Use work item
descriptions as inspiration for commit subject lines when changes
align with documented work.

### Strategy Generation

For suggest-commits, classify changed files and build 1-3 atomic
commit strategies with rationale. For make-commits, ingest the
recommendation report and execute the primary strategy in dependency
order (dependencies before dependents).

## Supporting Procedures

Git commit procedures carry the step-level mechanics for each task.
Link conditionally on what the assigned objective requires:

the `git_commit_procedure` skill of the ctxmod builtin `commit_discipline` profile

| Objective | Primary procedure |
|-----------|-------------------|
| suggest-commits | `commit_recommendations` in the ctxmod builtin `commit_discipline` profile |
| make-commits | `commits` in the ctxmod builtin `commit_discipline` profile + `precommit` in the ctxmod builtin `commit_discipline` profile |

## Conditional-Link Dispatch Table

| Phase invoking this objective | Variant to dispatch | Procedures launched |
|-------------------------------|---------------------|---------------------|
| `git_commit_default_phase` (Phase 1) | suggest-commits | `commit-recommendations` |
| `git_commit_default_phase` (Phase 3) | make-commits | `commits` + `precommit` |
| `git_commit_safe_phase` (Phase 1) | suggest-commits (2-3 strategies) | `commit-recommendations` |
| `git_commit_safe_phase` (Phase 4) | make-commits (approved only) | `commits` + `precommit` |
| `git_commit_unsafe_phase` (Phase 1) | suggest-commits | `commit-recommendations` |
| `git_commit_unsafe_phase` (Phase 3) | make-commits (unsafe mode) | `commits` + `precommit` (informational) |
| Ad-hoc commit analysis (any other objective needing commit recommendations) | suggest-commits | `commit-recommendations` |

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Reserve the report path just-in-time and create the structured commit report.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

Commit reports include:

- **Repository Pattern Analysis**: Identified conventions from recent history
- **Change Classification**: File groupings by module and type
- **Work Item Context**: Related work items with semantic descriptions
- **Commit Strategies** (suggest-commits): 1-3 recommended approaches with file lists
- **Commits Created** (make-commits): Hashes, file lists, validation status
- **Rationale**: Why changes are grouped or executed this way
- **Next Steps**: How to proceed with each strategy or follow-up actions

## Tool Permissions

**Allowed**:
- Bash: git commands (read-only for suggest-commits; mutating for make-commits)
- Bash: precommit hooks (make-commits only)
- Read: Recommendation report, reference files
- Glob: Work item discovery
- Grep: Pattern verification
- Write: reserved report path only

**Forbidden** (suggest-commits):
- Modifying source files
- Staging or committing changes
- Running precommit hooks
- Launching sub-agents

**Forbidden** (make-commits):
- Modifying file content beyond auto-fixes
- Creating new files
- Deviating from recommended strategy without clear reason
- Launching sub-agents

## Response Format

After writing the report, return a brief response:

**suggest-commits**:
```
Wrote commit recommendation report to <reserved report path>.

Summary: [2-3 sentence summary of recommended strategy and rationale]
```

**make-commits**:
```
Created N commits.

Commits:
- scope: description (hash)
- scope: description (hash)

Completion report: <reserved report path>
```

Do not return the full report content. The report file is the deliverable.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Prompting for strategy selection (make-commits) | Adds unnecessary turn | Execute recommended strategy |
| Ignoring precommit failures (make-commits, validated mode) | Bad commits enter history | Fix or document blockers |
| Single strategy when alternatives exist (suggest-commits) | Misses better alternatives | Present 2-3 when multiple valid approaches exist |
| Repository pattern assumptions | Wrong conventions | Always analyze recent history first |
| Over-large commits | Lose atomicity | Split when sub-components are independent |
| Modifying commit messages from report (make-commits) | Defeats recommendation purpose | Use messages from report |
| Including precommit results (suggest-commits) | Blocks parallelization | Generate recommendations independently |

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`
`work_store_first` in the ctxmod builtin `work_discipline` profile

## Sub-Layer Launch

When this objective requires step-level execution, launch the
`git-commit-procedure` agent. Procedure agents carry the per-procedure
mechanics under 
- `commit_recommendations` in the ctxmod builtin `commit_discipline` profile
- `commits` in the ctxmod builtin `commit_discipline` profile
- `precommit` in the ctxmod builtin `commit_discipline` profile
.

Brief template for the `git_commit_procedure` agent — Execute <procedure-name> within git commit objective:

```text
<ctxmod_activate skill="git_commit_procedure" />
Procedure: <procedure-name>
<procedure-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Available procedure agents in the git commit capability: dispatch
against the procedure variants in 
- `commit_recommendations` in the ctxmod builtin `commit_discipline` profile
- `commits` in the ctxmod builtin `commit_discipline` profile
- `precommit` in the ctxmod builtin `commit_discipline` profile

(commit-recommendations, commits, precommit).
