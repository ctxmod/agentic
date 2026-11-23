---
description: "Report writing phase — produce synthesis reports combining findings from multiple prior sources through discovery, analysis, planning, writing, and conditional validation."
binds_to_depth: phase
---

# Write Report Phase

Report writing phase — produce synthesis reports that combine findings
from multiple prior sources into a coherent document. This is one of
four sibling phase skills in the writing capability (report, work-item,
context, intent); they share the same methodology layer
(`writing_phase`) and the shared objective (`writing_objective`) and
procedure (`writing_procedure`).

Report writing is a **phase, not a workflow**. The main agent invokes it
directly via the absorbed `/writing_report` slash-command, the `/write`
router, or any other domain's workflow agent references this phase as a
"write report" sub-step.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Producing synthesis reports from prior discovery/analysis phases
- Reports combine multiple source documents
- Schema compliance matters (discovery, analysis, validation, summary)
- Source attribution is critical

## Phase Orchestration

This phase dispatches objective-depth sub-agents through the shared
`writing_objective`, with report-specific reference material below
scoping each dispatch. The `writing_phase` methodology layer carries
the variant-selection reference (model choice, coordination pattern,
transition criteria, anti-patterns).

Every phase's report path is reserved by the executing sub-agent
just-in-time — `ctxmod work new report "<phase title>"
--owner <owning work item, if any>` immediately before writing. The
`- Report:` lines below name the slug convention each reservation uses;
they are never pre-minted dispatch parameters. Sub-agents return their
reserved paths, and the main agent passes those returned paths onward,
never report content.

```
Phase 1: Source Collection (Sonnet)
         |
Phase 2: Synthesis Analysis (Sonnet)
         |
Phase 3: Planning (Sonnet)
         |
Phase 4: Writing (Opus)
         |
Phase 5: Validation (conditional)
```

### Phase 1: Source Collection

Gather source materials for synthesis. Launch the discovery sub-agent
against the source scope. Launch parallel discovery sub-agents when
source materials span multiple directories or categories.

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-source-collection.md`

**Workflow-specific notes**:
- Collect source references only; do not synthesize
- Catalog structure and content of each source
- Note gaps in available sources
- If no sources specified, search work item reports directory
- Include sources from related work items if relevant

### Phase 2: Synthesis Analysis

Analyze source materials for synthesis approach.

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-synthesis-analysis.md`

**Workflow-specific notes**:
- Analysis only; do not write final report
- Identify key themes across sources
- Note contradictions requiring resolution
- Determine synthesis structure
- Map findings to report type schema

### Phase 3: Planning

