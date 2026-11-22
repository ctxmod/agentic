---
description: "Lifecycle status phase — read-only work item status discovery through context resolution, status gathering, and hierarchy mapping."
binds_to_depth: phase
---

# Lifecycle Status Phase

Lifecycle status phase — discover work item status and hierarchy
(read-only). This is one of four sibling leaf phase skills in the
lifecycle capability (start, block, status, complete); it dispatches
bare per the only-where-content-exists decision — no
objective/procedure layer.

Lifecycle status is a **leaf phase**. The main agent invokes it directly
via the absorbed `/lifecycle_status` slash-command or any other domain's
workflow agent references this phase as a "lifecycle status" sub-step.

Sub-agents follow `work_store_first` in the ctxmod builtin `work_discipline` profile for read operations.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Establishing work context before starting work
- Re-establishing context after returning to a project
- Surveying active work items across project
- Checking work item hierarchy and relationships
- Understanding blocker scope and affected items

The status phase produces no persistent changes — status display is
the primary deliverable.

## Phase Orchestration

This leaf phase dispatches Haiku sub-agents for status gathering and
hierarchy mapping; no objective/procedure layer exists. Two modes
based on context availability:

**Specific Item Mode**:
```
Phase 1: Context Resolution (main agent)
         |
Phase 2: Status Gathering (Haiku, conditional)
         |
Phase 3: Hierarchy Mapping (Haiku, conditional)
         |
Phase 4: Status Display (main agent)
```

**General Survey Mode**:
```
Phase 1: Context Resolution (main agent)
         |
Phase 2: Status Gathering (Haiku, conditional)
         |
Phase 4: Status Display (main agent)
```

### Phase 1: Context Resolution

`ctx:github.com/ctxmod/agentic/context::shared::lifecycle_start_phase@work/reference/context_resolution.md`

**Workflow-specific notes**:
- Main agent determines scope from parameter, conversation, or directory
- **Context modes**:
  - **Specific item**: Work item identifier resolved
  - **General**: No specific work item context
- No sub-agent needed
- Mode determines which conditional phases execute

### Phase 2: Status Gathering (conditional)

**If specific item context**:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::lifecycle_status_phase/reference/status_gathering.md" />

**If general context (no specific item)**:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::lifecycle_status_phase/reference/work_item_survey.md" />

- Report (specific): `<work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-status.md`
- Report (general): `reports/<YYYY-MM>/<DD-HHMM>-<id>-work-item-survey.md`

**Workflow-specific notes**:
- Conditional on context mode from Phase 1
- Specific item mode gathers detailed status for resolved work item
- General mode surveys all active and blocked items across project

### Phase 3: Hierarchy Mapping (specific-item only, conditional)

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::lifecycle_status_phase/reference/hierarchy_mapping.md" />

- Report: `<work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-hierarchy.md`

Skipped in general survey mode.

**Workflow-specific notes**:
- Conditional: executes only in specific item context
- Skipped in general survey mode
- Consumes Phase 2 status report for parent/child context

### Phase 4: Status Display

Main agent formats and returns status information. No sub-agent needed.

**For specific item context**:

```
Work Item: {ID} - {Title}
Status: {status}
Type: {epic|story|issue|task}

Parent: {parent ID and title, or "None"}

Children:
- {child ID}: {title} ({status})
- {child ID}: {title} ({status})

Blockers: {none, or list}

Recent Reports:
- {date}: {report slug}
```

**For general context**:

```
Active Work Items:

Epics:
- E01: {title} ({status})

Stories:
- S03: {title} ({status})

Issues:
- I07: {title} ({status})
- I12: {title} ({status}) [BLOCKED]

Tasks:
- T02: {title} ({status})
```

## Information Flow

```
Phase 1: Context Resolution
- Receives: User parameter or context
- Produces: WORK_ITEM_PATH (if specific), CONTEXT_MODE (specific|general)

Phase 2: Status Gathering
- Specific: Receives WORK_ITEM_PATH, WORK_ITEM_TYPE
           Produces: <work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-status.md
- General: Produces: reports/<YYYY-MM>/<DD-HHMM>-<id>-work-item-survey.md

Phase 3: Hierarchy Mapping (specific only)
- Receives: WORK_ITEM_PATH, STATUS_REPORT
- Produces: <work item dir>/reports/<YYYY-MM>/<DD-HHMM>-<id>-hierarchy.md

Phase 4: Status Display
- Receives: Status report, hierarchy report (if applicable)
- Produces: Formatted display to developer
```

Main agent passes report paths between phases, never report content.

## Conditional Phase Logic

| Phase | Executes When | Skips When |
|-------|---------------|------------|
| Phase 2 | Always | Never |
| Phase 3 | Specific item context | General context |
| Phase 4 | Always | Never |

Phase 1 always executes.

## Workflow Variants

### Specific Item Status

Status for explicit work item:

```
/lifecycle_status mnbvcxzqwr
```

Shows that issue's status, hierarchy, children, and blockers.

### Context-Based Status

Status for current context (conversation or directory):

```
/lifecycle_status
# While in a story's item directory: <store>/items/2026-02/22-0815-kpqrstbcdf-auth-overhaul/
```

Shows that story's status and hierarchy.

### General Project Survey

Status for all active work items:

```
/lifecycle_status
# No specific context
```

Shows all active and blocked items across the project.

## Output Characteristics

**Read-only**: Status phase produces no persistent changes to work items.

**Report persistence**: Status reports are written to work item
`reports/` directories for audit trail, but the primary deliverable is
the formatted display in Phase 4.

**Context re-establishment**: Status phase helps developers
re-establish context after returning to a project by showing active
work and blockers.

## Error Handling

| Situation | Response | Recovery |
|-----------|----------|----------|
| Context resolution finds nothing | Proceed with general survey mode | No recovery needed |
| Status gathering fails | Report error, skip to display | Re-invoke with explicit item |
| Hierarchy mapping fails | Report partial results | Continue without hierarchy |

Error handling is lenient — status phase provides best-effort visibility
rather than halting on failures.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Modifying work items | Status is read-only | Use start/block/complete phases for modifications |
| Skipping hierarchy mapping | Incomplete context picture | Map full parent chain for specific items |
| Deep progress analysis | Exceeds scope | Focus on status and hierarchy only |
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
the `lifecycle_block_phase` skill
the `lifecycle_complete_phase` skill
