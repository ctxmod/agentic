# Risk-Aware Planning Objective

Plan execution strategies for high-risk changes by identifying threats, assessing their probability and impact, defining mitigations, and establishing contingency paths. Risk-aware planning produces plans where every significant risk has a documented response.

## Base Behavior

the `planning_objective` skill

## Scope

Risk-aware planning applies when execution carries elevated uncertainty or consequence:

- Safety-critical changes where failure affects data integrity or availability
- Irreversible modifications lacking straightforward rollback
- Cross-domain changes affecting multiple artifact types or systems
- Changes with external dependencies outside direct control
- High-value artifacts where defects carry disproportionate cost
- Novel approaches with limited precedent in the project

Standard planning suffices when risks are well-understood and mitigations are routine.

## Core Behavior

1. **Identify**: Catalog risks from analysis reports, dependency maps, and execution context
2. **Classify**: Categorize each risk by source (technical, process, dependency, environmental)
3. **Assess**: Score probability and impact for each risk using consistent scales
4. **Mitigate**: Define preventive actions that reduce probability or limit impact
5. **Contingency**: Establish response plans for risks that materialize despite mitigation
6. **Sequence**: Order execution steps to surface high-risk elements early
7. **Report**: Document the risk-aware plan at the reserved report path

## Relationship to Other Planning Types

| Type | Focus | Risk Handling |
|------|-------|---------------|
| General | Single-approach execution | Implicit assumptions |
| Phased | Multi-phase with gates | Gate criteria may include risk |
| Incremental | Iterative with feedback | Adapts as risks emerge |
| **Risk-Aware** | **Explicit risk management** | **Systematic identification and mitigation** |
| Constraint-Based | Feasibility under limits | Constraints as fixed boundaries |

Risk-aware planning differs from phased planning: phased plans organize work into stages; risk-aware plans organize work around threat management. A plan can be both phased and risk-aware.

## Risk Identification

Sources for risk discovery:

- **Analysis reports**: Findings flagged as uncertain, conflicting, or high-impact
- **Dependency analysis**: External systems, shared resources, ordering constraints
- **Change scope**: Breadth of modification, number of affected artifacts
- **Historical precedent**: Similar past changes and their outcomes
- **Assumption inventory**: Unstated beliefs that could prove wrong

Record each risk with a concise description, source attribution, and affected plan steps.

## Risk Assessment

Score each risk on two dimensions:

| Level | Probability | Impact |
|-------|-------------|--------|
| High | Likely to occur based on evidence | Blocks execution or causes significant rework |
| Medium | Possible given current conditions | Delays execution or degrades quality |
| Low | Unlikely without unexpected changes | Minor inconvenience, easily absorbed |

Combine dimensions to determine response priority:

| | High Impact | Medium Impact | Low Impact |
|---|-------------|---------------|------------|
| **High Probability** | Mitigate + contingency | Mitigate | Accept with monitoring |
| **Medium Probability** | Mitigate | Accept with monitoring | Accept |
| **Low Probability** | Contingency only | Accept | Accept |

`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/risk_assessment.md`

## Mitigation Strategies

Each mitigation reduces either probability or impact:

- **Avoidance**: Restructure the plan to eliminate the risk source
- **Reduction**: Add verification steps, checkpoints, or safeguards
- **Transfer**: Shift risk to a component better equipped to handle it
- **Early exposure**: Sequence risky steps first to fail fast
- **Incremental execution**: Break risky steps into smaller, verifiable units

Document the cost of each mitigation. Mitigations that cost more than the risk they address warrant reconsideration.

## Pre-Planning

Before constructing the risk-aware plan:

1. Read analysis reports and identify findings with uncertainty flags
2. Map dependencies between planned changes and external systems
3. Review past reports for similar changes and their outcomes
4. Inventory assumptions that the plan relies on
5. Reserve the report path just-in-time (`ctxmod work new report "<title>"`)
6. Gather rollback capabilities and constraints for the target environment

## During Planning

Apply risk awareness throughout plan construction:

- **Front-load uncertainty**: Schedule high-risk steps early when reversal is cheaper
- **Define checkpoints**: Place verification points after each risky step
- **Bound blast radius**: Isolate risky changes to limit cascading failure
- **Preserve rollback paths**: Ensure each step can be undone until the next checkpoint confirms success
- **Document residual risk**: State what remains after mitigation and why it is acceptable

## Contingency Plans

For each risk rated as needing contingency:

1. Define the trigger condition that activates the contingency
2. Specify the response actions in execution order
3. Identify resources or access required for the response
4. Estimate time cost of executing the contingency
5. Document the recovery state after contingency completes

Contingency plans are pre-decisions. They eliminate deliberation under pressure.

## Verification

After completing the risk-aware plan:

1. Confirm every identified risk has an assigned response (mitigate, contingency, or accept)
2. Verify checkpoints exist after each high-risk step
3. Check that mitigation costs do not exceed the risks they address
4. Validate contingency triggers are specific and observable
5. Ensure the plan remains executable if no risks materialize

## Tool Permissions

**Allowed**: Read (reports, reference), Grep (risk pattern search), Glob (artifact discovery), Write (reserved report path only)

**Forbidden**: Modifying source artifacts, launching sub-agents, writing outside the reserved report path

## Response Format

After writing the plan, return:

```
Wrote risk-aware plan to <reserved report path>.

Summary: [2-3 sentences covering approach, primary risks, and key mitigations]

Risks identified: [count by priority level]
```

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Listing risks without responses | Incomplete planning | Assign mitigation, contingency, or explicit acceptance |
| Equal treatment of all risks | Wasted effort on low-priority items | Prioritize by probability and impact |
| Mitigations more costly than risks | Negative value | Compare mitigation cost against expected risk cost |
| Vague contingency triggers | Delayed response | Define specific, observable trigger conditions |
| Omitting residual risk | False confidence | State what remains after mitigation |
| Risk assessment without evidence | Arbitrary scores | Ground assessments in analysis findings |
| Planning only for failure | Pessimistic paralysis | Plan the execution path with risk checkpoints |

## Examples

**Documentation Restructure with Cross-References**

Risks: broken cross-references after reorganization (high probability, high impact), content loss during migration (low probability, high impact), inconsistent terminology across merged sections (medium probability, medium impact). Mitigations: build complete reference map before moves, verify content checksums after each batch, run terminology audit per section. Contingency: restore from version control if reference validation fails post-move.

**Research Methodology Change**

Risks: data incompatibility between old and new collection methods (medium probability, high impact), gaps in coverage during transition period (medium probability, medium impact). Mitigations: run parallel collection using both methods for one cycle, document mapping between old and new data formats. Contingency: extend parallel period and reconcile discrepancies before committing to new method.

**Specification Compliance Remediation**

Risks: remediation of one requirement conflicts with another (high probability, medium impact), scope expansion as hidden non-compliance surfaces (medium probability, medium impact). Mitigations: map requirement interdependencies before changes, address isolated requirements first. Contingency: revert conflicting changes and escalate requirement conflicts for resolution before proceeding.

**Configuration Format Migration**

Risks: downstream consumers fail to parse new format (medium probability, high impact), environment-specific variations missed during conversion (low probability, high impact). Mitigations: validate new format against all known consumers, enumerate environment variants from inventory. Contingency: maintain old format as fallback until all consumers confirm compatibility.

## Related

the `planning_objective` skill

`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/risk_assessment.md`

`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/checkpoint_definition.md`

`ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/dependency_analysis.md`

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
