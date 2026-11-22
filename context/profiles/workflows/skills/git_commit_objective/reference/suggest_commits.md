# Git Commit Suggestion Objective

Analyze repository changes and recent work items to recommend optimal commit strategies. Produce a commit recommendation report that groups changes logically and follows repository conventions.

## Core Behavior

1. **Analyze**: Examine git status, diff, and recent commit patterns
2. **Discover**: Review recent work items for context about completed work
3. **Classify**: Group changes by logical scope, type, and dependencies
4. **Recommend**: Generate 1-3 commit strategies with rationale
5. **Report**: Write recommendations to the reserved report path

## Methodology

### Phase 1: Repository Pattern Analysis

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

### Phase 2: Change Discovery

Gather current repository state:

```bash
git status
git diff
git diff --staged
```

Identify modified, created, and deleted files with their change types.

### Phase 3: Work Item Context

Compulsory read: `ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/any_work_items.md`

Discover recent work items (epics, stories, issues, tasks) to understand work context:

**Preferred (work-store items)**:
1. `ctxmod work list` (filtered by `--kind`/`--status`) plus a keyword search of record bodies (`rg -il <keyword> <store-dir>`) -- find work items related to the changed scope by title or content keyword.
2. `rg -l <path> <store-dir>` -- reverse lookup which work items reference each changed file path (the store directory is the one `ctxmod state show` reports).
3. `ctxmod work show <id>` and `ctxmod work list --kind report --owner <id>` -- retrieve metadata, status, and report paths for matched items.

`work_store_first` in the ctxmod builtin `work_discipline` profile

**Fallback discovery methodology** (from any-work-items.md):
1. **Scan work-item storage locations** using system-appropriate methods (file globbing, API queries, CLI tools)
2. **Read metadata** from discovered items to extract identifiers, types, statuses, and summaries
3. **Match changes to work items** using the matching algorithm below

Work items provide semantic context for commit messages. Use work item descriptions as inspiration for commit subject lines when changes align with documented work.

### Matching Algorithm

Associate git changes with discovered work items:

1. **Extract work item metadata**:
   - Read metadata fields: identifier, type, status, parent, summary
   - Extract acceptance criteria and completion descriptions

2. **Keyword matching**:
   - Search commit subject/body for work item identifiers
   - Match file paths in commits to work item scope descriptions

3. **Semantic alignment**:
   - Compare commit keywords to work item summaries
   - Check if changes complete acceptance criteria
   - Assess file paths against work item context

4. **Report associations** in format:
   ```
   Commit "{subject}" -> Work Item {ID} ({summary})
   Rationale: {why this commit relates to the work item}
   ```

### Phase 4: Commit Strategy Generation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Classify changed files, build atomic commit groupings, and generate commit messages following repository patterns.
`commit_recommendations` of the `git_procedures` skill in the ctxmod builtin `commit_discipline` profile
```

Apply the commit recommendations procedure to:

1. Classify changed files by module and type
2. Build atomic commit groupings
3. Order dependencies before dependents
4. Generate commit messages following repository patterns
5. Present 1-3 strategies with rationale when multiple valid approaches exist

### Phase 5: Recommendation Synthesis

`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/pattern_aggregation.md`

Synthesize findings into actionable recommendations:

- **Primary strategy**: Recommended approach with strongest rationale
- **Alternative strategies**: Valid approaches with trade-offs documented
- **Work item mapping**: Which commits relate to which work items (if applicable)

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Reserve the commit recommendation report path just-in-time and create the structured output following the report organization and creation procedures.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

### Commit-Specific Report Sections

Beyond standard report structure, commit recommendation reports include:

- **Repository Pattern Analysis**: Identified conventions from recent history
- **Change Classification**: File groupings by module and type
- **Work Item Context**: Related work items with semantic descriptions
- **Commit Strategies**: 1-3 recommended approaches with file lists
- **Rationale**: Why changes are grouped this way
- **Next Steps**: How to proceed with each strategy

**Output format**:
```markdown
## Recommended Strategy

1. **scope: description**
   - Files: [list]
   - Work Item: [related work item, if applicable]
   - Rationale: [why grouped]

2. **scope: description**
   - Files: [list]
   - Rationale: [why grouped]

## Repository Patterns
- Prefix: [identified format]
- Case: [identified case]
- Verb tense: [identified tense]
```

## Tool Permissions

**Allowed**:
- Bash (read-only): git status, diff, log
- Glob: Work item discovery
- Read: Work items, reference files
- Grep: Pattern verification
- Write: reserved report path only

**Forbidden**:
- Running precommit hooks (handled by separate procedure)
- Modifying source files
- Staging or committing changes
- Launching sub-agents

## Precommit Note

This objective does not run precommit validation. Precommit checks happen separately via:

Brief template for a procedure-depth agent (launched from objective depth):

```text
Run precommit validation separately from commit recommendation generation so the two can proceed in parallel.
`precommit` of the `git_procedures` skill in the ctxmod builtin `commit_discipline` profile
```

Separation enables parallelization: commit recommendations generate while precommit validation runs independently.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Including precommit results | Blocks parallelization | Generate recommendations independently |
| Ignoring work items | Miss semantic context | Use work items for commit message inspiration |
| Single strategy always | Misses better alternatives | Present 2-3 when multiple valid approaches exist |
| Repository pattern assumptions | Wrong conventions | Always analyze recent history first |
| Over-large commits | Lose atomicity | Split when sub-components are independent |
| Hardcoded discovery paths | Work items undiscovered when structure differs | Reference discovery objective for correct storage locations |

## Examples

**Feature Completion with Work Item**

Discovered work item: `feature/user-authentication.story`
Changes: `src/auth/login.ts`, `src/auth/session.ts`, `src/auth/middleware.ts`

Recommended:
1. `auth: add login and session management`
   - Files: all auth files
   - Work Item: feature/user-authentication
   - Rationale: Single logical feature, implementation complete

**Mixed Changes Across Modules**

Changes: `pkg/auth/*`, `ui/components/Button.tsx`, `docs/api.md`

Strategy 1 (recommended):
1. `pkg/auth: add role-based access control`
2. `ui: fix button hover state`
3. `docs: update authentication API`

Strategy 2:
1. `auth and ui: RBAC and button fixes`
2. `docs: update authentication API`

Rationale: Strategy 1 preserves atomicity. Strategy 2 groups by commit count but mixes unrelated changes.

## Related

`commits` of the `git_procedures` skill in the ctxmod builtin `commit_discipline` profile

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::git_commit_objective/reference/make_commits.md" />
