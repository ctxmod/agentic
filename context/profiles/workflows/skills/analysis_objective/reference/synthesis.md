# Synthesis Analysis Objective

Combine multiple discovery reports to identify cross-cutting patterns, resolve contradictions, and generate unified insights. Synthesis extracts what reports reveal together that no single report shows alone.

## Base Behavior

the `analysis_objective` skill

## Synthesis Focus

Synthesis extracts what reports reveal together that no single report shows alone. Core activities:

- **Cross-source pattern detection**: Identify findings appearing in 2+ reports
- **Contradiction resolution**: Model why sources disagree through refutational synthesis
- **Confidence assessment**: Apply GRADE levels based on evidence quality

## Methodology

Apply these procedures in sequence:

### Phase 1: Report Ingestion

Brief template for a procedure-depth agent (launched from objective depth):

```text
Consume the input reports and extract their findings with source attribution (path:line).
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/report_ingestion.md`
```

Consume input reports and extract findings with source attribution (path:line).

### Phase 2: Pattern Aggregation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Aggregate findings across reports using the Best Fit Framework and build a concept-to-source matrix.
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/pattern_aggregation.md`
```

Apply Best Fit Framework: start with schema, fit findings, extend when needed. Create concept-to-source matrix.

### Phase 3: Contradiction Resolution

Brief template for a procedure-depth agent (launched from objective depth):

```text
Model disagreements between sources through refutational synthesis and develop boundary conditions for when each finding applies.
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/contradiction_resolution.md`
```

Model disagreements through refutational synthesis. Develop boundary conditions explaining when each finding applies.

### Phase 4: Confidence Assessment

Brief template for a procedure-depth agent (launched from objective depth):

```text
Assign GRADE confidence levels to the synthesized findings and document the rationale for each assessment.
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/confidence_assessment.md`
```

Assign GRADE confidence levels to synthesized findings. Document rationale for each assessment.

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Create the synthesis report by organizing the unified insights, writing the structured report at a report path reserved just-in-time via `ctxmod work new report "<title>"`, and producing its summary.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/summary_production.md`
```

### Synthesis-Specific Sections

Beyond standard report structure, synthesis reports include:

- **Cross-Report Patterns**: Themes appearing in 2+ reports with source attribution
- **Contradictions Resolved**: Conflicts with resolution rationale and boundary conditions

## Tool Permissions

**Allowed**: Read (reports), Grep (verification), Write (reserved report path only)

**Forbidden**: Modifying source files, launching sub-agents, inlining report content

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Aggregation without interpretation | No added value | Extract cross-report patterns |
| Ignoring contradictions | Incomplete | Model disagreement sources |
| Equal weight to all findings | Misleading | Apply frequency weighting |
| Losing source attribution | Unverifiable | Trace to report:line |
| Overconfident conclusions | Misleading | State confidence levels |

## Examples

**Multi-Architecture Synthesis**

Contradiction: architecture favors modules (architecture.md:45-52), patterns show inheritance (patterns.md:23-31). Resolve via recency. Recommend modular migration.

**Validation Aggregation**

Cross-report pattern: docs lag implementation (test.md:12, doc.md:34, source.md:56). Recommend simultaneous validation.

**Research Consolidation**

Frequency: 8/10 recommend Redis. Contradiction: paper favors Memcached for small scale. Recommend Redis with <1GB exception.

## Related

`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`

`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
