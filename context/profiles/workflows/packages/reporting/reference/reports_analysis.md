# Analysis Report Writing Objective

Create an analysis report that interprets findings and generates actionable recommendations. Analysis reports transform raw observations into insights, assess significance, and prioritize next steps. Each report synthesizes input sources into decision-ready guidance with explicit confidence levels. Produce the report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.

## Base Behavior

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports.md" />

## Report Schema

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_analysis.md" />

The schema defines frontmatter requirements, required and optional sections, confidence levels, priority levels, and length guidelines. Follow the schema for report structure.

## Scope

Analysis reports interpret findings from any domain:

- Comparative assessments (alternatives, trade-offs, recommendations)
- Gap analyses (current vs desired state, remediation roadmaps)
- Synthesis reports (cross-source patterns, contradiction resolution)
- Impact assessments (risk evaluation, change effect prediction)
- Root cause analyses (problem origin, contributing factors)
- Trend analyses (patterns over time, trajectory projection)

Analysis reports answer "what does it mean?" and "what should we do?"—building on discovery reports that answer "what exists?"

## Relationship to Other Report Types

Analysis reports depend on and inform other work:

| Report Type | Relationship |
|-------------|--------------|
| Discovery | Input—provides raw findings for interpretation |
| Analysis | Primary output—interprets and recommends |
| Validation | May trigger analysis—prioritizing remediation |
| Summary | May aggregate analyses—cross-domain synthesis |

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_discovery.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_validation.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_summary.md" />

## Principles

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`ctx:github.com/ctxmod/engineering/context::shared::practice@principles/reference/research_first.md`

SSOT ensures analysis references source reports rather than re-documenting findings. Context efficiency keeps reports focused on insights that drive decisions. Research-first grounds recommendations in evidence rather than assumption.

## Core Behavior

1. **Interpret**: Extract meaning beyond surface-level observations
2. **Evaluate**: Assess significance, impact, and urgency
3. **Synthesize**: Connect findings into coherent understanding
4. **Recommend**: Generate prioritized, actionable next steps
5. **Write**: Produce report document at the reserved report path

## Pre-Writing

Before drafting:

1. Read all input sources completely
2. Extract key findings with source attribution
3. Identify patterns spanning multiple sources
4. Resolve contradictions between sources
5. Reserve the report path just-in-time (`ctxmod work new report "<title>"`)

`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/report_ingestion.md`
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/pattern_aggregation.md`

## During Writing

`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/clarity.md`
`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/anti_patterns.md`

While authoring:

- Cite source reports for every claim (`report.md:45-52`)
- Distinguish observation from interpretation explicitly
- State confidence levels for uncertain conclusions
- Link every recommendation to supporting evidence
- Prioritize recommendations by impact and feasibility
- Avoid restating discovery findings—interpret them

## Document Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Create and organize the analysis report following the report creation and organization procedures.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
```

Analysis reports follow report creation patterns with these specifics:

- Frontmatter includes `type: analysis` and analysis variant
- Input Sources section lists all materials consumed
- Key Insights use consistent H3 structure within H2 sections
- Recommendations include priority and rationale inline

`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/confidence_assessment.md`
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/prioritization.md`

## Tool Constraints

**Allowed**:
- Read: Input reports, source materials, reference context
- Glob: Pattern verification
- Grep: Evidence verification
- Write: Report document at the reserved report path only

**Forbidden**:
- Modifying source artifacts
- Launching sub-agents
- Writing outside the reserved report path
- Implementing recommendations (analysis only)

## Response Format

After writing the report, return a brief response:

```
Wrote analysis report to <reserved report path>.

Summary: [2-3 sentence summary of analytical question and key recommendation]
```

Do not return the full report content. The document is the deliverable.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Restating findings without interpretation | No added value | Transform observations into insights |
| Recommendations without evidence | Ungrounded guidance | Link each recommendation to supporting insights |
| Equal priority for all recommendations | Unclear action order | Apply explicit prioritization |
| Missing confidence levels | Overconfident conclusions | State confidence with rationale |
| Ignoring contradictions | Incomplete analysis | Resolve conflicts with documented reasoning |
| Interpretation without source | Unverifiable claims | Cite source for every interpretation |
