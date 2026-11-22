# Gap Analysis Objective

Compare current state against desired state to identify missing elements, quality deficiencies, and improvement opportunities. Gap analysis transforms discovery findings into prioritized remediation roadmaps.

## Base Behavior

the `analysis_objective` skill

## Gap Analysis Focus

### Current vs Desired State

Establish baseline through discovery reports. Define target through requirements, reference models, or specification documents. The gap exists in the delta between these states.

### Gap Categorization

| Category | Focus |
|----------|-------|
| Feature Gaps | Missing functionality, unsupported user flows |
| Quality Gaps | Technical debt, architecture trade-offs, code quality |
| Process Gaps | DevOps maturity, missing feedback loops |
| Documentation Gaps | Tutorials, how-tos, references, explanations |

### Impact Assessment

Score each gap across three dimensions: business value, risk severity, and frequency. See procedure for scoring scales.

## Methodology

### Phase 1: State Definition

Document current state from discovery reports. Document desired state from requirements or reference models. Use consistent granularity across both.

### Phase 2: Gap Identification

Systematically compare states. Categorize each gap (feature, quality, process, documentation). Record evidence with source attribution (path:line).

### Phase 3: Impact Assessment

Brief template for a procedure-depth agent (launched from objective depth):

```text
Score each identified gap across business value, risk severity, and frequency using the three-dimensional impact criteria.
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/impact_scoring.md`
```

Score each gap using the three-dimensional impact criteria. Estimate effort for remediation. Flag dependencies between gaps.

### Phase 4: Prioritization

Brief template for a procedure-depth agent (launched from objective depth):

```text
Apply the impact/effort matrix to the scored gaps to generate a prioritized remediation roadmap.
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/prioritization.md`
```

Apply impact/effort matrix to generate prioritized remediation roadmap.

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Create the gap analysis report by organizing the prioritized gaps and writing the structured report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

### Gap-Specific Sections

Beyond standard report structure, gap reports include:

- **Current State**: Baseline inventory from discovery
- **Desired State**: Target model or requirements
- **Gaps by Category**: Feature, quality, process, and documentation gaps with evidence
- **Prioritization Matrix**: Gaps plotted by impact/effort

## Tool Permissions

**Allowed**: Read (reports, reference), Grep (verification), Glob (file discovery), Write (reserved report path only)

**Forbidden**: Modifying source files, launching sub-agents, writing outside the reserved report path

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Listing without prioritization | Unclear next steps | Apply impact/effort matrix |
| Skipping impact assessment | Arbitrary priorities | Score by value, risk, frequency |
| Vague gap descriptions | Unactionable | Include evidence and specific examples |
| Missing remediation paths | Incomplete analysis | Recommend concrete next steps |
| Treating all gaps equally | Resource waste | Categorize and prioritize explicitly |

## Examples

**Objective Gap Analysis**

Current: Agent writing workflow has 8 available objectives. Desired: 12 objectives required per spec. Gap: 4 missing objectives (validation-source, analysis-comparative, discovery-patterns, generation-template). Prioritize validation-source (high impact, low effort).

**Documentation Coverage Gap**

Current: API reference exists for 60% of endpoints. Desired: 100% coverage with examples. Gap: 40% endpoints undocumented, zero have examples. Quick win: Add examples to existing docs. Strategic: Complete missing references.

**Process Maturity Gap**

Current: Manual deployment with SSH. Desired: CI/CD with automated rollback. Gaps: No pipeline definition, no artifact versioning, no rollback mechanism. Recommend incremental adoption: pipeline first, versioning second, rollback third.

## Related

`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_before_execution.md" />

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
