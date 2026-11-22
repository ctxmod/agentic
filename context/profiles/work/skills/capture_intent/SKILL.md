---
description: "Multi-phase intent capture — raw transcription, contextual discovery, gap analysis, clarification via AskUserQuestion, and polished intent authoring."
binds_to_depth: main
---

# Capture Intent

Multi-phase intent capture — transcribe the user's raw input,
discover the surrounding context, analyze gaps, clarify with the
user, then author a polished intent document. The main agent loads
this skill and orchestrates phase-depth sub-agents end-to-end.

The pipeline is **iterative, not linear**. Phases 2-4 (discovery,
analysis, clarification) repeat as many times as needed until the
intent is clear, detailed, rigorous, exhaustive, and meticulously
written. Between rounds, the main agent infers as much as possible
between the lines of the user's input — only asking about things
that genuinely cannot be inferred.

The final intent must contain **no open questions** unless they are
truly undecidable by the user (they depend on things that will become
clear when working on fulfilling the intent). Before drafting the
final intent, anything that can be **tested** (e.g., running a tool
with specified flags, checking an API's behavior, verifying a file
exists) MUST be tested rather than guessed.

This is a **main-only orchestrator skill**. It is loaded directly by
the main agent (no `render-agent:` block, no launched paired agent);
the main agent executes the phases below itself, dispatching each
phase to the appropriate phase-depth agent. The `exclusivity.agents:
true` flag is the author's declaration that this orchestrator is the
exclusive path for intent capture — no other skill should own the
capture pipeline.

## When to Use

Use this skill when:

- Capturing user intent that needs structured refinement
- Requirements are ambiguous or underspecified
- Contextual discovery (codebase, patterns, prior work) informs the intent
- Polished, validated intent document is the deliverable

Do NOT use this skill when:

- User explicitly invokes a write sub-step directly (`/write_intent` etc.)
- Intent is already captured and only needs editing — load `intent_write` instead
- A single-phase write suffices with no discovery or clarification

## Orchestration Context

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
<ctxmod_inline path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />

## Phases

The main agent executes five phases. Each phase (except Phase 4)
dispatches a phase-depth sub-agent. Report paths are passed between
phases; report content is never inlined into the orchestrator.

```
Phase 1: Raw Intent Capture (intent_write)
         |
         v
Phase 2: Contextual Discovery (parallel fan-out)
         |
         v
Phase 3: Gap Analysis (analyze_gap_phase)
         |
         v
Phase 4: Clarification (main agent, AskUserQuestion)  <-- may run many rounds
         |
         +<------- repeat 2-4 as needed until intent is exhaustive
         |
Phase 5: Verify (test assumptions, run tools/APIs, check file existence)
         |
         v
Phase 6: Final Intent Write (intent_write)
```

**The pipeline is iterative.** Phases 2-4 repeat as many times as
needed. Each clarification round may surface new areas to discover
or analyze. The main agent decides when the intent is sufficiently
refined to proceed to Phase 5 (verify) and Phase 6 (final write).

### Phase 1: Raw Intent Capture

Launch the `intent_write` agent to transcribe the user's exact input
with typo and spelling corrections only — no augmentation, no
reorganization, no inferred content. The raw transcript is the
baseline for downstream phases.

- Output: `{INTENT_PATH}` (raw draft)

**Workflow-specific notes**:
- Transcribe only what the user said
- Correct spelling and typos; do not rephrase
- Preserve the user's ordering and emphasis
- Flag (do not resolve) obvious ambiguities inline as `[CLARIFY: ...]`

### Phase 2: Contextual Discovery

Main agent selects the discovery phase(s) relevant to the intent's
domain. Launch selected discovery agents in parallel.

- `discover_architecture_phase` — codebase structure, boundaries, layers
- `discover_patterns_phase` — recurring patterns, conventions
- `discover_dependencies_phase` — upstream/downstream dependencies
- `discover_consumers_phase` — consumers of the affected surface

