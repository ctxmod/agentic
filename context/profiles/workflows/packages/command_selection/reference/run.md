# Run Command Selection Guidance

Selection guidance for the `/run` command — the entry point for
orchestrating complete work item execution from current state to
done. This document captures work-item-type adaptations, conditional
phase logic, and error-recovery guidance. Execution lives in the run
workflow (<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_run.md" />) and the
lifecycle phase skills.

## Work Item Type Adaptations

The run workflow adapts execution based on work item type. Type
determines discovery focus, research need, execution pattern, and
completion condition.

| Type | Discovery Focus | Research Need | Execution Pattern | Completion Condition |
|------|-----------------|---------------|-------------------|----------------------|
| Epic | Strategic context | Deep research common | Story decomposition and orchestration | All stories completed or cancelled |
| Story | Feature context | Best practices typical | Issue decomposition and orchestration | All issues completed or cancelled |
| Issue | Implementation context | Specific patterns as needed | Direct changes or task decomposition | Completion criteria met, tasks resolved |
| Task | Specific artifacts | Minimal or none | Direct changes only | Acceptance criteria met |

## Work Item Context

This command requires work item context. It analyzes the work item
definition, determines current progress, and orchestrates remaining
work to completion.

Context resolution order:
1. Explicit parameter (`/run I07`)
2. Active work item in conversation
3. Current directory if within work item path

## Workflow Shape

The run workflow executes existing work items through analysis,
discovery, planning, implementation, and validation phases. See
run workflow (<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_run.md" />) for complete phase definitions,
agent templates, and information flow.

**Phase summary**:

1. Work Item Analysis (governs conditional logic for all subsequent phases)
2. Status Transition (conditional: draft only; auto-transitions parent
   for hierarchical items)
3. Scope Discovery (parallel, conditional: no recent reports within
   24h)
4. Research (conditional: approach unclear)
5. Gap Analysis
6. Execution Planning
7. Execution (creates and orchestrates child items for hierarchical
   work items)
8. Validation (story-completion validator; parallel fan-out only when explicitly selected)
9. Remediation Loop (conditional, max 3 iterations)
10. Completion Verification
11. Status Completion (conditional: verification passes)

## Conditional Phase Execution

Phases execute conditionally based on context. Understanding the
skip conditions prevents redundant work.

| Phase | Executes When | Skips When |
|-------|---------------|------------|
| Phase 2: Status Transition | `status == open` (draft) | Already `in_progress` (active) or halts if `blocked` |
| Phase 3: Scope Discovery | No recent discovery reports (24h threshold) | Recent comprehensive reports exist |
| Phase 4: Research | Approach unclear or work item requests research | Exact implementation specified, purely structural work |
| Phase 9: Remediation Loop | Any Phase 8 validation fails | All validations pass |
| Phase 11: Status Completion | Phase 10 determines `eligible: yes` | Phase 10 determines `eligible: no` |

## Recursive Orchestration

Epics, stories, and issues with child items trigger recursive
execution. Main agent launches parallel run workflows for
independent children, sequential for dependent children. Parent
completes after all children reach terminal state.

**Hierarchical status handling**: Phase 2 automatically transitions
parent status for hierarchical work items. If running a child item
whose parent is in `open` (draft) status, Phase 2 transitions the parent to
`in_progress` (active) before transitioning the child. When running a parent
(epic, story), Phase 2 transitions it before Phase 7 launches
children. No manual status verification or `/lifecycle_start`
commands are required.

## Error Handling

| Situation | Response | Recovery |
|-----------|----------|----------|
| Work item blocked | Report blocker details, halt immediately | Resolve blocker, re-invoke `/run` |
| Remediation loop exhausted (3 iterations) | Report remaining issues, preserve all artifacts and reports | Manual correction then re-invoke `/run`, or accept with known issues |
| Completion verification fails | Report unmet criteria, recommend specific next actions | Address criteria, re-invoke `/run` |
| Child item blocked or failed | Report child status, continue with other children | Address child separately, re-invoke parent `/run` |
| Missing dependencies | Report dependency, halt affected phases | Acquire dependency, re-invoke `/run` |
| Scope exceeds single-session capacity | Report progress, propose decomposition | Decompose into smaller work items |

## Notes

The run command orchestrates complete work item execution from
current state to done. It adapts to work item type, skips unnecessary
phases based on existing progress, and handles the full
validation-remediation cycle. For complex work items (epics,
stories), it recursively orchestrates child item completion. The
command prioritizes efficiency by reusing existing reports and
parallelizing independent work.

When execution cannot complete (blocked, validation loops exhausted,
scope issues), the command provides actionable guidance rather than
failing silently. All progress persists in reports for subsequent
invocations.

## Execution Pointers

- Invoke `/run [work-item]` with explicit identifier when possible
- For hierarchical items, the workflow auto-transitions parents — no
  manual `/lifecycle_start` needed
- Re-invocation is safe: completed phases short-circuit, incomplete
  phases resume from reports

See run workflow (<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_run.md" />) and
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`.
