# Precommit Workflow

Orchestrate precommit validation, work item creation, and execution to completion. Precommit workflow discovers issues through hook validation, organizes findings into story with issues, and executes those items.

Validation covers non-test checks only — formatting, lint, reference validity, and commit safety. Under deliberate execution no phase of this workflow executes tests: automatic legacy test-suite invocation is removed, and test evidence enters only through deliberate selection (see Phase 4).

## Workflow Overview

Precommit workflow follows a sequential pattern with conditional work item creation:

```
Phase 1: Precommit Discovery
         ↓
Phase 2: Work Item Creation (conditional)
         ├── Planning
         └── Writing
         ↓
Phase 3: Execution Orchestration
         ↓
Phase 4: Final Validation
         ↓
      Complete
```

## When to Use

Use precommit workflow when:

- Validating repository files against precommit hooks
- Issues discovered require work item tracking
- Execution benefits from story-based organization
- Final verification ensures all issues resolved

Precommit workflow bridges precommit validation with story-based work item execution.

## Phase Structure

### Phase 1: Precommit Discovery

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_precommit.md" />

**Workflow-specific notes**:
- Sequential pattern for discovery
- Detect precommit configuration
- Classify all failures by type (non-test checks only)
- Apply auto-fixes when available
- Output: issue list with categorization

### Phase 2: Work Item Creation (conditional)

**Conditional execution**: Skip when no issues found or all issues auto-fixed.

`ctx:github.com/ctxmod/agentic/context::shared::planning_phase@workflows/reference/work_item.md`

**Workflow-specific notes**:
- Sequential pattern for planning
- Group related issues
- Determine story creation strategy (existing, new under epic, or standalone)
- Main agent resolves identifiers before launch

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation/reference/implementation_work_item.md" />

**Workflow-specific notes**:
- Sequential pattern for creation
- Create story (if new) and issues
- Set parent-child relationships
- Issues reference precommit findings

### Phase 3: Execution Orchestration

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_run.md" />

**Workflow-specific notes**:
- For each work item created, execute run workflow
- Execute work items sequentially, one at a time. Each work item receives all previous work item reports as compulsory reads before execution.
- Dependent items execute sequentially
- Follow run workflow phases (analysis through completion)

### Phase 4: Final Validation

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_precommit.md" />

**Workflow-specific notes**:
- Sequential pattern for validation
- Verify non-test precommit hooks pass
- Confirm all issues resolved
- Report remaining issues if any
- No test execution here: test evidence enters only via explicit deliberate selection — deliberately selected package tests or a deliberately invoked test tool (live capability) — never as an automatic step of this workflow

## Information Flow

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />

```
Phase 1: reports/<YYYY-MM>/<DD-HHMM>-<id>-01-precommit-discovery.md
Phase 2: {STORY_PATH}/reports/<YYYY-MM>/<DD-HHMM>-<id>-02-planning.md (conditional)
         {STORY_PATH}/reports/<YYYY-MM>/<DD-HHMM>-<id>-02-creation.md (conditional)
Phase 3: {ISSUE_PATH}/reports/<YYYY-MM>/<DD-HHMM>-<id>-*.md (per work item)
Phase 4: reports/<YYYY-MM>/<DD-HHMM>-<id>-04-final-validation.md
```

## Variants

### Auto-Fix Only

When all issues resolved via auto-fix:

```
Precommit Discovery → Complete
```

No work items created, no execution needed.

### Append to Existing Story

When `--story` flag provided:

```
Discovery → Planning → Issue Creation → Execution → Validation
```

Uses existing story, appends new issues.

### New Story Under Epic

When `--epic` flag provided:

```
Discovery → Planning → Story + Issue Creation → Execution → Validation
```

Creates new story under epic with issues.

## Error Handling

| Situation | Response |
|-----------|----------|
| No precommit configuration | Report and exit (nothing to do) |
| All issues auto-fixed | Report and exit (no work items needed) |
| Work item validation fails | Follow run workflow remediation |
| Final validation fails | Report remaining issues, propose manual intervention |

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Creating work items for auto-fixed issues | Skip work item creation when all auto-fixed |
| Skipping final validation | Always verify precommit passes |
| Running a test suite during discovery or final validation | Tests never execute automatically; deliberate `--work` (landed) or `test:` (landed) selection only |
