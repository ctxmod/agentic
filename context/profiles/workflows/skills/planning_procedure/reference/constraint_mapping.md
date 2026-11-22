# Constraint Mapping Procedure

Identify boundaries that limit the feasible solution space, classify each constraint by rigidity, and verify the plan satisfies all constraints. This procedure produces a constraint inventory with classification, interaction analysis, and satisfaction evidence for each entry.

## Prerequisites

- Planned approach defined or under evaluation
- Access to resource availability, timeline, and capability information
- Stakeholder requirements or external mandates documented
- Discovery and analysis reports available for constraint extraction

## Steps

### 1. Extract Constraints from Sources

Survey all inputs for statements that restrict the solution space:

| Source Type | Constraint Signals |
|-------------|-------------------|
| Analysis reports | "must", "cannot", "limited to", "requires" |
| Stakeholder input | Deadlines, budgets, approval gates |
| Technical environment | Capacity limits, compatibility requirements |
| Organizational policy | Standards, compliance mandates, access controls |
| Prior commitments | Dependencies on other initiatives, existing contracts |

Record each constraint with its source attribution. Vague restrictions ("should be fast") require clarification before they qualify as constraints.

### 2. Classify Constraints by Rigidity

Assign each constraint to one of three categories:

| Category | Definition | Response |
|----------|------------|----------|
| **Hard** | Immovable boundary; violating it invalidates the plan | Design around it; no trade-off permitted |
| **Soft** | Preferred boundary; violating it degrades quality or increases cost | Optimize toward it; accept trade-off with documented rationale |
| **Assumed** | Inferred boundary not explicitly stated; may dissolve under scrutiny | Validate with stakeholder or evidence before treating as hard or soft |

Classification depends on authority and consequence. A regulatory deadline is hard. A preference for a particular tool is soft. An unstated belief about team capacity is assumed.

### 3. Build the Constraint Inventory

Consolidate findings into a structured table:

```markdown
| ID | Constraint | Category | Source | Impact if Violated |
|----|-----------|----------|--------|-------------------|
| K1 | Delivery by Q3 end | Hard | Project charter | Initiative cancelled |
| K2 | Budget under $50K | Soft | Department allocation | Requires escalation approval |
| K3 | Team has capacity | Assumed | Estimation meeting | Schedule slips if incorrect |
| K4 | Compatible with existing API | Hard | Integration requirement | Breaks downstream consumers |
```

Use sequential IDs (K1, K2, ...) for cross-referencing in later planning steps. The "Impact if Violated" column forces explicit reasoning about each constraint's consequence.

### 4. Analyze Constraint Interactions

Examine pairs and groups of constraints for three interaction types:

**Reinforcing**: Constraints that support each other. K1 (deadline) and K2 (budget) together may limit scope in compatible ways.

**Conflicting**: Constraints that resist simultaneous satisfaction. A quality standard (hard) combined with a compressed timeline (hard) may require scope reduction to resolve the tension.

**Dependent**: One constraint's value depends on another. Available budget (K2) may shift based on delivery timeline (K1).

Document interactions explicitly. Conflicting hard constraints require resolution before planning proceeds -- they indicate an infeasible plan if left unaddressed.

### 5. Determine the Feasible Space

Map the solution space that satisfies all hard constraints simultaneously:

1. Start with the full set of possible approaches
2. Eliminate options that violate any hard constraint
3. Score remaining options against soft constraints
4. Flag approaches that rely on assumed constraints

If no options survive hard constraint filtering, the constraint set is infeasible. Return to stakeholders with the specific conflict requiring resolution.

### 6. Verify Plan Satisfaction

For each constraint in the inventory, document how the plan addresses it:

| ID | Constraint | Satisfaction Method | Evidence |
|----|-----------|-------------------|----------|
| K1 | Delivery by Q3 end | Schedule shows completion 2 weeks before deadline | Execution plan step 4 |
| K2 | Budget under $50K | Estimated cost $38K including contingency | Cost breakdown section |
| K3 | Team has capacity | Confirmed 2 FTE available through Q3 | Resource allocation table |
| K4 | Compatible with existing API | Design uses existing endpoints, no breaking changes | Dependency analysis K4 |

Every hard constraint requires explicit satisfaction evidence. Soft constraints require either satisfaction evidence or documented rationale for the accepted trade-off.

## Decision Points

**Assumed constraint cannot be validated**: Treat as soft and plan for the scenario where it holds and where it does not. Document both paths. Revisit when validation becomes possible.

**Two hard constraints conflict**: Escalate to the authority that imposed them. Present the conflict with evidence showing simultaneous satisfaction is impossible. Do not resolve hard constraint conflicts by downgrading one to soft without stakeholder agreement.

**Soft constraint violation improves overall outcome**: Accept the trade-off when the net benefit is clear. Document which soft constraint is violated, what improvement results, and who approved the deviation.

**New constraint discovered mid-planning**: Add it to the inventory, classify it, and re-evaluate constraint interactions. If the new constraint invalidates the current approach, reassess feasibility before continuing.

**Constraint inventory grows beyond 15 items**: Group related constraints into clusters (timeline, resource, technical, policy). Analyze interactions within clusters first, then between clusters. Large inventories analyzed pairwise become intractable.

## Success Criteria

- All constraints extracted from available sources with attribution
- Each constraint classified as hard, soft, or assumed
- Constraint interactions documented (reinforcing, conflicting, dependent)
- Feasible solution space identified or infeasibility reported with specific conflict
- Plan satisfaction verified for every hard constraint with evidence
- Soft constraint trade-offs documented with rationale

## Common Issues

**Treating soft constraints as hard**: Over-constraining the solution space eliminates viable approaches. Verify each hard classification by asking: "Would violating this actually invalidate the plan, or would it reduce quality?" Only immovable boundaries deserve hard status.

**Ignoring assumed constraints**: Unvalidated assumptions become invisible risks. Assumed constraints that prove false mid-execution force unplanned adaptation. Validate early or plan for both outcomes.

**Constraint drift during execution**: Constraints established at planning time may shift. Timelines extend, budgets adjust, requirements evolve. Include a re-evaluation trigger in the plan -- a scheduled point where constraint currency is verified.

**Incomplete source survey**: Missing a constraint source leads to blind spots. Cross-reference at least three source types (stakeholder, technical, organizational) before finalizing the inventory.

**Conflict avoidance**: Conflicting hard constraints get quietly downgraded to avoid difficult conversations. This defers the problem to execution, where the cost of resolution multiplies. Surface conflicts during planning.
