# Academic Work Workflow

Orchestrate academic tasks from classification through validated output. The workflow adapts its phase pipeline based on task type: new writing, correction, analysis, or integration. Each type activates a different subset of phases while sharing the same validation and revision infrastructure.

> **Concurrency gate.** Where this document describes agents running "in parallel" or "concurrently", that shape is available only to agents whose resolved `allow-parallel-agents` posture is `true` — the read-only discovery, analysis, research, review, and validation phase agents. The concurrency instruction in your own agent-depth instructions is authoritative: if it tells you to use one agent at a time, run the same steps sequentially in the listed order. The phase decomposition, report paths, and synthesis steps are unchanged either way.

## Workflow Overview

```
Phase 0: Task Classification (direct)
         ↓
Phase 1: Document Discovery (conditional)
         ↓
Phase 2: Academic Research (parallel, conditional)
         ├── Papers research
         ├── Surveys research
         └── Specifications research
         ↓
Phase 3: Analysis (adapts by task type)
         ↓
Phase 4: Planning (conditional)
         ↓
Phase 5: Execution (Opus)
         ↓
Phase 6: Academic Validation ←──── Phase 7: Revision Loop (max 3)
         ↓ All pass
      Complete
```

## When to Use

Use academic work workflow when:

- Writing a new academic document (paper, thesis chapter, literature review, research report, letter)
- Correcting or revising an existing academic document against validation findings
- Analyzing and summarizing academic sources into structured output
- Integrating findings from multiple sources into a unified document
- The task requires academic rigor: proper citation, evidence-backed claims, disciplinary structure

Academic work workflow extends the academic research workflow with full lifecycle support including writing, correction, and validation.

## Task Types

| Type | Input | Primary Activity | Output |
|------|-------|------------------|--------|
| New Writing | Topic, requirements, optional sources | Research, plan, write | Complete academic document |
| Correction | Existing document, feedback or validation findings | Validate, analyze, revise | Revised document |
| Analysis | Sources to analyze | Discover, research, synthesize | Analysis report or summary |
| Integration | Multiple source documents | Discover, research gaps, synthesize | Unified document |

Task type determines which phases execute. Phase 0 classifies the task; subsequent phases check the classification and skip when irrelevant.

## Phase Structure

### Phase 0: Task Classification (direct)

**Main agent performs directly (no sub-agent).**

Classify the task into one of four types based on:

- Whether an existing document requires modification (Correction) or new output is needed (New Writing, Analysis, Integration)
- Whether the task centers on producing original prose (New Writing), fixing existing prose (Correction), interpreting sources (Analysis), or merging multiple inputs (Integration)
- Whether academic research is needed or the task operates on provided materials

Output: task type, input artifacts, target output path, scope boundaries. Store classification in the Phase 0 report for downstream phases to reference.

### Phase 1: Document Discovery (conditional)

**Conditional execution**: Skip when task classification identifies no existing documents or sources to discover (rare; most tasks involve at least source scanning).

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_academic.md" />

**Workflow-specific notes**:
- **New Writing**: Scan academic landscape around the topic; identify seminal papers, active debates, research gaps
- **Correction**: Catalog the existing document structure, identify sections requiring revision, extract feedback items
- **Analysis**: Catalog provided sources, extract metadata, identify relationships between sources
- **Integration**: Catalog all input documents, map overlapping content, identify gaps between sources
- Output: discovery report with source inventory and structural assessment

### Phase 2: Academic Research (parallel, conditional)

**Conditional execution**: Skip for Correction tasks (operate on existing material). Skip when task provides all required sources and no external research is needed.

`ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_phase@academic/reference/academic_papers.md`
`ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_phase@academic/reference/surveys.md`
`ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_phase@academic/reference/specifications.md`
`ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_phase@academic/reference/query_formulation.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

**Workflow-specific notes**:
- Formulate queries from discovery findings before launching parallel search
- Launch three research agents in parallel in a single message: papers, surveys, specifications
- **New Writing**: Broad research to establish theoretical foundation and identify related work
- **Analysis**: Targeted research to contextualize and validate source material
- **Integration**: Gap-filling research where input documents leave questions unanswered
- Fan-in at Phase 3

### Phase 3: Analysis

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_academic_synthesis.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_general.md" />

**Workflow-specific notes**:
- Always executes. Content and agent selection adapt by task type:
- **New Writing**: Academic synthesis (Opus) -- integrate research findings into thematic clusters, identify theoretical framework, map evidence to planned argument structure
- **Correction**: General analysis (Sonnet) -- classify validation findings by severity, identify root causes, determine revision scope, map feedback to specific document sections
- **Analysis**: Academic synthesis (Opus) -- synthesize sources thematically, assess evidence quality, identify consensus and disagreement, produce structured findings
- **Integration**: Academic synthesis (Opus) -- map relationships across input documents, resolve contradictions, identify synthesis opportunities, establish unified argument thread
- Output: analysis report with task-type-appropriate recommendations

### Phase 4: Planning (conditional)

**Conditional execution**: Execute for New Writing and Integration. Skip for Correction (revision scope determined by analysis) and Analysis (output structure determined by synthesis).

`ctx:github.com/ctxmod/agentic/context::shared::planning_phase@workflows/reference/general.md`

**Workflow-specific notes**:
- **New Writing**: Plan document structure, section outline, argument flow, citation strategy, and section-level evidence mapping
- **Integration**: Plan unified document organization, source attribution strategy, and how to reconcile conflicting findings across inputs
- Output: execution plan with structural outline and content assignments per section

### Phase 5: Execution (Opus)

`ctx:github.com/ctxmod/agentic/context::shared::writing_phase@workflows/reference/academic.md`
`ctx:github.com/ctxmod/agentic/context::shared::remediation_phase@workflows/reference/general.md`

**Workflow-specific notes**:
- Always executes. Uses Opus for all task types. Activity adapts:
- **New Writing**: Write the complete academic document following the plan from Phase 4. Apply citation conventions, disciplinary structure, and evidence-backed argumentation throughout.
- **Correction**: Apply revisions using the academic revision procedure. Execute the four-pass revision cycle (structure, sentence variety, word choice, engagement) targeting findings from Phase 3 analysis.
- **Analysis**: Write the analysis report or summary document. Organize findings thematically with source attribution and confidence levels.
- **Integration**: Write the unified document following the integration plan from Phase 4. Reconcile source material while maintaining attribution clarity.

`ctx:github.com/ctxmod/agentic/context::shared::remediation_procedure@workflows/reference/academic_revision.md`

### Phase 6: Academic Validation

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation/reference/validation_academic.md" />

**Workflow-specific notes**:
- Validation occurs AFTER writing completes (not embedded)
- Validates Phase 5 output against academic quality criteria
- Validation dimensions: citation correctness, evidence quality, source credibility, bias indicators, structural compliance, language standards
- Severity classification: missing/fabricated citations and unsourced claims are Errors; untriangulated findings and unacknowledged bias are Warnings
- Output: validation report with itemized findings and severity levels
- Exit condition: zero Errors and fewer than 5 Warnings

### Phase 7: Revision Loop (max 3 iterations)

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_loop.md" />

Brief template for a objective-depth agent (launched from workflow depth):

```text
Apply targeted academic revisions scoped to Phase 6 validation findings only (no scope expansion). Each iteration remediates errors then re-runs validation; exit on zero Errors and fewer than 5 Warnings, or report remaining issues on exhaustion.
`ctx:github.com/ctxmod/agentic/context::shared::remediation_procedure@workflows/reference/academic_revision.md`
```

**Workflow-specific notes**:
- **Conditional**: Triggers when Phase 6 reports Errors or Warnings exceeding threshold
- Each iteration: Opus remediation agent applies targeted corrections, then re-runs Phase 6 validation
- Remediation follows the academic revision procedure, scoped to validation findings only (no scope expansion)
- **Exit on success**: Zero Errors, fewer than 5 Warnings
- **Exit on exhaustion**: Report remaining issues, preserve all artifacts and reports, propose manual resolution

## Information Flow

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />

```
Phase 0: reports/<YYYY-MM>/<DD-HHMM>-<id>-00-classification.md
Phase 1: reports/<YYYY-MM>/<DD-HHMM>-<id>-01-discovery.md (conditional)
Phase 2: reports/<YYYY-MM>/<DD-HHMM>-<id>-02-research-papers.md (parallel, conditional)
         reports/<YYYY-MM>/<DD-HHMM>-<id>-02-research-surveys.md (parallel, conditional)
         reports/<YYYY-MM>/<DD-HHMM>-<id>-02-research-specs.md (parallel, conditional)
Phase 3: reports/<YYYY-MM>/<DD-HHMM>-<id>-03-analysis.md
Phase 4: reports/<YYYY-MM>/<DD-HHMM>-<id>-04-planning.md (conditional)
Phase 5: {OUTPUT_PATH} (the academic document or revision)
         reports/<YYYY-MM>/<DD-HHMM>-<id>-05-execution.md
Phase 6: reports/<YYYY-MM>/<DD-HHMM>-<id>-06-val-academic.md
Phase 7: reports/<YYYY-MM>/<DD-HHMM>-<id>-07-iter-{N}-remediation.md (loop)
         reports/<YYYY-MM>/<DD-HHMM>-<id>-07-iter-{N}-validation.md (loop)
