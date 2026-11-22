# Report Writing Objective

Create structured reports that document findings, analysis, or status. Reports transform raw information into organized, referenceable documents. Each report type follows a specific schema while sharing common structural patterns.

## Base Behavior

the `writing_objective` skill

## Report Categories

Reports fall into four categories based on purpose:

| Category | Purpose | Produces |
|----------|---------|----------|
| Discovery | Document what exists | Inventories, catalogs, maps |
| Analysis | Interpret findings | Insights, recommendations |
| Validation | Assess compliance | Pass/fail status, violations |
| Summary | Synthesize across sources | Consolidated overviews |

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_discovery.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_analysis.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_validation.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_summary.md" />

## Report Structure

All reports share common structural elements:

### Required Elements

**Frontmatter**: Metadata including type, date, scope, and related files.

**Title**: Descriptive title indicating report scope and type.

**Summary**: 1-3 sentences providing orientation. State scope, key findings, and conclusions. Keep brief—details belong in body sections.

**Body Sections**: Organized content following category-specific schemas.

**Metadata**: Sources examined, methods used, and limitations affecting completeness.

### Optional Elements

**Recommendations**: Suggested actions when findings warrant them.

**Appendices**: Supporting details too detailed for main sections.

**Cross-References**: Links to related reports and source materials.

## Source Attribution

Reports require traceable attribution for all claims:

| Source Type | Format |
|-------------|--------|
| File with lines | `path/to/file.ext:15-42` |
| File without lines | `path/to/file.ext` |
| Section reference | `report.md § Section Name` |
| External source | `[Title](URL)` |
| Observation | `[observation: context]` |

Position attribution inline or immediately following the claim.

## Report Organization

Brief template for a procedure-depth agent (launched from objective depth):

```text
Organize the report according to the report organization procedure.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
```

Reports follow a hierarchical structure:

- **H1**: Report title (one per report)
- **H2**: Major sections (findings, analysis, recommendations)
- **H3**: Subsections within major sections
- **H4**: Rarely needed; consider restructuring if frequent

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Create the report at a report path reserved just-in-time via `ctxmod work new report "<title>"` following the report creation procedure.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

Each report path is reserved just-in-time via `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing. The slug determines file naming.

## Length Guidelines

| Scope | Target Length |
|-------|---------------|
| Narrow (single artifact) | 300-500 words |
| Standard (category or domain) | 500-1000 words |
| Broad (cross-domain) | 1000-1500 words |

Optimize for signal-to-noise ratio. Longer reports require proportionally significant content.

## Tool Constraints

**Allowed**:
- Read: Source materials, related reports, reference files
- Glob: File pattern verification
- Grep: Finding verification
- Write: Report at the reserved report path only

**Forbidden**:
- Modifying source artifacts
- Launching sub-agents
- Writing outside the reserved report path
- Making implementation decisions

## Response Format

After writing the report, return a brief response:

```
Wrote {report type} report to <reserved report path>.

Summary: [2-3 sentence summary of scope and key findings]
```

Do not return full report content. The document is the deliverable.

## Variants

Report variants specialize for different purposes:

| Variant | Focus |
|---------|-------|
| `discovery` | Exploration findings with source attribution |
| `analysis` | Interpretation with recommendations |
| `validation` | Compliance assessment with severity classification |
| `summary` | Cross-report synthesis with consolidated insights |

Each variant adds specific sections and content requirements to this base structure.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Claims without attribution | Unverifiable findings | Include source for every claim |
| Inline quotation | Bloats report | Reference with path:line |
| Missing summary | Unclear orientation | Open with 1-3 sentence summary |
| Exhaustive listing | Low signal | Summarize and categorize |
| Returning full content | Bloats orchestrator | Return summary + path |
| Missing metadata | Incomplete context | Document sources and methods |
