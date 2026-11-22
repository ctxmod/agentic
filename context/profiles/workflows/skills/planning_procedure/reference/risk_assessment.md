# Risk Assessment Procedure

Identify execution risks, assess their probability and impact, define mitigations, and establish contingency plans. This procedure produces a prioritized risk register that informs plan structure and checkpoint placement.

## Prerequisites

- Planned approach selected with execution steps defined
- Access to historical data on similar work (past incidents, failure modes, lessons learned)
- Stakeholder input on risk tolerance and acceptable residual risk
- Dependency map available (identifies interaction points where failures propagate)

## Steps

### 1. Identify Risks

Enumerate risks across five categories:

| Category | Description | Examples |
|----------|-------------|----------|
| Technical | Failures in tools, processes, or artifacts | Incompatible formats, missing dependencies, corrupted state |
| Scope | Changes to boundaries or requirements | Scope expansion, discovered prerequisites, shifting priorities |
| Dependency | Failures in upstream or downstream elements | External system unavailability, blocked prerequisites, stale inputs |
| Resource | Insufficient capacity or capability | Time pressure, skill gaps, concurrent demands |
| Environmental | External factors beyond direct control | Infrastructure changes, policy updates, third-party behavior |

For each risk, record:
- **Description**: What could go wrong (specific and observable)
- **Category**: One of the five above
- **Trigger**: Condition or event that causes the risk to materialize
- **Affected elements**: Which plan steps, artifacts, or outcomes the risk threatens

### 2. Assess Probability

Rate each risk on a five-level scale:

| Level | Label | Definition | Indicator |
|-------|-------|------------|-----------|
| 5 | Almost certain | Expected to occur during execution | Historical precedent in similar work |
| 4 | Likely | Will probably occur | Has occurred in related contexts |
| 3 | Possible | May occur under certain conditions | Known contributing factors present |
| 2 | Unlikely | Could occur but not expected | Requires unusual circumstances |
| 1 | Rare | Requires exceptional circumstances | No precedent in similar work |

Base assessments on evidence: historical frequency, contributing factor analysis, and dependency complexity. Avoid gut-feel ratings when data exists.

### 3. Assess Impact

Rate each risk on a five-level scale:

| Level | Label | Definition | Recovery |
|-------|-------|------------|----------|
| 5 | Severe | Prevents completion of the planned objective | Requires fundamental replanning |
| 4 | Major | Significantly degrades outcomes or timelines | Requires partial replanning and rework |
| 3 | Moderate | Causes measurable setback but remains recoverable | Requires targeted corrective action |
| 2 | Minor | Creates inconvenience without altering outcomes | Handled through existing checkpoints |
| 1 | Negligible | Detectable but does not affect execution | Resolved through normal process |

Impact assessment considers both direct effects (on the immediate step) and cascading effects (on downstream steps through dependency chains).

### 4. Calculate Risk Priority

Combine probability and impact to assign priority:

```
Risk Score = Probability x Impact
```

Map scores to priority levels:

| Score Range | Priority | Response |
|-------------|----------|----------|
| 15-25 | Critical | Mitigate before execution begins |
| 8-14 | High | Mitigate during planning; monitor actively |
| 4-7 | Medium | Accept with monitoring; mitigate if cost-effective |
| 1-3 | Low | Accept; document for awareness |

### 5. Define Mitigations

For each risk at Medium priority or above, define one or more mitigations:

| Mitigation Type | When to Use | Example |
|-----------------|-------------|---------|
| Avoidance | Risk can be eliminated by changing approach | Choose alternative execution path |
| Reduction | Probability or impact can be decreased | Add validation step before risky operation |
| Transfer | Another party can absorb the risk | Delegate to specialist, use managed service |
| Acceptance | Mitigation cost exceeds risk cost | Document risk and monitor |

Each mitigation entry includes:
- **Action**: Specific step to take
- **Timing**: When the mitigation applies (before execution, during specific step, at checkpoint)
- **Owner**: Who is responsible for executing the mitigation
- **Residual risk**: Risk level remaining after mitigation applied

### 6. Create Contingency Plans

For risks rated Critical or High, define contingency plans that activate when the risk materializes despite mitigation:

```markdown
### Contingency: {Risk Description}

**Trigger**: {Observable condition indicating risk has materialized}
**Response**:
1. {Immediate action to contain impact}
2. {Assessment step to determine scope}
3. {Recovery action or escalation path}

**Rollback point**: {Nearest checkpoint to restore from}
**Decision deadline**: {Time or condition by which response must be chosen}
```

### 7. Compile Risk Register

Assemble all assessments into a structured register:

```markdown
## Risk Register

| ID | Risk | Category | P | I | Score | Priority | Mitigation | Owner |
|----|------|----------|---|---|-------|----------|------------|-------|
| R1 | {description} | Technical | 4 | 3 | 12 | High | {summary} | {who} |
| R2 | {description} | Scope | 2 | 5 | 10 | High | {summary} | {who} |
| R3 | {description} | Resource | 3 | 2 | 6 | Medium | {summary} | {who} |
```

Sort by score descending. Group contingency plans after the register for Critical and High items.

## Decision Points

**No historical data available**: Use structured estimation. Identify analogous situations, consult with experienced practitioners, and rate conservatively. Flag assessments as estimate-based for later validation.

**Risk scores cluster at boundaries**: When a score falls on a priority boundary (e.g., score of 8 between Medium and High), consider the risk category. Technical and dependency risks warrant rounding up; resource risks with flexible timelines may round down.

**Mitigation costs exceed risk impact**: Accept the risk. Document the cost-benefit analysis so the acceptance decision is traceable. Re-evaluate if conditions change.

**Stakeholders disagree on risk tolerance**: Use the most conservative tolerance level. Document dissenting positions with rationale. Revisit after initial execution produces data.

**New risks discovered during execution**: Add to the register with current assessment. If the new risk is Critical, pause execution at the nearest checkpoint and update the plan before proceeding.

**Cascading risks identified**: When one risk materializing triggers others, assess the cascade as a compound risk with combined impact. Mitigate the root risk first; document the chain for monitoring.

## Success Criteria

- All identified risks recorded with category, trigger, and affected elements
- Probability and impact assessed on defined scales with supporting rationale
- Risk scores calculated and priority levels assigned
- Mitigations defined for all Medium, High, and Critical risks
- Contingency plans documented for all Critical and High risks
- Risk register compiled, sorted by priority, and integrated into execution plan

## Common Issues

**Vague risk descriptions**: "Something might go wrong" provides no basis for assessment. Require observable descriptions: what fails, under what conditions, with what visible effect.

**Uniform probability ratings**: All risks rated as "Possible" (3) suggests insufficient analysis. Differentiate using historical data, dependency complexity, and contributing factor count.

**Impact assessed in isolation**: Evaluating each risk independently misses cascading failures. After individual assessment, review the register for risks that share affected elements or dependency chains.

**Mitigation without ownership**: Unassigned mitigations do not execute. Every mitigation needs a responsible party and a timing commitment, even when that party is the plan executor.

**Static risk register**: Risks change as execution progresses. Schedule register reviews at each checkpoint. Remove resolved risks, update probability for evolving conditions, and add newly discovered risks.

**Over-mitigation of low risks**: Applying mitigations to every identified risk inflates plan complexity. Accept low-priority risks explicitly rather than adding unnecessary process.
