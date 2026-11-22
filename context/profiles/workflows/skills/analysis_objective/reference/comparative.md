# Comparative Analysis Objective

Evaluate alternatives against weighted criteria using structured decision methods. Ground all comparisons in research findings. Produce recommendation reports that articulate trade-offs and confidence levels explicitly.

## Base Behavior

the `analysis_objective` skill

## Comparative Focus

### Decision Matrix Construction

Build a criteria-by-alternatives matrix with 3-7 evaluation criteria, weights summing to 1.0, and 0-1 normalized scores.

### Evidence Integration

Weight evidence by source credibility. Apply quality tiers: peer-reviewed research receives highest weight, vendor documentation lowest. Detect marketing bias by checking for missing trade-offs.

### Trade-off Articulation

Identify what each alternative optimizes and sacrifices. Document opportunity costs. Assess sensitivity: how much would weights need to change for rankings to flip?

## Methodology

### Phase 1: Criteria Definition

Define evaluation dimensions from research findings, project constraints, and stakeholder priorities. Include technical factors (performance, scalability, security) and non-technical factors (team expertise, maintenance cost, ecosystem maturity).

### Phase 2: Alternative Identification

Extract candidates from discovery reports. Research additional options if the initial set seems incomplete. Establish a baseline for relative comparison when appropriate.

### Phase 3: Structured Evaluation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Build the decision matrix, weight the criteria, score the alternatives, and calculate weighted totals with sensitivity analysis.
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/criteria_weighting.md`
```

Build decision matrix, assign weights, score alternatives, and calculate weighted totals. Perform sensitivity analysis by varying weights to test decision robustness.

### Phase 4: Confidence Assessment

Brief template for a procedure-depth agent (launched from objective depth):

```text
Apply GRADE-based confidence scoring to the recommendation and state the confidence level with reasoning.
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/confidence_assessment.md`
```

Apply GRADE-based confidence scoring to the recommendation. State confidence level (High/Moderate/Low) with reasoning.

### Phase 5: Recommendation Synthesis

Generate a primary recommendation with supporting rationale. Document trade-offs for runners-up. Identify conditions under which secondary options become preferable.

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Create the comparative analysis report by organizing the recommendation and trade-offs and writing the structured report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

### Comparative-Specific Sections

Beyond standard report structure, comparative reports include:

- **Decision Matrix**: Criteria-by-alternatives table with weights and scores
- **Trade-offs**: What each alternative optimizes and sacrifices
- **Sensitivity Analysis**: How results change with different weightings
- **Secondary Options**: Conditions under which other alternatives become preferable

## Tool Permissions

**Allowed**: Read (reports, research), Grep (verification), Glob (context), Write (reserved report path only)

**Forbidden**: Modifying sources, launching sub-agents, inlining full reports

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Unequal evidence quality | Biased conclusions | Ensure comparable evidence across alternatives |
| Ignoring weight sensitivity | Fragile decisions | Perform sensitivity analysis on weights |
| Hidden trade-offs | Poor decisions downstream | State opportunity costs explicitly |
| Vendor comparison bias | Marketing distorts analysis | Detect and correct for promotional content |
| Single-criterion decisions | Oversimplified assessment | Apply multi-criteria with explicit weights |
| Recommendations without confidence | Overconfident guidance | State confidence with reasoning |

## Examples

**Tool Selection Comparison**

Compare React vs Vue vs Svelte for new SPA. Criteria: performance (0.2), ecosystem maturity (0.3), learning curve (0.2), team fit (0.3). Evidence: benchmarks, GitHub metrics, hiring data, developer surveys. Trade-off: React ecosystem breadth vs Svelte runtime performance. Confidence: Moderate (team experience data incomplete).

**Architecture Alternative Evaluation**

Compare monolith vs modular monolith vs microservices. Criteria: scalability (0.25), operational complexity (0.25), deployment speed (0.25), team coordination (0.25). Evidence: case studies, academic research, blog posts from practitioners. Recommendation: modular monolith for current scale. Confidence: High (research consensus aligns with team size).

**Build vs Buy Analysis**

Compare custom auth vs Auth0 vs Cognito. Criteria: time-to-market (0.3), long-term cost (0.2), control (0.25), maintenance burden (0.25). Evidence: vendor pricing, internal estimates, security requirements. Trade-off: custom control vs vendor speed. Sensitivity: recommendation changes if team exceeds 20 engineers. Confidence: Low (highly situational, cost projections uncertain).

## Related

the `analysis_objective` skill

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis_objective/reference/synthesis.md" />

`ctx:github.com/ctxmod/engineering/context::shared::practice@principles/reference/research_first.md`

`ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/external.md`

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
