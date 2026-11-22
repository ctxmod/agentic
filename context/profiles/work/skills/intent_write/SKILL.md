---
description: "Write intent documents — raw transcription and polished authoring with structured sections, discovery synthesis, gap remediation, and acceptance criteria."
binds_to_depth: phase
---

# Intent Write

Intent writing phase — produce validated intent files capturing
requirements, constraints, technical specifications, and acceptance
criteria. This phase serves two call sites: (1) the main agent
loading the `capture_intent` skill dispatches it for both raw
transcription (Phase 1) and final authoring (Phase 5); (2) any
other domain's workflow agent references this phase as a
"write intent" sub-step.

Intent writing is a **phase, not a workflow**. The main agent invokes
it directly via the absorbed `/write_intent` slash-command, the
`/write` router, or the `capture_intent` orchestrator skill.

Intent path construction: `ctxmod work new intent "<title>" [--owner <most specific work item>]` prints INTENT_PATH (`<owner item dir>/intents/<YYYY-MM>/<DD-HHMM>-<id>-<slug>.md`). An intent is a source: its payload is written once and sealed (`ctxmod work touch <id>`).

**Base path derivation**:

| Most specific context | INTENT_BASE | REPORTS_BASE |
|-----------------------|-------------|--------------|
| TASK_PATH provided | the task's `intents/` (`--owner <task id>`) | the task's `reports/` |
| ISSUE_PATH provided (no task) | the issue's `intents/` | the issue's `reports/` |
| STORY_PATH provided (no issue) | the story's `intents/` | the story's `reports/` |
| EPIC_PATH provided (no story) | the epic's `intents/` | the epic's `reports/` |
| No work item context | the store's top-level `intents/` | the store's top-level `reports/` |

<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

## When to Use

Use this phase when:

- Producing intent files for work item specification
- Requirements, constraints, specifications, acceptance criteria needed
- Multi-source research informs the intent
- Quality verification across structure, content, consistency required
- Editing existing intent files (`--edit` mode)
- The `capture_intent` orchestrator dispatches raw-write or final-write

## Phase Orchestration

This phase dispatches objective-depth sub-agents through the shared
`writing_objective`, with intent-specific reference material scoping
each dispatch. The `writing_phase` methodology layer carries the
variant-selection reference (model choice, coordination pattern,
transition criteria, anti-patterns).

When invoked standalone (not through `capture_intent`), this phase
runs its own six-step pipeline:

```
Phase 0: Preflight (main agent)
         |
Phase 1: Clarification Discovery (Sonnet)
         |
Phase 2: Research (Parallel Fan-Out + Synthesis)
         |
Phase 3: Intent Augmentation (Sonnet)
         |
Phase 4: Intent Composition (Opus)
         |
Phase 5: Validation (Sonnet, parallel)
         |
Phase 6: Remediation (conditional loop)
```

When invoked by `capture_intent`, the orchestrator has already
produced raw transcript, discovery, and gap context — this phase
runs only the composition (Phase 4) and validation (Phases 5/6)
sub-steps in final-write mode, or the raw-transcription sub-step
alone in raw-write mode.

### Phase 0: Preflight (main agent)

Main agent performs directly (no sub-agent):

1. Capture current date/time
2. Derive the owner (INTENT_BASE and REPORTS_BASE) from work item context hierarchy
3. Generate the title (and so the slug) from user input description
4. Create the intent record: `ctxmod work new intent "<title>" [--owner <owner id>]` — its printed `file` is INTENT_PATH
5. If `--edit` provided: set EDIT_MODE=true, EXISTING_INTENT={path or id}; otherwise EDIT_MODE=false. Because an intent payload is sealed once written, an edit produces a NEW intent record that supersedes the existing one and cites its id
6. No directories to create: the store creates them with each record

**Output parameters**: INTENT_PATH, REPORTS_BASE, EDIT_MODE,
EXISTING_INTENT (if editing), USER_INPUT, date, time, slug.

### Phase 1: Clarification Discovery

