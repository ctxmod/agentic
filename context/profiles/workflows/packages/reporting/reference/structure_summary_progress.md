# Progress Summary Schema

Progress summaries track completion against objectives. They communicate what is done, what remains, and what blocks forward movement. Progress summaries enable stakeholders to assess trajectory and identify intervention points.

## Base Schema

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_summary.md" />

## Frontmatter

```yaml
---
type: summary
variant: progress
date: YYYY-MM-DD
time: HH-mm
source: "{context-path or 'general'}"
objective: "{objective-name}"
scope: "{progress scope}"
period: "{tracking period}"
baseline: "{path to previous progress summary if applicable}"
---
```

| Field | Required | Description |
|-------|----------|-------------|
| type | Yes | Always `summary` |
| variant | Yes | Always `progress` |
| period | Yes | Time period covered |
| baseline | Conditional | Previous progress summary for comparison |

Additional fields from base summary schema apply.

## Audience

Progress summaries target:

- Project managers tracking delivery
- Team leads coordinating work
- Stakeholders monitoring milestones
- Participants needing orientation

Readers expect clear status, measurable progress, and actionable blockers.

## Content Priority

| Priority | Include | Exclude |
|----------|---------|---------|
| High | Completion status, blockers, trajectory | Implementation details |
| Medium | Velocity trends, dependencies | Technical specifics |
| Low | Context when needed for understanding | Exhaustive activity logs |

Emphasize "where we are" relative to "where we should be."

## Required Sections

### Status Overview

Single sentence stating overall status:

```markdown
## Status Overview

**Status**: {On Track | At Risk | Behind | Ahead}

{Completion metric}: {X of Y items (Z%)} against target of {target%} at {milestone}.
```

### Completion Summary

Items completed this period:

```markdown
## Completion Summary

| Item | Completed | Notes |
|------|-----------|-------|
| {Item name} | {date} | {relevant context} |
```

Or as bulleted list:

```markdown
## Completed This Period

- {Completed item with date}
- {Completed item with significance}
```

### In Progress

Active work with current state:

```markdown
## In Progress

### {Work Item}

- **Status**: {current state}
- **Progress**: {X% complete or stage}
- **Blockers**: {blocking issues or "None"}
- **Next**: {immediate next step}
```

### Blockers

Items preventing progress:

```markdown
## Blockers

### {Blocker Title}

- **Affects**: {what work is blocked}
- **Owner**: {responsible party}
- **Age**: {days since identified}
- **Impact**: {consequence if unresolved}
- **Escalation**: {current escalation status}
```

### Upcoming

Next period priorities:

```markdown
## Upcoming

| Priority | Item | Target |
|----------|------|--------|
| 1 | {High priority item} | {milestone or date} |
| 2 | {Next priority item} | {milestone or date} |
```

## Metrics Section

Quantitative measures:

```markdown
## Metrics

| Metric | Current | Target | Status |
|--------|---------|--------|--------|
| Completed | 8 of 15 (53%) | 50% at midpoint | On Track |
| In Progress | 4 items | - | - |
| Blocked | 3 items | 0 | At Risk |
| Velocity | 4/week | 4/week | Stable |
```

## Trend Analysis

Compare against previous periods:

```markdown
## Trend

| Period | Completed | Velocity | Blockers |
|--------|-----------|----------|----------|
| Week 1 | 3 | 3/week | 1 |
| Week 2 | 5 | 5/week | 0 |
| Week 3 | 4 | 4/week | 3 |

{Trend observation: stable, improving, declining}
```

## Example

```markdown
---
type: summary
variant: progress
date: {date_4}
time: 09-00
source: general
objective: summary
scope: Configuration standardization initiative
period: {date_1} to {date_3}
baseline: reports/{date_3}/09-00-summary-progress.md
---

# Configuration Standardization Progress

## Status Overview

**Status**: On Track

Completed 8 of 15 items (53%) against target of 50% at midpoint. Three blockers require attention but do not threaten timeline.

## Metrics

| Metric | Current | Target | Status |
|--------|---------|--------|--------|
| Completed | 8 of 15 (53%) | 50% | On Track |
| In Progress | 4 items | - | - |
| Blocked | 3 items | 0 | Attention |
| Velocity | 4/week | 4/week | Stable |

## Completed This Period

- Discovery phase complete ({date_1})
- Analysis report delivered ({date_2})
- Validation report delivered ({date_2})
- Naming remediation complete ({date_2})

## In Progress

### Documentation Update

- **Status**: Writing in progress
- **Progress**: 60%
- **Blockers**: None
- **Next**: Complete environment precedence section

### Automation Setup

- **Status**: Configuration pending
- **Progress**: 20%
- **Blockers**: Awaiting infrastructure access
- **Next**: Configure validation in pipeline

## Blockers

### Infrastructure Access Pending

- **Affects**: Automation setup
- **Owner**: Platform team
- **Age**: 2 days
- **Impact**: Delays automation by blocked duration
- **Escalation**: Request submitted, awaiting response

## Upcoming

| Priority | Item | Target |
|----------|------|--------|
| 1 | Complete documentation | 2026-05-17 |
| 2 | Resolve infrastructure access | 2026-05-17 |
| 3 | Deploy automation | 2026-05-19 |

## Trend

| Period | Completed | Velocity |
|--------|-----------|----------|
| Week 1 | 4 items | 4/week |
| Week 2 (partial) | 4 items | 4/week |

Velocity stable at target rate.
```

## Length Guidelines

| Metric | Target |
|--------|--------|
| Word count | 300-600 words |
| Maximum | 800 words |
| Format | Scannable with clear status indicators |

## Validation Checklist

- [ ] Status clearly stated with key metric
- [ ] Completion includes specific counts and percentages
- [ ] Blockers include owner, age, and escalation status
- [ ] Metrics section provides quantitative overview
- [ ] Upcoming items prioritized with targets
- [ ] Trend data included when baseline available
- [ ] Total length under 800 words