```

## Execution Strategy

### Main Agent Behavior

1. **Classify** (Phase 0, direct): Read user request, classify task type, identify input artifacts and output target
2. **Discover** (Phase 1, conditional): Launch discovery sub-agent appropriate to task type
3. **Research** (Phase 2, conditional, parallel): Formulate queries, then launch three research sub-agents in a single message
4. **Analyze** (Phase 3): Launch analysis or synthesis sub-agent based on task type; Opus for synthesis tasks, Sonnet for correction analysis
5. **Plan** (Phase 4, conditional): Launch planning sub-agent for New Writing and Integration tasks
6. **Execute** (Phase 5, Opus): Launch academic writing sub-agent with all accumulated report paths
7. **Validate** (Phase 6): Launch academic validation sub-agent targeting Phase 5 output
8. **Decision**: Validation passes threshold -- complete. Findings exceed threshold -- enter Phase 7 revision loop (max 3 iterations, returns to Phase 6 after each remediation)
9. **Complete**: Return output path and validation summary

### Parameter Configuration

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

Additional parameters:

| Parameter | Required | Description |
|-----------|----------|-------------|
| `task_type` | Derived | Set by Phase 0 classification: `new_writing`, `correction`, `analysis`, `integration` |
| `citation_style` | No | Citation format: APA, MLA, Chicago, IEEE. Defaults to APA |
| `output_path` | Yes | Target path for the academic document |

## Conditional Phase Execution

| Phase | New Writing | Correction | Analysis | Integration |
|-------|-------------|------------|----------|-------------|
| Phase 0: Classification | Always | Always | Always | Always |
| Phase 1: Discovery | Yes | Yes | Yes | Yes |
| Phase 2: Research | Yes (broad) | Skip | Conditional | Yes (gap-filling) |
| Phase 3: Analysis | Opus synthesis | Sonnet analysis | Opus synthesis | Opus synthesis |
| Phase 4: Planning | Yes | Skip | Skip | Yes |
| Phase 5: Execution | Write document | Apply revisions | Write report | Write unified document |
| Phase 6: Validation | Always | Always | Always | Always |
| Phase 7: Revision Loop | Conditional | Conditional | Conditional | Conditional |

Phases 0, 1, 3, 5, and 6 always execute for all task types. Phases 2 and 4 skip based on task type. Phase 7 triggers only when validation findings exceed the acceptance threshold.

## Variants

### Correction-Only

When revising an existing document against feedback:

```
Classification → Discovery → Analysis → Revision → Validation → Revision Loop
```

Skips Phases 2 and 4. Discovery catalogs the existing document; analysis classifies findings; execution applies the revision procedure.

### Research-Heavy Writing

When a new paper requires extensive literature grounding:

```
Classification → Discovery → Research (3 parallel) → Synthesis → Planning → Writing → Validation → Revision Loop
```

Full pipeline with broad research. Consider using perplexity_deep_research for initial exploration before structured academic search.

### Quick Analysis

When summarizing provided sources without external research:

```
Classification → Discovery → Analysis → Report Writing → Validation
```

Skips Phase 2. Operates entirely on provided materials.

### Multi-Source Integration

When combining findings from several existing documents:

```
Classification → Discovery → Gap Research (parallel) → Synthesis → Planning → Integrated Writing → Validation → Revision Loop
```

Phase 2 focuses on filling gaps between input documents rather than broad literature search.

## Error Handling

| Situation | Response |
|-----------|----------|
| Task type ambiguous | Request clarification from user before proceeding |
| No sources found during discovery | Report gap, propose scope adjustment or alternative sources |
| Research returns insufficient results | Proceed with available material, note limitations in output |
| Revision loop exhausted (3 iterations) | Report remaining issues, preserve all artifacts, propose manual review |
| Citation verification fails | Flag specific citations, attempt correction in revision loop |
| Existing document not found (Correction) | Halt, report path error |

## Quality Criteria

| Criterion | Validation Method | Phase |
|-----------|-------------------|-------|
| Citation correctness | All citations resolve to real sources with accurate attribution | 6 |
| Evidence quality | Claims backed by peer-reviewed or authoritative sources | 6 |
| Source credibility | CRAAP framework scores above threshold for all cited sources | 6 |
| Bias acknowledgment | Detected bias indicators accompanied by explicit discussion | 6 |
| Structural compliance | Document follows disciplinary convention (IMRaD or equivalent) | 6 |
| Language standards | Academic register, hedging calibrated to evidence strength | 6 |
| Argument coherence | Logical flow from introduction through conclusion without gaps | 6, 7 |
| Claim proportionality | Strength of claims matches strength of supporting evidence | 6 |

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Researching for correction tasks | Correction operates on existing material and feedback | Skip Phase 2; analyze provided findings |
| Planning for analysis tasks | Analysis output structure follows synthesis, not a separate plan | Let Phase 3 synthesis determine structure |
| Skipping validation for "minor" corrections | Hidden issues propagate to final output | Always execute Phase 6 |
| Mixing revision passes | Structural changes invalidate sentence-level edits | Follow the four-pass revision sequence |
| Broad research when sources are provided | Wastes context budget on redundant material | Skip Phase 2 or limit to gap-filling |
| Reading intermediate reports into main agent | Violates context efficiency | Pass report paths between phases |
| Expanding revision scope beyond validation findings | Scope creep exhausts iteration budget | Fix only issues identified by validation |
| Single validation pass without revision loop | Unverified quality after corrections | Use validation loop for quality convergence |
