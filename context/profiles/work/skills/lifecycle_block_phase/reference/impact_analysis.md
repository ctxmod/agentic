# Impact Analysis Phase

Analyze how a work item lifecycle state change affects parent, child, and dependent work items. This phase identifies cascade effects and validates hierarchy consistency.

## When to Use

Use impact analysis phase when:

- Work item status change may affect related items
- Blocking a parent item affects child execution
- Completing a parent requires child resolution verification
- Hierarchy integrity must be validated after transition

Impact analysis follows transition verification and precedes or accompanies status transitions.

Preferred when available: `ctxmod work show <id>` plus `ctxmod work list --parent <id>` for relationship discovery. Full content still needed for assessment. Retain `find`-based resolution as active fallback (safety-critical phase). See `work_store_first` in the ctxmod builtin `work_discipline` profile.

Brief template for a sub-agent (model: haiku):

```text
Analyze {transition type} impact on related work items for {WORK_ITEM_ID}.

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{transition}-impact" --owner <work item, if any>` immediately before writing; returns the path
- WORK_ITEM_PATH={work item directory}
- WORK_ITEM_TYPE={epic|story|issue|task}
- TRANSITION_TYPE={active-to-blocked|active-to-completed|any-to-cancelled}
- VERIFICATION_REPORT={verification report path}

Additional context:
- {transition-specific scope reference}

Delimitations:
- Analysis only; do not modify any work items
- Identify affected items within hierarchy
- Assess cascade effects

Notes:
- Read work item file to identify parent/child relationships
- Check status of related items
- Determine which items are impacted
- Assess severity of impact
```

## Analysis Scopes by Transition Type

### Active to Blocked (Block)

**Analysis scope**: Children and dependents

**Impacts**:
- Child items cannot proceed while parent blocked
- Dependent items in other branches may be affected
- Sibling items may need to compensate for blocked work

**Assessment**:
- List all child items with their current status
- Identify which children are actively in-progress
- Note any children that should be blocked as cascade effect
- Identify external dependencies affected by block

### Active to Completed (Complete)

**Analysis scope**: Parent chain

**Impacts**:
- Parent item may become eligible for completion
- Sibling completion status affects parent eligibility

**Assessment**:
- Check parent work item status
- Determine if all parent's children are now resolved
- Assess parent completion eligibility
- Identify any remaining blockers for parent

### Any to Cancelled (Cancel)

**Analysis scope**: Children and parent

**Impacts**:
- Child items will be orphaned (parent reference invalid)
- Parent item loses child contribution
- Sibling items may have dependency gaps

**Assessment**:
- List all child items
- Determine cancellation strategy for children (cascade vs orphan)
- Check if parent has other active children
- Assess whether parent can proceed without this child

### Draft to Active, Blocked to Active (Start)

**Analysis scope**: None (optional phase)

Start transitions typically do not require impact analysis unless the work item has children. Skip this phase for start transitions on items without children.

## Context Requirements

**Parameters provided by main agent**:

| Parameter | Required | Purpose |
|-----------|----------|---------|
| Report (reserved) | Yes | Sub-agent creates the report record just-in-time with `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing |
| WORK_ITEM_PATH | Yes | Work item directory |
| WORK_ITEM_TYPE | Yes | Work item type (epic, story, issue, task) |
| TRANSITION_TYPE | Yes | Transition variant being analyzed |
| VERIFICATION_REPORT | Conditional | Verification report if available |

## Output Format

Impact analysis produces an analysis report:

```markdown
# Impact Analysis: {Transition Type}

## Work Item
{ID}: {Title} (Current: {status}, Transitioning to: {target status})

## Affected Items

### Parent
{Parent ID}: {Title} ({current status})
- Impact: {Description of how parent is affected}

### Children
{For each child:}
- {Child ID}: {Title} ({current status})
  - Impact: {Description of how child is affected}

### Dependents
{For each dependent item outside hierarchy:}
- {Dependent ID}: {Title} ({current status})
  - Impact: {Description of how dependent is affected}

## Cascade Effects
{List of secondary effects and recommendations}

## Recommendations
{Suggested actions for affected items}
```

## Transition Criteria

Impact analysis completes when:

- Related work items identified (parent, children, dependents)
- Impact on each related item assessed
- Cascade effects documented
- Recommendations provided
- Report written to the reserved report path

If analysis fails to complete, report the issue with failure context.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Analyzing without transition type | Generic analysis misses impacts | Always specify transition type |
| Modifying work items during analysis | Violates analysis-only scope | Analysis reports; modifications execute separately |
| Ignoring external dependents | Incomplete impact picture | Check cross-hierarchy dependencies |
| Using Sonnet/Opus | Wastes capability on analysis | Use Haiku for relationship mapping |
