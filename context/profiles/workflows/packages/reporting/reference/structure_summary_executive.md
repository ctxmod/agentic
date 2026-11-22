# Executive Summary Schema

Executive summaries target stakeholders requiring high-level overview without technical depth. They distill findings into outcome-focused content, highlight decisions needed, and communicate risks at appropriate abstraction levels.

## Base Schema

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_summary.md" />

## Frontmatter

```yaml
---
type: summary
variant: executive
date: YYYY-MM-DD
time: HH-mm
source: "{context-path or 'general'}"
objective: "{objective-name}"
scope: "{summary scope}"
status: "{on track | at risk | blocked}"
---
```

| Field | Required | Description |
|-------|----------|-------------|
| type | Yes | Always `summary` |
| variant | Yes | Always `executive` |
| status | Yes | Overall status: on track, at risk, or blocked |

Additional fields from base summary schema apply.

## Audience

Executive summaries target:

- Non-technical stakeholders
- Project sponsors and leadership
- Decision-makers requiring status overview
- Cross-functional team leads

Readers expect outcomes and implications without implementation details.

## Content Priority

| Priority | Include | Exclude |
|----------|---------|---------|
| High | Outcomes, decisions needed, blockers | Technical methodology |
| Medium | Progress metrics, resource implications | Implementation specifics |
| Low | Background context when needed | Detailed findings |

Emphasize "what" and "so what"—not "how."

## Required Sections

### Status Summary

2-3 sentences stating overall status:

```markdown
## Status Summary

**Status**: {On Track | At Risk | Blocked}

{Brief explanation of status with key indicator}
```

### Key Outcomes

Accomplished items in stakeholder terms:

```markdown
## Key Outcomes

- {Outcome in business language}
- {Outcome with impact}
- {Outcome with stakeholder relevance}
```

### Decisions Needed

Action items requiring stakeholder input:

```markdown
## Decisions Needed

### {Decision Title}

**Context**: {why this decision is needed}

**Options**:
- Option A: {description with implications}
- Option B: {description with implications}

**Recommendation**: {suggested option with brief rationale}

**Timeline**: {when decision is needed}
```

### Risks and Blockers

Items threatening objectives:

```markdown
## Risks and Blockers

### {Risk or Blocker Title}

- **Impact**: {what is affected}
- **Severity**: High | Medium | Low
- **Mitigation**: {current status or action}
```

### Next Steps

Upcoming focus areas:

```markdown
## Next Steps

1. {Near-term priority}
2. {Milestone or deliverable}
3. {Dependency or coordination need}
```

## Business Language Translation

Transform technical findings into stakeholder-accessible language:

| Technical | Executive |
|-----------|-----------|
| System latency exceeds threshold | User experience degraded |
| Test coverage insufficient | Quality assurance incomplete |
| Dependency conflict | Delivery timeline at risk |
| Resource contention | Team capacity constrained |

Preserve accuracy while increasing accessibility.

## Example

```markdown
---
type: summary
variant: executive
date: {date}
time: {time}
source: general
objective: summary
scope: Q2 initiative status
status: at risk
---

# Q2 Initiative Executive Summary

## Status Summary

**Status**: At Risk

Three of five deliverables complete. Integration phase blocked pending external input. Timeline achievable if blocker resolves within 5 days.

## Key Outcomes

- Core functionality delivered and operational
- Documentation complete and approved
- Stakeholder review completed with positive feedback

## Decisions Needed

### Resource Allocation for Phase 2

**Context**: Phase 2 requires specialized expertise not currently available.

**Options**:
- Option A: External engagement (higher cost, faster timeline)
- Option B: Internal training (lower cost, delayed timeline)

**Recommendation**: Option A based on deadline constraints

**Timeline**: Decision needed by end of week

## Risks and Blockers

### External Dependency Unresolved

- **Impact**: Integration phase blocked
- **Severity**: High
- **Mitigation**: Escalated to partner organization; daily follow-up active

### Team Capacity Constrained

- **Impact**: Parallel work limited
- **Severity**: Medium
- **Mitigation**: Prioritization meeting scheduled

## Next Steps

1. Resolve integration blocker
2. Begin Phase 2 planning with resource decision
3. Schedule stakeholder review for completed deliverables
```

## Length Guidelines

| Metric | Target |
|--------|--------|
| Word count | 200-400 words |
| Maximum | 500 words |
| Page equivalent | One page when formatted |

## Validation Checklist

- [ ] Status clearly stated in frontmatter and body
- [ ] Content uses business language, not technical jargon
- [ ] Decisions include options and recommendations
- [ ] Risks include severity and mitigation status
- [ ] Next steps are actionable and time-relevant
- [ ] Total length under 500 words
