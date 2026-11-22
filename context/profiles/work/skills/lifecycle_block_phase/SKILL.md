---
description: "Lifecycle block phase — mark a work item as blocked with documented reason through context resolution, transition verification, blocker documentation, and impact analysis."
binds_to_depth: phase
---

# Lifecycle Block Phase

Lifecycle block phase — mark an active work item as blocked, documenting
the impediment and assessing impact on related work. This is one of
four sibling leaf phase skills in the lifecycle capability (start,
block, status, complete); it dispatches bare per the only-where-content-exists
decision — no objective/procedure layer.

Lifecycle block is a **leaf phase**. The main agent invokes it directly
via the absorbed `/lifecycle_block` slash-command or any other domain's
workflow agent references this phase as a "lifecycle block" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Work item progress is impeded by external factors
- Documenting blockers for visibility and tracking
- Assessing impact on child and dependent items
- Transitioning active items to blocked state

The block phase applies to all work item types with consistent
documentation requirements.

## Phase Orchestration

This leaf phase dispatches Haiku sub-agents for verification,
documentation, and impact analysis; no objective/procedure layer exists.

```
Phase 1: Context Resolution (main agent)
         |
Phase 2: Transition Verification (Haiku)
         |
Phase 3: Status Transition + Blocker Documentation (Haiku, conditional)
         |
Phase 4: Impact Analysis (Haiku)
         |
Phase 5: Notification (main agent)
```

### Phase 1: Context Resolution

`ctx:github.com/ctxmod/agentic/context::shared::lifecycle_start_phase@work/reference/context_resolution.md`

**Workflow-specific notes**:
- Main agent resolves work item from parameter, conversation, or directory
- Validates blocking reason is provided (`--reason` flag required)
- Determines WORK_ITEM_PATH, WORK_ITEM_TYPE, WORK_ITEM_ID, BLOCKER_REASON
- No sub-agent needed

### Phase 2: Transition Verification

`ctx:github.com/ctxmod/agentic/context::shared::lifecycle_start_phase@work/reference/transition_verification.md`

- Report: `<work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-block-verification.md`

**Workflow-specific notes**:
- Haiku sub-agent verifies work item can transition to blocked
- Checks current status is `in_progress` (active)
- Identifies child items that may be affected
- Produces verification report with eligibility determination

### Phase 3: Status Transition + Blocker Documentation (conditional)

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::lifecycle_block_phase/reference/blocker_documentation.md" />

**Workflow-specific notes**:
- **Conditional**: Executes only if Phase 2 verification passes
- Combined operation: updates status AND documents blocker atomically
- Changes status from `in_progress` to `blocked` (`ctxmod work set <id> status=blocked`)
- Adds blocker entry to Blockers section
- Preserves existing blockers if section exists

### Phase 4: Impact Analysis

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::lifecycle_block_phase/reference/impact_analysis.md" />

- Report: `<work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-block-impact.md`

**Workflow-specific notes**:
- **Conditional**: Executes after Phase 3 completes (blocking was successful)
- Haiku sub-agent analyzes impact on related items
- Identifies child items that cannot proceed
- Notes dependent items in other branches

### Phase 5: Notification

Main agent returns result to developer. No sub-agent needed.

**Success notification includes**:
- Work item identifier
- Blocker reason
- New status (blocked)
- Impact summary (affected children, dependents)

**Failure notification includes**:
- Work item identifier
- Current status (if not active)
- Reason blocking cannot proceed

## Information Flow

```
Phase 1: Context Resolution
- Receives: User parameter, --reason flag
- Produces: WORK_ITEM_PATH, WORK_ITEM_TYPE, WORK_ITEM_ID, BLOCKER_REASON

Phase 2: Transition Verification
- Receives: WORK_ITEM_PATH, TRANSITION_TYPE=active-to-blocked
- Produces: <work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-block-verification.md

Phase 3: Status Transition + Blocker Documentation
- Receives: WORK_ITEM_PATH, BLOCKER_REASON, BLOCK_DATE, VERIFICATION_REPORT
- Produces: Modified {WORK_ITEM_PATH}/{TYPE}.md

Phase 4: Impact Analysis
- Receives: WORK_ITEM_PATH, TRANSITION_TYPE=active-to-blocked
- Produces: <work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-block-impact.md

Phase 5: Notification
- Receives: Transition result, impact analysis report
- Produces: Summary response to developer
```

Main agent passes report paths between phases, never report content.

## Conditional Phase Logic

| Phase | Executes When | Skips When |
|-------|---------------|------------|
| Phase 2 | Always | Never |
| Phase 3 | Verification passes | Verification fails |
| Phase 4 | Phase 3 completes | Phase 3 skipped |
| Phase 5 | Always | Never |

Phases 1 and 5 always execute.

## Blocker Documentation Format

Blockers are documented in a Blockers section:

```markdown
## Blockers

### {Date}: {Brief Title}

{Detailed description of blocking condition}

**Resolution path**: {What needs to happen to unblock}
```

Multiple blockers accumulate. Resolution via `/lifecycle_start` removes
entries.

## Workflow Variants

### Direct Block

Block without explicit work item parameter (resolves from context):

```
/lifecycle_block --reason "Waiting for API specification"
```

Main agent resolves work item from active conversation or current directory.

### Explicit Block

Block with explicit work item identifier:

```
/lifecycle_block mnbvcxzqwr --reason "Waiting for API specification"
```

Main agent uses provided identifier directly.

### Multi-Block

Documenting multiple blockers over time:

```
/lifecycle_block mnbvcxzqwr --reason "First blocker"
# ... later ...
/lifecycle_block mnbvcxzqwr --reason "Second blocker"
```

Each invocation adds a new blocker entry while preserving existing blockers.

## State Transitions

| From | To | Criteria |
|------|-----|----------|
| active | blocked | Blocker reason documented |

Blocking from other states (draft, blocked, completed, cancelled) is
invalid. Return error with current state.

## Error Handling

| Situation | Response | Recovery |
|-----------|----------|----------|
| Context resolution fails | Halt, report ambiguity | Specify work item explicitly |
| Missing --reason flag | Halt, report required flag | Provide blocking reason |
| Verification fails (not active) | Skip Phase 3-4, report state | Transition to active first |
| Blocker documentation fails | Halt, report failure | Manual correction, re-invoke |
| Impact analysis fails | Continue, report partial results | Optional analysis phase |

Error handling follows stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Blocking without reason | Undocumented impediment | Always require --reason flag |
| Blocking non-active items | Invalid state transition | Verify current status first |
| Skipping impact analysis | Incomplete picture of effects | Analyze affected children/dependents |
| Overwriting existing blockers | Lost historical context | Preserve and accumulate blockers |
| Reading intermediate reports | Context bloat | Pass report paths only |

## Cross-Cutting Reference

This skill links cross-cutting principles at their existing `reference/`
paths per intent §4.3 — they are not duplicated here:

- main-agent posture — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />
- orchestration context shape — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
- parameter passing conventions — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
- work-item discovery via the ctxmod work store — `work_store_first` in the ctxmod builtin `work_discipline` profile

## Sibling Phase Reference

The other three lifecycle leaf phase skills:

the `lifecycle_start_phase` skill
the `lifecycle_status_phase` skill
the `lifecycle_complete_phase` skill