Plan the report structure.

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-planning.md`

**Workflow-specific notes**:
- Plan structure only; do not write report
- Define sections per schema
- Outline content for each section
- Specify source references
- Match schema requirements for report type
- Balance detail with readability

### Phase 4: Writing (Opus)

Produce the synthesis report.

- Output: `{REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{type}.md`

**Workflow-specific notes**:
- Write report only
- Follow report-type schema structure
- Reference sources rather than duplicating
- Frontmatter: type, date, sources
- Include file references with line ranges where applicable
- Synthesis presents coherent narrative

### Phase 5: Validation (Conditional)

**When to validate**:
- Schema-validated report types (discovery, analysis, validation, summary)
- Cross-reference integrity critical

**When to skip**:
- General prose reports
- Self-contained content without cross-references

A skipped loop is marked degradation, not quiet opt-out: for HTML/rich
artifacts the report still states what was checked and how (silent
completeness is never the fallback), and unverifiable claims carry the
`[unverified: …]` marker defined in creation.md's Decision Points.
Markdown/plain-text artifacts are exempt (they degrade by nature).

Before accepting the final artifact, run one consumer-perspective read
per creation.md step 8 (re-open the artifact as its consumer, with no
context beyond the artifact itself); a skip of the remediation loop
never skips this read.

If validating, apply the validation loop (max 3 remediation iterations).

## Report Types

| Type | Purpose | Schema reference |
|------|---------|------------------|
| `discovery` | Catalog findings from exploration | <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_discovery.md" /> |
| `analysis` | Synthesize insights from data | <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_analysis.md" /> |
| `validation` | Document verification results | <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_validation.md" /> |
| `summary` | Executive overview of work | <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_summary.md" /> |

## Information Flow

```
Phase 1: Source Collection
- Receives: REPORTS_BASE, source paths (if provided)
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-source-collection.md

Phase 2: Synthesis Analysis
- Receives: Phase 1 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-synthesis-analysis.md

Phase 3: Planning
- Receives: Phase 2 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-planning.md

Phase 4: Writing
- Receives: Phase 3 output, available Phase 1/2 outputs
- Produces: {REPORTS_BASE}/<YYYY-MM>/<DD-HHMM>-<id>-{type}.md

Phase 5: Validation (conditional)
- Receives: Report path, planning output (criteria reference)
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-04-validation.md
```

Main agent passes report **paths** between phases, never report content. Each Produces path is reserved by the producing sub-agent just-in-time and reaches the next phase via the return message, not the dispatch.

## Execution Strategy

1. **Resolve report type**: Determine from `type` argument
2. **Source Collection**: Launch discovery sub-agent(s); wait
3. **Analysis**: Launch analysis sub-agent with discovery output
4. **Planning**: Launch planning sub-agent with analysis output
5. **Writing**: Launch writing sub-agent (Opus) with planning output
6. **Conditional Validation** (if schema-validated report):
   a. Launch validation sub-agent targeting created report
   b. Read validation report
   c. If errors: Launch remediation, re-validate (max 3 iterations)
   d. If clean or skipped: Exit
7. **Complete**: Return report path and validation summary

## Workflow Variants

### Lightweight Report

Skip Phase 2 (analysis) when:
- Single source informs content
- Report type has predetermined structure
- Source catalog sufficient for synthesis

```
Source Collection → Planning → Writing → Validation
```

### Multi-Source Report

Use parallel discovery when sources span multiple directories:

```
Source Collection (Parallel) → Analysis → Planning → Writing → Validation
├── Source directory A ───────┐
├── Source directory B ───────┼──→ Synthesis
└── Related work items ───────┘
```

## Error Handling

| Situation | Response |
|-----------|----------|
| Source collection finds insufficient material | Report gaps, propose alternative sources, await decision |
| Validation exceeds iterations | Report persistent issues, document unresolved, await decision |
| Writing fails | Check planning output, propose re-planning, await decision |

Error handling follows stop-report-propose-await pattern.

## Quality Criteria

| Criterion | Validation Method |
|-----------|-------------------|
| Schema compliance | Phase 5 checks report-type schema |
| Source attribution | Phase 5 verifies every claim references a source |
| Cross-reference integrity | Phase 5 validates link tags resolve |
| Clarity and anti-patterns | Phase 5 applies clarity principles |

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Skipping source collection | Uninformed synthesis | Gather sources first |
| Writing without planning | Incoherent structure | Plan before writing |
| Single validation pass | Unverified quality | Use validation loop |
| Duplicating source content | Bloats report | Reference with file:line |
| Returning report content | Bloats orchestrator | Return path only |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`writing_objective` agent. The objective in turn launches
`writing_procedure` for step mechanics.

Brief template for the `writing_objective` agent — Execute <phase-name> within report writing:

```text
<ctxmod_activate skill="agentic_workflows_writing_objective" />
Artifact category: report
Variant: <report-type>
Phase-specific instructions:
  <phase-specific parameters>
Reserve your report path just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write there and return the path.
```

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"write report" as a sub-step. Launch the `write_report_phase` agent
from any workflow that needs synthesis-report writing.

Phase-selection reference (model choice, coordination pattern,
transition criteria, anti-patterns) applies to every writing phase
variant:

the `writing_phase` skill

## Cross-Cutting Reference

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile

Depth-scoped writing guidance for the sub-agents this phase launches:

the `writing_objective` skill
the `writing_procedure` skill

## Sibling Phase Reference

The other two writing capability phase skills:

the `write_item_phase` skill
the `write_context_phase` skill (`github.com/ctxmod/writing_context/context`, `authoring` profile)

Intent writing moved to its own `intent` domain:

the `intent_write` skill
