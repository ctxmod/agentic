---
description: "Writing-phase dispatch guidance — produce artifacts where prose quality is the primary deliverable. Always Opus except report variant."
binds_to_depth: phase
---

# Writing Phase

Produce artifacts where writing is the primary action. Writing differs
from execution: execution modifies existing artifacts or applies
changes; writing produces new content where prose quality matters.
Writing phases always use Opus for the actual writing sub-agent
(except the `report` variant, which synthesizes from existing
content).

## When to Use

Use writing phase when:

- Primary output is new written content
- Prose quality directly affects artifact value
- Content requires synthesis and articulation
- Schema compliance and structure matter
- The artifact is the deliverable (not a side effect)

Writing is distinct from execution. Use writing for documents, work
items, specifications, reports. Use execution for code changes,
configuration modifications, file operations.

## Variants

Specialized writing variants exist for artifact categories. The
pre-decomposition variant references under `reference/*.md` (general,
docs, academic, report, work-item) remain the dispatch templates for
ad-hoc objective-depth or procedure-depth agents loading this skill
directly.

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::writing_phase/reference/academic.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::writing_phase/reference/docs.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::writing_phase/reference/general.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::writing_phase/reference/report.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::writing_phase/reference/work_item.md" />

### Variant Selection

Read the reference file matching the artifact category. Each variant
is independently applicable.

| Variant | When | Read |
|---------|------|------|
| General writing | Most writing needs where prose matters; no specialized schema | `reference/general.md` |
| Documentation | Creating/updating documentation; uses documentation procedures | `reference/docs.md` |
| Academic | Papers, literature reviews, scholarly text with citation conventions | `reference/academic.md` |
| Report | Reports synthesizing findings from input sources (no Opus override) | `reference/report.md` |
| Work item | Work items requiring schema structure and quality prose | `reference/work_item.md` |

### Capability Phase Skills (Shared-Methodology Decision)

The four capability phase skills absorb the workflow orchestration and
command bodies for the writing domain. Each capability phase links
this `writing_phase` skill as its shared methodology reference (model
choice, coordination pattern, transition criteria, anti-patterns).
This is the recorded resolution: **`writing_phase` survives as shared
methodology** linked from each capability phase, rather than folding
into each capability phase's body. Rationale: the variant-selection
reference (`reference/*.md`) is cross-capability shared material
consumed by 2+ capability phases and by ad-hoc objective/procedure
agents — satisfying the Q1 §1 step-3 escape hatch (cross-capability
reuse).

| Capability phase | Absorbs | Slash-commands |
|------------------|---------|----------------|
| `write_report_phase` | Report writing workflow | `/writing_report` |
| `write_item_phase` | Work item writing workflow (4 variants) | `/write-epic`, `/write-story`, `/write-issue`, `/write-task` |
| `write_context_phase` | Context content writing workflow (5 variants) | `/write-reference`, `/write-rule`, `/write-skills`, `/write-agent`, `/write-command` |

Intent writing moved out of the writing domain to its own `intent`
domain. The `intent_write` phase skill (at the `intent_write` skill)
absorbs the former `write-intent-phase` workflow and is launched
either standalone via `/write_intent` or end-to-end through the
`capture_intent` orchestrator skill (at
the `capture_intent` skill).

## Model Selection

**Primary**: Opus.

Writing requires careful reasoning about structure, clarity, and
prose quality. Opus provides the capability needed for high-quality
written artifacts.

**Exception — Report variant**: report writing synthesizes from
existing content rather than producing original prose, so the default
model provides sufficient capability. All other variants use Opus.

**No other exceptions**: If cost is a concern, reduce scope rather
than model capability.

## Coordination Pattern

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/sequential.md" />

Writing typically follows sequential pattern, proceeding after
discovery and analysis provide the content foundation.

For multiple independent artifacts sharing the same context, parallel
writing is possible but each writing agent uses Opus.

## Context Requirements

**Parameters provided by main agent**:

| Parameter | Required | Purpose |
|-----------|----------|---------|
| OUTPUT_PATH | Yes | Where to write the artifact |
| INPUT_REPORT | If applicable | Prior phase report providing content |
| PARENT_PATH | For child items | Parent work item for context |
| Work item context | If applicable | EPIC_PATH, STORY_PATH, ISSUE_PATH |

## Output Format

Writing produces the artifact directly at OUTPUT_PATH. No separate
report is generated — the artifact is the deliverable.

