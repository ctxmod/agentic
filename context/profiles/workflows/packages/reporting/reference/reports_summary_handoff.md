# Handoff Summary Report Objective

Create handoff summaries enabling seamless continuation by successors. Handoff summaries capture current state, pending items, relevant background, and implicit knowledge that might otherwise be lost during transitions.

## Base Behavior

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_summary.md" />

## Report Schema

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_summary_handoff.md" />

The schema defines frontmatter requirements, required sections (handoff summary, current state, pending items, decisions made, open questions, reference materials), and length guidelines. Follow the schema for report structure.

## Audience

Handoff summaries target:

- Successors continuing work after session boundary
- Team members inheriting responsibilities
- Future agents resuming long-running tasks
- Reviewers needing historical context

Readers need sufficient context to continue without the original participant.

## Focus Areas

| Priority | Content |
|----------|---------|
| High | Current state, pending items, blocking issues |
| Medium | Decisions made, rationale, assumptions |
| Low | Completed work (unless relevant to pending) |

Emphasize what successors need to continue effectively.

## Methodology

### Phase 1: Report Ingestion

`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/report_ingestion.md`

**Preferred (work-store items)**:
- `ctxmod work list [--parent <id>] --status in_progress` -- in-progress items needing continuation.
- `ctxmod work show <id>` plus `ctxmod work list --owner <id> --kind report,intent` -- per-item state, report paths, and intent path for active items.

`work_store_first` in the ctxmod builtin `work_discipline` profile

Extract from source reports and session context:

- Current work state and stopping point
- Pending items not yet addressed
- Decisions made with rationale
- Assumptions and constraints
- Open questions requiring resolution
- Known issues and workarounds

### Phase 2: Context Preservation

Capture implicit knowledge: document decisions with reasoning, note workarounds and their triggers, make assumptions explicit, list external dependencies, document unexpected behaviors encountered.

### Phase 3: Prioritization

Order by continuation urgency:

1. Immediate next steps
2. Blocking issues requiring resolution
3. Background context for decisions
4. Reference materials for depth

### Phase 4: Summary Production

`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/summary_production.md`

Write summary with handoff structure. Reserve your report path just-in-time (`ctxmod work new report "<title>"`) and output there.

## Input Requirements

- Reports from the session or period being handed off
- Work items in progress
- Session notes or logs (if available)
- Prior handoff summaries for continuity

## Tool Permissions

**Allowed**:
- Read: Source reports, work items, session context, prior handoffs
- Glob: Report and artifact discovery
- Write: reserved report path only

**Forbidden**:
- Modifying source materials
- Launching sub-agents
- Writing outside the reserved report path
- Continuing work (capture state, do not extend)

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Vague state description | Cannot locate continuation point | Specific file, line, and action |
| Missing rationale | Cannot understand past choices | Document why, not just what |
| Undocumented workarounds | Successor hits same issues | Note temporary solutions |
| Implicit assumptions | Hidden context gaps | Make assumptions explicit |
| Exhaustive history | Buries actionable items | Focus on continuation needs |