Analyze user input to identify ambiguities, missing specifications,
and areas requiring investigation.

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-clarification.md`

**Workflow-specific notes**:
- Discovery only; do not write intent file
- Focus on ambiguities, conflicts, missing context, unclear specifications
- If EDIT_MODE=true, compare USER_INPUT against EXISTING_INTENT
- Do not pause to ask user clarifying questions; identify what needs investigation
- Identify missing items in each category: requirements, constraints, specs, acceptance criteria
- Flag specific research queries for Phase 2

### Phase 2: Research (Fan-Out + Synthesis)

Investigate unclear items through multi-source research, then
synthesize findings.

**Phase 2a: Research Execution** — this phase agent writes the intent
file and is **not** granted `allow-parallel-agents: true`, so launch
these one at a time, in the foreground, waiting for each. If the
concurrency instruction in your own agent-depth instructions ever grants
concurrent dispatch, these read-only searches may share a single
message:

- Agent 1: Documentation Research — search documentation directories,
  reference files, principles. Cite specific files and sections.
- Agent 2: Work Item Context Research — search work item directories
  for context, traceability, dependencies. Follow `parent` fields
  and body cross-references.
- Agent 3 (conditional): External Research — launch only if Phase 1
  identifies need for external knowledge. Use web search and academic
  search tools.

- Reports: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02a-research-{documentation,work-items,external}.md`

**Phase 2b: Research Synthesis** — aggregate after all 2a agents
complete:

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02b-research-synthesis.md`

**Workflow-specific notes**:
- Synthesis only; do not write intent file
- Aggregate findings, resolve contradictions, prioritize relevance
- Identify gaps that remain after research
- Assess confidence levels for synthesized conclusions
- Produce structured output organized by intent section

### Phase 3: Intent Augmentation

Integrate clarification and research findings with user input to
produce a comprehensive augmentation plan.

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-augmentation.md`

**Workflow-specific notes**:
- Analysis and augmentation planning only; do not write intent file
- If EDIT_MODE=true: identify conflicts between new requirements and existing intent
- Ensure intent captures ALL functional requirements, constraints, technical specifications, acceptance criteria
- Produce structured augmentation plan for composition phase
- Document conflict resolution strategy explicitly when editing

### Phase 4: Intent Composition (Opus)

Write the formatted intent file with proper structure and content.

- Output: `{INTENT_PATH}`

**Workflow-specific notes**:
- Write intent file only; do not modify other files
- Follow the intent document structure documented in <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::intent_write/reference/intent_structure.md" />
- Format content for clarity and density
- Include suggested improvements from augmentation report
- If EDIT_MODE=true: merge changes while preserving valid existing content
- Ensure all sections from augmentation plan are present

### Phase 5: Validation

Before accepting the intent file, apply one consumer-perspective read
per creation.md step 8: a work-item author opening the intent file cold
can act on it without reading anything else. The main agent applies this
check directly; it is a preamble check, not a fourth validation agent.

Validate intent file across three independent dimensions. Launch the
three agents one at a time, in the foreground, unless the concurrency
instruction in your own agent-depth instructions grants concurrent
dispatch.

- Agent 1: Structure Validation — metadata fields, section presence, heading hierarchy
- Agent 2: Content Validation — anti-patterns, clarity, completeness
- Agent 3: Consistency Validation — contradictions, reference resolution, work item links

- Reports: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-05-validation-{structure,content,consistency}.md`

### Phase 6: Remediation (Conditional Loop)

Execute only if any Phase 5 validation reports contain failures.
Maximum 3 iterations.

**Loop execution**:
1. All Phase 5 validations pass: skip Phase 6, workflow complete
2. Any failures: launch remediation agent
3. After remediation: re-launch all three Phase 5 validation agents
4. All re-validations pass: workflow complete
5. Failures remain and iterations < 3: return to step 2
6. Iterations = 3: proceed with best-effort intent file, report unresolved issues

## Raw-Write Sub-Mode

When invoked by `capture_intent` in raw-write mode, this phase
executes only a transcription step:

- Read USER_INPUT verbatim
- Apply typo and spelling corrections only
- Preserve user ordering and emphasis
- Flag (do not resolve) obvious ambiguities inline as `[CLARIFY: ...]`
- Write to `{INTENT_PATH}` and return the path

Raw-write does NOT run Phases 1-6. It is the baseline for downstream
capture phases (discovery, gap analysis, clarification, final write).

## Information Flow

```
Phase 0: Preflight
- Receives: USER_INPUT, work item context
- Produces: INTENT_PATH, REPORTS_BASE, EDIT_MODE, slug

Phase 1: Clarification Discovery
- Receives: USER_INPUT, EDIT_MODE, EXISTING_INTENT
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-01-clarification.md