Sub-agent response:

```
Wrote {artifact type} to {OUTPUT_PATH}.

Summary: {2-3 sentence description of content}
```

## Transition Criteria

Writing completes and transitions when:

- Artifact is written to OUTPUT_PATH
- Content follows schema structure
- Prose meets clarity standards
- Summary confirms completion

If writing cannot produce compliant artifact, report the issue and
await guidance.

## Quality Standards

Writing sub-agents must apply:

- `ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/clarity.md`
- `ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/anti_patterns.md`
- `ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/academic_register.md`
- `ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/information_hierarchy.md`

Key standards:

- Active voice (80%+ of sentences)
- Concrete language over vague terms
- No AI writing anti-patterns
- Sentence variety (length and structure)
- Schema compliance
- Calibrated to the least-specialist intended reader

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Using Sonnet for writing | Quality degradation | Always use Opus (except `report` variant) |
| Writing without schema | Inconsistent structure | Reference schema first |
| Skipping clarity standards | AI-sounding prose | Apply writing style rules |
| Returning artifact content | Bloats orchestrator | Return path only |
| Combining writing with execution | Mixed concerns | Separate phases |
| Writing child items | Scope creep | Separate writing objectives |
| Re-routing mid-task after variant selection | Wasted work, schema mismatch, inconsistent artifacts | Stick with the selected variant; if the ask was genuinely ambiguous, pick one, state the choice in one line, continue. |

## Source-Path Note

The phase files in this skill's `reference/` directory derive from the
legacy writing-phase corpus (5 files, one per variant) plus its sibling
index (folded into this SKILL.md body). The original directory shape
(one file per variant) is preserved; in-domain objective links point
at the `writing_objective` skill and the `reporting` package.
Cross-domain consumers of the writing phase files were enumerated for a
later horizontal sweep.

**Divergence note:** Writing's phase content stands as its own
skill (Option A) rather than collapsing into the workflow skill
(Option B) because the 5 phase variant files are consumed by **three
sibling workflows** — the writing, academic work, and work-item
workflows — satisfying the cross-workflow-reuse escape hatch of the
consolidation rules. This was the first Option-A divergence in the
skill consolidation; it is recorded in that consolidation's execution
report with evidence.

## Objective Context

The objective these phases serve:

the `writing_objective` skill

## Cross-Cutting Reference

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`work_store_first` in the ctxmod builtin `work_discipline` profile

## Sub-Layer Launch

When this phase requires objective-depth execution, launch the
`writing_objective` agent. Objective agents carry the per-artifact
schema and creation methodology under 
- `ctx:github.com/ctxmod/writing_context/context::shared::artifacts@authoring/reference/agents_authoring.md`
- `ctx:github.com/ctxmod/writing_context/context::shared::artifacts@authoring/reference/commands_authoring.md`
- `ctx:github.com/ctxmod/writing_context/context::shared::artifacts@authoring/reference/reference_authoring.md`
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports.md" />
- `ctx:github.com/ctxmod/writing_context/context::shared::artifacts@authoring/reference/rules_authoring.md`
- `ctx:github.com/ctxmod/writing_context/context::shared::artifacts@authoring/reference/skills_authoring.md`
- `ctx:github.com/ctxmod/agentic/context::shared::items@work/reference/work_items.md`
.

Brief template for the `writing_objective` agent — Produce <artifact-type> within writing phase:

```text
<ctxmod_activate skill="agentic_workflows_writing_objective" />
Artifact: <artifact-type>
<objective-specific instructions>
Write to {OUTPUT_PATH}.
```

Available objective agents in the writing domain: dispatch against the
artifact-category variants in 
- `ctx:github.com/ctxmod/writing_context/context::shared::artifacts@authoring/reference/agents_authoring.md`
- `ctx:github.com/ctxmod/writing_context/context::shared::artifacts@authoring/reference/commands_authoring.md`
- `ctx:github.com/ctxmod/writing_context/context::shared::artifacts@authoring/reference/reference_authoring.md`
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports.md" />
- `ctx:github.com/ctxmod/writing_context/context::shared::artifacts@authoring/reference/rules_authoring.md`
- `ctx:github.com/ctxmod/writing_context/context::shared::artifacts@authoring/reference/skills_authoring.md`
- `ctx:github.com/ctxmod/agentic/context::shared::items@work/reference/work_items.md`

(reports, work-items, agents, commands, reference, rules, skills).
