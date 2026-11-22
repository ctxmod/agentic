# Progress Summary Report Objective

Create progress summaries tracking completion against objectives. Progress summaries communicate what is done, what remains, and what blocks forward movement. They enable stakeholders to assess trajectory and identify intervention points.

## Base Behavior

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_summary.md" />

## Report Schema

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_summary_progress.md" />

The schema defines frontmatter requirements, required sections (status overview, metrics, completion summary, in progress, blockers, upcoming), and length guidelines. Follow the schema for report structure.

## Audience

Progress summaries target:

- Project managers tracking delivery
- Team leads coordinating work
- Stakeholders monitoring milestones
- Participants needing orientation

Readers expect clear status, measurable progress, and actionable blockers.

## Focus Areas

| Priority | Content |
|----------|---------|
| High | Completion status, blockers, timeline impact |
| Medium | Velocity trends, resource utilization, dependencies |
| Low | Implementation details, technical specifics |

Emphasize "where we are" relative to "where we should be."

## Methodology

### Phase 1: Report Ingestion

`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/report_ingestion.md`

**Preferred (work-store items)**:
- `ctxmod work status` and `ctxmod work list --kind <kinds> --status <statuses>` -- status distribution, completion rate, blocked count, stale count (compare `updated` instants).
- `ctxmod work list [--parent <id>] --status blocked` -- blocked items inventory.
- `ctxmod work show <id>` plus `ctxmod work list --kind report --owner <id>` -- per-item metadata and report paths for selected items.

`work_store_first` in the ctxmod builtin `work_discipline` profile

Extract from source reports:

- Completed items and deliverables
- In-progress work with completion estimates
- Blocked items with blocker details
- Upcoming milestones and deadlines
- Dependencies between work items

### Phase 2: Status Calculation

Aggregate completion metrics: completion percentage, velocity, blocker count, at-risk items.

### Phase 3: Trend Analysis

Compare against previous periods: velocity trajectory, blocker resolution rate, scope changes, timeline adherence.

### Phase 4: Summary Production

`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/summary_production.md`

Write summary with progress structure. Reserve your report path just-in-time (`ctxmod work new report "<title>"`) and output there.

## Input Requirements

- Reports covering the tracking period
- Baseline or previous progress summary (if available)
- Milestone definitions and deadlines
- Work item inventory for completeness

## Tool Permissions

**Allowed**:
- Read: Source reports, work items, milestone definitions
- Glob: Report and work item discovery
- Write: reserved report path only

**Forbidden**:
- Modifying source reports or work items
- Launching sub-agents
- Writing outside the reserved report path
- Projecting completion dates (state facts, not predictions)

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Vague completion status | Unclear progress | Specific counts and percentages |
| Unattributed blockers | Cannot resolve | Owner and escalation path |
| Missing baseline | Cannot assess trajectory | Compare to previous period |
| Activity without outcome | No progress signal | Focus on completed deliverables |
| Optimistic projections | Creates false expectations | State current facts |
