# Decision Summary Report Objective

Create decision summaries presenting options with trade-offs for informed decision-making. Decision summaries structure information to enable stakeholders to evaluate alternatives, understand implications, and make informed choices.

## Base Behavior

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_summary.md" />

## Report Schema

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_summary_decision.md" />

The schema defines frontmatter requirements, required sections (decision context, options, comparison matrix, risks, recommendation), trade-off dimensions, and length guidelines. Follow the schema for report structure.

## Audience

Decision summaries target:

- Decision-makers evaluating alternatives
- Technical leads choosing approaches
- Stakeholders providing input on direction
- Teams aligning on strategy

Readers expect clear options, objective trade-offs, and sufficient context to decide.

## Focus Areas

| Priority | Content |
|----------|---------|
| High | Options, trade-offs, recommendations |
| Medium | Constraints, risks, reversibility |
| Low | Implementation details, historical context |

Emphasize enabling informed choice over advocating specific outcomes.

## Methodology

### Phase 1: Report Ingestion

`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/report_ingestion.md`

Extract from source reports:

- Options identified through analysis
- Criteria for evaluation
- Trade-offs and constraints
- Risks associated with each option
- Recommendations with rationale

### Phase 2: Option Structuring

Organize options for comparison: clear option names, descriptions, advantages, disadvantages, risks, and reversibility.

### Phase 3: Trade-off Analysis

Create comparable dimensions: cost, quality, risk, timeline, flexibility. Use consistent assessment scales across options.

### Phase 4: Summary Production

`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/summary_production.md`

Write summary with decision structure. Reserve your report path just-in-time (`ctxmod work new report "<title>"`) and output there.

## Input Requirements

- Analysis reports containing options evaluation
- Comparative analysis (if available)
- Constraint documentation
- Stakeholder priorities (if documented)

## Tool Permissions

**Allowed**:
- Read: Source reports, constraint documentation, prior decisions
- Glob: Report discovery
- Write: reserved report path only

**Forbidden**:
- Modifying source reports
- Launching sub-agents
- Writing outside the reserved report path
- Making the decision (present options, not conclusions)

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Single option presentation | Not a real decision | Multiple viable alternatives |
| Missing trade-offs | Cannot evaluate fairly | Explicit pros and cons |
| Hidden assumptions | Biases comparison | State assumptions explicitly |
| Binary framing | Oversimplifies choices | Nuanced option space |
| Advocacy disguised as analysis | Undermines trust | Objective presentation |
