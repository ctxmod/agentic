---
description: "Orchestrate academic tasks — write papers, correct documents, analyze sources, integrate findings. Classifies task type and executes the appropriate workflow phases."
binds_to_depth: phase
---

# Work Academic Phase

Orchestrate academic work across writing, correction, analysis, and
integration tasks. Classify the task type from user input and flags,
then execute the appropriate workflow phases with external research,
citation management, and iterative quality verification.

Work academic is a **phase, not a workflow**. The main agent invokes
it directly via the `/scholarly_task` slash-command.

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/patterns_overview.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflows_overview.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflow_work_academic.md" />

## When to Use

Use this phase when:

- Drafting new academic content (papers, essays, literature reviews)
- Revising existing documents for quality
- Analyzing source documents for findings
- Integrating sources into existing documents

## Task Type Classification

The main agent classifies the task type before selecting workflow
phases. Explicit `--type` overrides automatic classification.

**Automatic classification**:

| Signals Present | Inferred Type | Rationale |
|-----------------|---------------|-----------|
| `--document` provided, no `--sources` | `correction` | Existing document implies revision work |
| `--sources` provided, no `--document` | `analysis` | Source material implies analytical output |
| `--sources` and `--document` both provided | `integration` | Combining sources into existing document |
| Neither `--document` nor `--sources` | `writing` | No existing artifacts implies new composition |

**Task type capabilities**:

| Type | Primary Activity | Typical Output |
|------|-----------------|----------------|
| `writing` | Draft new academic content from research | Paper, essay, literature review |
| `correction` | Revise existing document for quality | Corrected document with change log |
| `analysis` | Examine source documents for findings | Analysis report with citations |
| `integration` | Synthesize sources into existing document | Updated document with integrated material |

## Phase Orchestration

Up to eight phases, conditional on task type. The work/academic
workflow reference carries the per-phase shape.

```
Phase 0: Task Classification (direct, main agent)
         ↓
Phase 1: Document Discovery (conditional)
         ↓
Phase 2: Academic Research (3 dispatches, conditional)
         ↓
Phase 3: Analysis (Opus synthesis or Sonnet finding analysis)
         ↓
Phase 4: Planning (conditional)
         ↓
Phase 5: Execution (Opus, single pass)
         ↓
Phase 6: Academic Validation
         ↓
Phase 7: Revision Loop (max 3, conditional)
         ↓
      Complete
```

**Conditional phase execution**:

| Phase | New Writing | Correction | Analysis | Integration |
|-------|-------------|------------|----------|-------------|
| 1: Discovery | Yes | Yes | Yes | Yes |
| 2: Research | Yes (broad) | Skip | Conditional | Yes (gap-filling) |
| 3: Analysis | Opus synthesis | Sonnet analysis | Opus synthesis | Opus synthesis |
| 4: Planning | Yes | Skip | Skip | Yes |
| 5: Execution | Write document | Apply revisions | Write report | Write unified document |
| 6: Validation | Always | Always | Always | Always |
| 7: Revision | Conditional | Conditional | Conditional | Conditional |

## Information Flow

```
Phase 0: Classification
- Receives: task-description, flags
- Produces: task type

Phase 1: Discovery
- Receives: task type, document/sources
- Produces: artifact inventory

Phase 2: Research (3 dispatches, conditional)
- Receives: Phase 1 output
- Produces: per-source research reports

Phase 3: Analysis
- Receives: Phase 2 output (or Phase 1 if research skipped)
- Produces: synthesis or finding analysis

Phase 4: Planning (conditional)
- Receives: Phase 3 output
- Produces: document structure plan

Phase 5: Execution
- Receives: Phase 4 output (or Phase 3 if planning skipped)
- Produces: document or report

Phase 6: Validation
- Receives: Phase 5 output
- Produces: validation reports

Phase 7: Revision (conditional)
- Receives: Phase 6 findings
- Produces: revised document
```

Main agent passes report **paths** between phases, never report content.

## Work Item Context

This phase operates with optional work item context. When work item
context exists, reports nest within work item directories. When
absent, reports write to the store's top-level `reports/` (no `--owner`).

**Story creation behavior**:

| Context Provided | Behavior |
|-----------------|----------|
| `--story {id}` specified | Append new issues to existing story |
| `--epic {id}` specified | Create new story under epic, create issues under story |
| Neither specified | Create standalone story (e.g., "Academic: {task-description}") |

## Error Handling

| Situation | Response |
|-----------|----------|
| No task description provided | Report usage and exit |
| Source files not found at specified paths | Report missing files, prompt for correction |
| Citation style not recognized | Default to APA, report override |
| Research yields no relevant results | Proceed with available sources, note gaps in report |
| Quality verification fails | Re-enter Phase 5 for targeted revision (max 3 iterations) |
| Output path not writable | Report and suggest alternative path |

Error handling follows stop-report-propose-await pattern.

## Sub-Layer Launch

When this phase requires research-depth execution, launch the
`workflow` agent for the academic-research phases:

Brief template for a workflow-depth agent — Execute academic research phases of work:academic:

```text
Execute the academic research workflow.
Phase: academic-papers query formulation → per-source search → synthesis
Citation style: <user-provided or APA default>
```

The non-research phases (classification, document discovery,
planning, execution, validation, revision loop) are driven directly
by the main agent per the workflow reference.

`ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/academic_register.md`
`ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/terminology.md`
`ctx:github.com/ctxmod/writing_academic/context::shared::citation_practice@scholarly_prose/reference/citation_practice.md`
`ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/rhythm.md`

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Skipping task classification | Always classify before phase selection |
| Running all phases unconditionally | Skip phases per the conditional table |
| Ignoring citation style | Enforce consistency against specified style |
| Single-pass validation | Use revision loop when validation fails |

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"execute an academic task" as a sub-step. Launch the
`scholarly_task_phase` agent from any workflow that needs academic
writing, correction, analysis, or integration.

## Cross-Cutting Reference

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/sequential.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_loop.md" />
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`work_store_first` in the ctxmod builtin `work_discipline` profile
