# Discovery Report Writing Objective

Create a discovery report that documents findings from systematic exploration. Discovery reports catalog what exists, where it resides, and how elements relate. Each report provides structured findings with full source attribution, enabling subsequent analysis or action. Produce the report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.

## Base Behavior

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports.md" />

## Report Schema

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_discovery.md" />

The schema defines frontmatter requirements, required and optional sections, attribution format, and length guidelines. Follow the schema for report structure.

## Scope

Discovery reports document exploration of any domain:

- Artifact inventories (files, resources, entities)
- Structural organization (hierarchies, groupings, boundaries)
- Relationship maps (dependencies, references, connections)
- Pattern catalogs (recurring structures, conventions, idioms)
- Gap identification (missing elements, incomplete coverage)
- Quality observations (anomalies, inconsistencies, violations)

Discovery reports answer "what exists?" without interpreting significance—interpretation belongs to analysis reports.

## Relationship to Other Report Types

Discovery reports feed downstream work:

| Report Type | Relationship |
|-------------|--------------|
| Discovery | Primary output—catalogs findings |
| Analysis | Consumes discovery—interprets findings |
| Validation | May trigger discovery—gap identification |
| Summary | Aggregates discovery—cross-report synthesis |

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_analysis.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_validation.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_summary.md" />

## Principles

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`

SSOT ensures discovery reports document authoritative locations rather than duplicating content. Context efficiency keeps reports focused on significant findings. Reference over duplication favors file references with line ranges over inline quotation.

## Core Behavior

1. **Organize**: Structure findings hierarchically by category
2. **Reference**: Include source attribution for all claims
3. **Quantify**: Provide counts, percentages, and metrics where applicable
4. **Identify gaps**: Note areas requiring further investigation
5. **Write**: Produce report document at the reserved report path

## Pre-Writing

Before drafting:

1. Collect all raw findings with source locations
2. Group findings into logical categories (3-7 major sections)
3. Identify patterns spanning multiple findings
4. Reserve the report path just-in-time (`ctxmod work new report "<title>"`)

`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/broad_scan.md`

## During Writing

`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/clarity.md`
`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/anti_patterns.md`

While authoring:

- Include source attribution for every claim
- Use file:line format for precise location (`path/to/file.ext:15-42`)
- Summarize rather than exhaustively list
- Quantify where possible (counts, percentages, ratios)
- Distinguish observations from interpretations
- Note uncertainty explicitly when source is unclear

## Document Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Create and organize the discovery report following the report creation and organization procedures.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
```

Discovery reports follow report creation patterns with these specifics:

- Frontmatter includes `type: discovery` and `objective: discovery`
- Findings section uses consistent H2/H3 heading hierarchy
- Each finding category gets its own H2 section
- Individual findings within categories use H3 headings

## Tool Constraints

**Allowed**:
- Read: Source materials, related reports
- Glob: File pattern verification
- Grep: Finding verification
- Write: Report document at the reserved report path only

**Forbidden**:
- Modifying source artifacts
- Launching sub-agents
- Writing outside the reserved report path
- Making implementation decisions (discovery is read-only analysis)

## Response Format

After writing the report, return a brief response:

```
Wrote discovery report to <reserved report path>.

Summary: [2-3 sentence summary of scope and key findings]
```

Do not return the full report content. The document is the deliverable.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Claims without attribution | Unverifiable findings | Include source for every claim |
| Exhaustive listing | Low signal, high noise | Summarize and categorize |
| Inline quotation | Bloats report, duplicates content | Reference with path:line |
| Interpretation as finding | Conflates discovery with analysis | State what exists, not what it means |
| Missing gaps section | Incomplete picture | Document what remains unknown |
| Generic titles | Unclear scope | Use descriptive titles indicating domain |