- Reports: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-discovery-{variant}.md`

**Selection guidance**: most intents warrant one or two discovery
phases. A single-domain intent (e.g., "refactor the auth layer") may
need only `discover_architecture_phase`. Cross-cutting intents (e.g.,
"introduce a new data format") may need dependencies and consumers
too. Do not launch discovery variants that are obviously irrelevant.

### Phase 3: Gap Analysis

Launch `analyze_gap_phase` to identify open questions, ambiguities,
scope concerns, and underspecified areas in the raw intent given the
discovery context.

- Report: `{REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-gap-analysis.md`

**Workflow-specific notes**:
- Gaps should be specific (point at a section / concept), not vague
- Each gap proposes a concrete resolution path
- Distinguish binding gaps (block composition) from informational gaps (nice-to-have)

### Phase 4: Clarification (iterative)

The main agent uses `AskUserQuestion` directly. This phase runs in
**as many rounds as needed** — not just once. Each round:

1. Frame 1-4 questions from the current gap analysis
2. Each question has 2-4 concrete options; first option is recommended
3. After receiving answers, re-evaluate: do new answers surface
   additional gaps? If yes, loop back to Phase 2 or 3 for the new
   areas, then ask again
4. Between rounds, **infer as much as possible** — only ask about
   things that genuinely cannot be inferred from context, user
   input, or discovery findings

**Question-construction rules**:
- Each option is a concrete, actionable resolution (not "Other")
- First option is the recommendation; mark it with "(Recommended)"
- Skip gaps already resolved by discovery findings or inference
- Merge related gaps into a single question
- Frame questions to steer the user toward a clear, detailed,
  rigorous, exhaustive intent

**No open questions in the final intent** unless they are truly
undecidable by the user — they depend on things that will become
clear when working on fulfilling the intent.

The full clarification doctrine — the design tree, frontier rounds, facts-versus-decisions,
and the empty-frontier termination test — lives in <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::capture_intent/reference/clarification_frontier.md" />.

### Phase 5: Verify Assumptions

Before drafting the final intent, **test anything that can be tested**:

- If the intent describes using a tool with specific flags, **run the
  tool with those flags** to verify it works as assumed
- If the intent references an API, **check the API's actual behavior**
- If the intent assumes a file exists or a pattern is valid, **verify
  it on disk**
- If the intent assumes a dependency or type signature, **check the
  actual source code**

Record verified facts in the final intent as confirmed decisions.
Record failures as constraints ("tool X with flag Y does not work;
the intent must account for this").

### Phase 6: Final Intent Write

Launch `intent_write` again with Phase 1 raw transcript, Phase 2
discovery reports, Phase 3 gap report, and Phase 4 clarification
answers. The agent authors the polished intent document following
the structure in `ctx:github.com/ctxmod/agentic/context::shared::intent_write@work/reference/intent_structure.md`.

- Output: `{INTENT_PATH}` (final, overwrites raw draft)

**Workflow-specific notes**:
- Write final intent only; do not modify other files
- Include clarification decisions as numbered "Confirmed decisions"
- Open questions section captures anything unresolved after Phase 4
- Acceptance criteria must be atomic and verifiable

## Sub-Layer Launch

| Phase | Agent | Purpose |
|-------|-------|---------|
| Raw write | `intent_write` | Transcribe user input verbatim |
| Discovery | `discover_architecture_phase` (+ variants) | Understand surrounding context |
| Analysis | `analyze_gap_phase` | Identify gaps and open questions |
| Clarification | (main agent, AskUserQuestion) | Resolve binding gaps with user |
| Final write | `intent_write` | Author polished intent document |

Brief template for the `intent_write` agent — <raw-write | final-write> within intent capture (Phase 1 or 5):

```text
<ctxmod_activate skill="agentic_work_intent_write" />
Mode: <raw | final>
Intent path: {INTENT_PATH}
User input: {USER_INPUT}
[final only] Phase 1 raw: {INTENT_PATH}
[final only] Discovery reports: {Phase 2 returned report paths}
[final only] Gap report: {Phase 3 returned report path}
[final only] Clarification: {PHASE_4_ANSWERS}
```

Brief template for the `discover_architecture_phase` agent — Discovery for intent capture (Phase 2):

```text
<ctxmod_activate skill="agentic_workflows_discover_architecture_phase" />
Scope: {scope derived from user input}
Report to: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-discovery-<variant>.md
```

Brief template for the `discover_consumers_phase` agent — Discovery for intent capture (Phase 2):

```text
<ctxmod_activate skill="agentic_workflows_discover_consumers_phase" />
Scope: {scope derived from user input}
Report to: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-discovery-<variant>.md
```

Brief template for the `discover_dependencies_phase` agent — Discovery for intent capture (Phase 2):

```text
<ctxmod_activate skill="agentic_workflows_discover_dependencies_phase" />
Scope: {scope derived from user input}
Report to: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-discovery-<variant>.md
```

Brief template for the `discover_patterns_phase` agent — Discovery for intent capture (Phase 2):

```text
<ctxmod_activate skill="agentic_workflows_discover_patterns_phase" />
Scope: {scope derived from user input}
Report to: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-discovery-<variant>.md
```

Brief template for the `analyze_gap_phase` agent — Gap analysis for intent capture (Phase 3):

```text
<ctxmod_activate skill="agentic_workflows_analyze_gap_phase" />
Input: {INTENT_PATH} (raw)
Context reports: {Phase 2 returned report paths}
Report to: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-gap-analysis.md
```

## Information Flow

```
Phase 1: Raw Intent Capture
- Receives: USER_INPUT, INTENT_PATH
- Produces: {INTENT_PATH} (raw draft)

Phase 2: Contextual Discovery
- Receives: USER_INPUT, INTENT_PATH
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-02-discovery-{variant}.md

Phase 3: Gap Analysis
- Receives: INTENT_PATH (raw), Phase 2 report paths
- Produces: {REPORTS_BASE}/{date}/<DD-HHMM>-<id>-03-gap-analysis.md

Phase 4: Clarification
- Receives: Phase 3 gap report (path)
- Produces: PHASE_4_ANSWERS (in-memory, passed to Phase 5)

Phase 5: Final Intent Write
- Receives: INTENT_PATH, Phase 1 raw, Phase 2 reports, Phase 3 report, Phase 4 answers
- Produces: {INTENT_PATH} (final)
```

Main agent passes report **paths** between phases, never report
content.

## Execution Strategy

1. **Raw capture**: Launch `intent_write`; wait
2. **Discovery** (parallel): Launch selected discovery variants; wait
3. **Gap analysis**: Launch `analyze_gap_phase`; wait
4. **Clarify**: Main agent runs `AskUserQuestion`; collect answers
5. **Final write**: Launch `intent_write` with full context; wait
6. **Complete**: Return intent path and any open questions

## Workflow Variants

### Lightweight Capture

Skip Phase 2 (discovery) when:

- User input is self-contained and codebase-independent
- Intent concerns a brand-new surface with no existing context
- All needed context is in the user's original message

```
Raw Capture → Gap Analysis → Clarification → Final Write
```

### Research-Heavy Capture

Expand Phase 2 with research phases (e.g., `research_practices_phase`,
`scholarly_research_phase`) when:

- Intent involves external standards or best practices
- Comparative analysis across prior art informs the design
- Multiple independent domains require investigation

### Synthesis Capture

Skip Phase 4 clarification only when the conversation already settled
the decisions it would ask; every binding decision stays traceable to
the conversation, and Phase 5 verification remains mandatory — unlike
Lightweight Capture, which skips discovery rather than clarification.

## Error Handling

| Situation | Response |
|-----------|----------|
| Discovery finds nothing relevant | Skip to gap analysis with empty context |
| Gap analysis finds no binding gaps | Skip Phase 4 clarification, go straight to Phase 5 |
| User declines to answer clarification | Capture as open question in final intent |
| Composition fails | Inspect augmentation inputs, retry once, then report |

Error handling follows the stop-report-propose-await pattern.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Augmenting during raw capture | Corrupts the baseline | Phase 1 transcribes; Phase 5 augments |
| Inlining report content into orchestrator | Bloats context | Pass report paths only |
| Asking vague clarification questions | User cannot answer concretely | Frame as 2-4 concrete options |
| Skipping gap analysis | Missed binding gaps | Always run Phase 3 before clarifying |
| Final write without discovery context | Uninformed intent | Always pass Phase 2/3 outputs to Phase 5 |
| Returning intent content from skill | Bloats main agent context | Return INTENT_PATH only |

## Cross-Workflow Reference

Other domains' workflow agents may reference this skill to "capture
intent" as a sub-step. The main agent loads this skill and runs the
phases; the launching workflow receives the final INTENT_PATH.

## Cross-Cutting Reference

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile

Depth-scoped guidance for the sub-agents this orchestrator launches:

the `intent_write` skill
the `discover_architecture_phase` skill
the `analyze_gap_phase` skill
