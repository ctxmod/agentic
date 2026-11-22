# Technical Summary Report Objective

Create technical summaries that preserve implementation context for developer audiences. Technical summaries consolidate findings while maintaining precision necessary for engineering decisions, code reviews, and architecture documentation.

## Base Behavior

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_summary.md" />

## Report Schema

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_summary_technical.md" />

The schema defines frontmatter requirements, required sections (technical summary, architecture overview, findings by category, dependencies, recommendations, references), and length guidelines. Follow the schema for report structure.

## Audience

Technical summaries target:

- Developers inheriting or reviewing work
- Technical leads making architecture decisions
- Engineers needing implementation context
- Code reviewers requiring background

Readers expect precision, completeness, and actionable technical detail.

## Focus Areas

| Priority | Content |
|----------|---------|
| High | Architecture decisions, code patterns, dependencies |
| Medium | Performance findings, security considerations, test coverage |
| Low | Business justification, stakeholder context |

Emphasize "how" and "why" at the implementation level.

## Methodology

### Phase 1: Report Ingestion

`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/report_ingestion.md`

Extract from source reports:

- Architecture decisions and rationale
- Code patterns and conventions identified
- Dependencies and version constraints
- Performance benchmarks and bottlenecks
- Security findings and mitigations
- Test coverage and quality metrics

Preserve file paths, line numbers, and code references.

### Phase 2: Technical Consolidation

Merge related findings while preserving detail: group similar patterns with representative examples, consolidate dependencies, show component relationships, aggregate repeated issues with occurrence counts.

### Phase 3: Reference Preservation

Maintain traceability to source: link file paths with line ranges, reference specific commits or versions, cite external documentation, preserve exact error messages or outputs.

### Phase 4: Summary Production

`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/summary_production.md`

Write summary with technical structure. Reserve your report path just-in-time (`ctxmod work new report "<title>"`) and output there.

## Input Requirements

- Discovery, analysis, or validation reports with technical content
- Codebase access for verification (optional)
- Previous technical summaries for continuity (if available)

## Tool Permissions

**Allowed**:
- Read: Source reports, reference documentation, code files
- Grep: Pattern verification
- Glob: File discovery
- Write: reserved report path only

**Forbidden**:
- Modifying source code
- Launching sub-agents
- Writing outside the reserved report path
- Simplifying beyond technical accuracy

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Vague references | Cannot locate in codebase | Exact paths and lines |
| Simplified explanations | Loses technical precision | Accurate terminology |
| Missing rationale | Cannot evaluate decisions | Include reasoning |
| Orphan findings | No actionable context | Link to components |
| Unstated assumptions | Hidden dependencies | Explicit prerequisites |
