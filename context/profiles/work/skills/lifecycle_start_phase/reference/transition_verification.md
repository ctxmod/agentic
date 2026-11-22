# Transition Verification Phase

Verify that a work item meets the criteria for transitioning between lifecycle states. This phase assesses readiness for status changes without modifying the work item.

## When to Use

Use transition verification phase when:

- Work item status transition is requested
- Transition criteria must be validated before proceeding
- Verification reports guide subsequent analysis and transition phases
- Lifecycle commands validate eligibility

Transition verification precedes status transitions and impact analysis.

Brief template for a sub-agent (model: haiku):

```text
Verify {work item} eligibility for {transition type} transition.

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- WORK_ITEM_PATH={work item directory}
- WORK_ITEM_TYPE={epic|story|issue|task}
- TRANSITION_TYPE={draft-to-active|blocked-to-active|active-to-blocked|active-to-completed|any-to-cancelled}

Additional context:
- {transition-specific criteria reference}

Delimitations:
- Verification only; do not modify work item
- Check transition criteria specific to transition type
- Report clear eligibility determination

Notes:
- Read work item file at {WORK_ITEM_PATH}/{TYPE}.md
- Check current status matches expected from-state
- Validate transition-specific criteria
- Document any blocking issues
```

## Transition Types and Criteria

### Draft to Active (Start)

**Current status**: draft
**Target status**: active

**Criteria**:
- Required sections present (varies by type: Summary, Success Criteria, etc.)
- Parent item status is active or completed (if parent exists)
- No unresolved blockers

### Blocked to Active (Unblock)

**Current status**: blocked
**Target status**: active

**Criteria**:
- Blocker marked as resolved in Blockers section
- Resolution path documented
- No remaining active blockers

### Active to Blocked (Block)

**Current status**: active
**Target status**: blocked

**Criteria**:
- Blocker reason provided
- Blocking condition documented

### Active to Completed (Complete)

**Current status**: active
**Target status**: completed

**Criteria**:
- All acceptance/success criteria met
- All child items resolved (completed or cancelled)
- No unresolved blockers

### Any to Cancelled (Cancel)

**Current status**: any
**Target status**: cancelled

**Criteria**:
- Cancellation reason documented
- Child items acknowledged (will be orphaned or cascade-cancelled)

## Context Requirements

**Parameters provided by main agent**:

| Parameter | Required | Purpose |
|-----------|----------|---------|
| Report (reserved) | Yes | Sub-agent creates the report record just-in-time with `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing |
| WORK_ITEM_PATH | Yes | Work item directory |
| WORK_ITEM_TYPE | Yes | Work item type (epic, story, issue, task) |
| TRANSITION_TYPE | Yes | Transition variant to verify |

## Output Format

Transition verification produces a verification report:

```markdown
# {Transition Type} Verification: {Work Item ID}

## Current Status
{current status}

## Transition Eligibility
{Eligible | Not Eligible}

## Criteria Assessment
{For each criterion:}
- {Criterion}: {Met | Not Met} - {Evidence or explanation}

## Blocking Issues
{List of issues preventing transition, or "None"}

## Recommendations
{If not eligible, what must be addressed}
```

## Transition Criteria

Transition verification completes when:

- Work item file is read and analyzed
- Transition type criteria are checked
- Eligibility determination is clear
- Blocking issues documented
- Report written to the reserved report path

If verification fails to complete, report the issue with failure context.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Verifying without transition type | Generic checks miss criteria | Always specify transition type |
| Modifying work item during verification | Violates verification-only scope | Verification reports; transitions execute separately |
| Skipping criteria documentation | Unclear eligibility rationale | Document each criterion assessment |
| Using Sonnet/Opus | Wastes capability on verification | Use Haiku for criteria checking |