Phase 2a: Research (parallel)
- Receives: Phase 1 output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02a-research-{documentation,work-items,external}.md

Phase 2b: Research Synthesis
- Receives: Phase 2a outputs
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02b-research-synthesis.md

Phase 3: Intent Augmentation
- Receives: Phase 1 output, Phase 2b output
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-augmentation.md

Phase 4: Intent Composition
- Receives: Phase 3 output
- Produces: {INTENT_PATH}

Phase 5: Validation (parallel)
- Receives: INTENT_PATH
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-05-validation-{structure,content,consistency}.md

Phase 6: Remediation (conditional)
- Receives: Validation reports, INTENT_PATH
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-06-remediation-{iteration}.md
```

Main agent passes report **paths** between phases, never report content.

## Execution Strategy

1. **Preflight**: Main agent derives paths, slug, mode
2. **Clarification**: Launch discovery sub-agent; wait
3. **Research**: Launch 2-3 research sub-agents, one at a time by default; wait
4. **Synthesis**: Launch synthesis sub-agent with research outputs; wait
5. **Augmentation**: Launch analysis sub-agent; wait
6. **Composition**: Launch writing sub-agent (Opus); wait
7. **Validation**: Launch three validation sub-agents, one at a time by default; wait
8. **Conditional Remediation**: If failures, remediate, re-validate (max 3)
9. **Complete**: Return intent path and validation summary

## Workflow Variants

### Edit Mode

When `--edit` flag provided:
- Phase 1 compares new input against existing intent
- Phase 3 identifies conflicts, proposes resolution strategy
- Phase 4 merges changes, preserving valid existing content, into a new
  intent record that cites the superseded one (the existing payload is
  sealed and never rewritten)

### Lightweight Intent

Skip Phase 2 (research) when:
- User input provides comprehensive specification
- Topic is internal to the context tooling (ctxmod) itself
- No external knowledge needed

```
Preflight → Clarification → Augmentation → Composition → Validation
```

### Research-Heavy Intent

Expand Phase 2 with additional research agents when:
- Multiple independent domains require investigation
- External standards or best practices inform the intent
- Comparative analysis across work items needed

## Error Handling

| Situation | Response |
|-----------|----------|
| Clarification finds fatal ambiguity | Report ambiguity, await decision |
| Research finds contradictory sources | Report contradiction, await decision |
| Validation exceeds iterations | Report persistent issues, await decision |
| Composition fails | Check augmentation output, await decision |

Error handling follows stop-report-propose-await pattern.

## Quality Criteria

| Criterion | Validation Method |
|-----------|-------------------|
| Structure compliance | Phase 5 checks metadata, sections, hierarchy |
| Content completeness | Phase 5 verifies requirements, constraints, specs, criteria |
| Internal consistency | Phase 5 checks contradictions, reference resolution |
| Work item link validity | Phase 5 verifies work item IDs exist |
| Clarity and anti-patterns | Phase 5 applies clarity principles |

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Skipping clarification | Ambiguous intent | Clarify ambiguities first |
| Skipping research | Uninformed intent | Research when external knowledge needed |
| Single validation pass | Unverified quality | Use validation loop |
| Composing without augmentation | Unstructured content | Augment before composing |
| Returning intent content | Bloats orchestrator | Return path only |
| Modifying files outside INTENT_PATH | Scope violation | Write only to INTENT_PATH |

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`writing_objective` agent. The objective in turn launches
`writing_procedure` for step mechanics.

Brief template for the `writing_objective` agent — Execute <phase-name> within intent writing:

```text
<ctxmod_activate skill="agentic_workflows_writing_objective" />
Artifact category: intent
Variant: intent
Phase-specific instructions:
  <phase-specific parameters>
Write to {INTENT_PATH} or reserve a report path just-in-time (`ctxmod work new report "<title>"`) and write there.
Reference: skills/intent/write/reference/intent-structure.md
```

## Cross-Workflow Reference

Other domains' workflow agents may reference this phase directly to
"write intent" as a sub-step. Launch the `intent_write` agent from
any workflow that needs intent file creation. The `capture_intent`
orchestrator skill is the recommended entry point for end-to-end
intent capture; this phase is the composition primitive underneath.

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

The other three writing capability phase skills:

the `write_report_phase` skill
the `write_item_phase` skill
the `write_context_phase` skill (`github.com/ctxmod/writing_context/context`, `authoring` profile)
