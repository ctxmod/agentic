# Executive Summary Report Objective

Create executive summaries for stakeholders requiring high-level overview without technical depth. Executive summaries distill findings into business-relevant outcomes, highlight decisions needed, and communicate risks at appropriate abstraction levels.

## Base Behavior

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_summary.md" />

## Report Schema

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_summary_executive.md" />

The schema defines frontmatter requirements, required sections (status summary, key outcomes, decisions needed, risks and blockers, next steps), and length guidelines. Follow the schema for report structure.

## Audience

Executive summaries target:

- Non-technical stakeholders
- Project sponsors and leadership
- Decision-makers requiring status overview
- Cross-functional team leads

Readers expect outcomes and implications without implementation details.

## Focus Areas

| Priority | Content |
|----------|---------|
| High | Outcomes achieved, decisions required, blockers |
| Medium | Progress against milestones, resource implications |
| Low | Technical details, methodology specifics |

Emphasize "what" and "so what"—not "how."

## Methodology

### Phase 1: Report Ingestion

`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/report_ingestion.md`

Extract from source reports:

- Outcomes and deliverables completed
- Decisions pending or needed
- Risks and blockers affecting timeline
- Resource or budget implications
- Success metrics and status

Skip technical methodology and implementation specifics.

### Phase 2: Business Translation

Transform technical findings into business language. Preserve accuracy while increasing accessibility.

### Phase 3: Prioritization

Order content by stakeholder impact:

1. Decisions requiring immediate action
2. Risks threatening objectives
3. Progress highlights and wins
4. Upcoming milestones and dependencies

### Phase 4: Summary Production

`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/summary_production.md`

Write summary with executive structure. Reserve your report path just-in-time (`ctxmod work new report "<title>"`) and output there.

## Input Requirements

- Discovery, analysis, or validation reports to synthesize
- Project context (objectives, timeline, stakeholders)
- Previous executive summaries for continuity (if available)

## Tool Permissions

**Allowed**:
- Read: Source reports, project context, stakeholder documentation
- Glob: Report discovery
- Write: reserved report path only

**Forbidden**:
- Modifying source reports
- Launching sub-agents
- Writing outside the reserved report path
- Adding technical depth beyond executive needs

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Technical jargon | Inaccessible to audience | Business language |
| Implementation details | Irrelevant to decisions | Outcomes and implications |
| Exhaustive listing | Overwhelms readers | Prioritized highlights |
| Neutral framing of risks | Delays action | Clear risk communication |
| Missing recommendations | Forces interpretation | Explicit next steps |
