# Prioritization Procedure

Apply an impact/effort matrix to rank findings, recommendations, or backlog items and sequence them for execution.

## Prerequisites

- List of items requiring prioritization (findings, recommendations, tasks)
- Stakeholder input on impact criteria (business value, user benefit, risk reduction)
- Effort estimation basis established (time, complexity, resource requirements)

## Steps

### 1. Define Impact Criteria

Establish what constitutes high vs low impact for your context:

- **Business value**: Revenue, cost savings, competitive advantage
- **User benefit**: Satisfaction, efficiency, adoption
- **Risk reduction**: Security, compliance, technical debt

Weight criteria if multiple apply. Document the definition before scoring.

### 2. Estimate Effort

Assess effort for each item using consistent units:

| Effort Level | Time Equivalent | Complexity Indicators |
|--------------|-----------------|----------------------|
| Low          | < 1 week        | Known patterns, minimal dependencies |
| Medium       | 1-4 weeks       | Some unknowns, moderate coordination |
| High         | > 4 weeks       | Significant unknowns, cross-team work |

Use relative sizing (S/M/L) when absolute estimates prove unreliable.

### 3. Plot Items on Matrix

Assign each item to one quadrant:

| Quadrant | Characteristics | Action |
|----------|-----------------|--------|
| Quick Wins | High impact + Low effort | Execute first |
| Strategic Projects | High impact + High effort | Plan as OKRs |
| Fillers | Low impact + Low effort | Execute when blocked |
| Money Pit | Low impact + High effort | Deprioritize or eliminate |

### 4. Sequence by Quadrant

Order execution:

1. Quick Wins (immediate value, fast feedback)
2. Strategic Projects (schedule with proper resourcing)
3. Fillers (opportunistic execution)
4. Money Pit (justify or remove from backlog)

### 5. Resolve Dependencies

Identify items that block others. Promote blockers even if their individual priority is lower. A low-impact item blocking three Quick Wins becomes urgent.

## Decision Points

**Borderline items**: When an item sits between quadrants, apply RICE scoring for precision: `(Reach x Impact x Confidence) / Effort`. Higher scores indicate higher priority.

**Contested impact ratings**: Return to stakeholders with specific questions. Present trade-offs rather than seeking abstract consensus.

**Effort uncertainty**: Use range estimates (2-4 weeks). If range spans two effort levels, assume higher until investigation reduces uncertainty.

**Dependencies create cycles**: Break cycles by identifying the minimum viable subset that unblocks forward progress.

## Success Criteria

- Every item assigned to exactly one quadrant
- Quick Wins identified and scheduled for immediate execution
- Strategic Projects have resource allocation or OKR assignment
- Dependencies mapped and incorporated into sequencing
- Money Pit items justified or removed

## Common Issues

**Inflated impact scores**: Teams rate their own items as high impact. Require metrics or user data supporting claims.

**Effort underestimation**: Technical debt and integration work often overlooked. Add buffer for cross-system items.

**Quadrant gaming**: Items restructured to appear as Quick Wins. Verify that decomposed items remain independently valuable.

**Stale prioritization**: Review matrix when new information emerges or after 2-4 weeks.
