# Summary Report Writing Objective

Create summary reports that synthesize multiple reports into consolidated overviews. Summary reports aggregate findings across reports, extract cross-cutting themes, and provide unified guidance. Each summary enables stakeholders to understand overall status without reading individual reports. Produce the report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.

## Base Behavior

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports.md" />

## Report Schema

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_summary.md" />

The schema defines frontmatter requirements, required and optional sections, synthesis guidelines, compression ratios, and length guidelines. Follow the schema for report structure.

## Variants

Specialized summary variants exist for different audiences and purposes:

| Variant | Audience/Purpose | Schema |
|---------|------------------|--------|
| `executive` | High-level stakeholder overview | `summary/executive.md` |
| `technical` | Detailed developer context | `summary/technical.md` |
| `progress` | Status and completion tracking | `summary/progress.md` |
| `decision` | Options and trade-offs for decisions | `summary/decision.md` |
| `handoff` | Context transfer for continuation | `summary/handoff.md` |

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_summary_executive.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_summary_technical.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_summary_progress.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_summary_decision.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_summary_handoff.md" />

Use this base objective for general-purpose summaries. Use variants when audience or purpose is specific.

## Scope

Summary reports aggregate content from any report type:

- Multi-report synthesis (combining discovery, analysis, or validation reports)
- Phase summaries (consolidating work within a time period)
- Domain rollups (aggregating reports across a functional area)

Summary reports answer "what's the overall picture?"—synthesizing individual reports into coherent wholes.

## Relationship to Other Report Types

Summary reports sit atop the reporting hierarchy:

| Report Type | Relationship |
|-------------|--------------|
| Discovery | Input—provides findings to summarize |
| Analysis | Input—provides insights to aggregate |
| Validation | Input—provides compliance status to roll up |
| Summary | Output—synthesizes inputs |

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_discovery.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_analysis.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_validation.md" />

## Principles

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`
`work_store_first` in the ctxmod builtin `work_discipline` profile

SSOT ensures summaries reference source reports rather than duplicating content. Context efficiency keeps summaries focused on cross-cutting insights and actionable conclusions.

## Core Behavior

1. **Aggregate**: Collect key points from multiple reports
2. **Synthesize**: Identify patterns spanning reports
3. **Consolidate**: Merge related findings into unified themes
4. **Prioritize**: Surface critical items requiring attention
5. **Write**: Produce report document at the reserved report path

## Pre-Writing

Before drafting:

1. Read all source reports completely
2. Extract key findings, recommendations, and action items
3. Identify themes spanning multiple reports
4. Consolidate duplicate or related items
5. Reserve the report path just-in-time (`ctxmod work new report "<title>"`)

`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/summary_production.md`

## During Writing

`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/clarity.md`
`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/anti_patterns.md`

While authoring:

- Reference source reports rather than duplicating content
- Use format `[report-name.md]` or path for citations
- Aggregate related findings into unified statements
- Preserve critical details while reducing volume
- Maintain traceability to source reports
- Highlight items requiring escalation or decision

## Document Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Create and organize the summary report following the report creation and organization procedures.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
```

Summary reports follow report creation patterns:

- Frontmatter includes `type: summary` and scope indicator
- Source Reports section lists all aggregated materials
- Key Themes draw from multiple sources with attribution
- Action Items reference originating reports

## Tool Constraints

**Allowed**:
- Read: Source reports, related context
- Glob: Report discovery
- Write: Report document at the reserved report path only

**Forbidden**:
- Modifying source reports
- Launching sub-agents
- Writing outside the reserved report path
- Original analysis (synthesis only—no new findings)

## Response Format

After writing the report, return a brief response:

```
Wrote summary report to <reserved report path>.

Summary: [2-3 sentence summary of scope and primary conclusions]
```

Do not return the full report content. The document is the deliverable.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Serial report listing | No synthesis value | Organize by theme |
| Duplicating source content | Bloats summary | Reference with paths |
| Missing source attribution | Unverifiable claims | Link each point to source |
| Original analysis | Scope creep | Synthesize existing conclusions |
| Equal weight to all items | Unclear priorities | Surface critical items prominently |
| Losing detail without reference | Cannot find depth | Enable navigation to sources |
