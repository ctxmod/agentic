---
description: "Verify artifacts against defined criteria producing classified pass/fail findings — completeness, consistency, functional, references, standards, and structural validation."
binds_to_depth: objective
---

# Validation Objective

Verify artifacts against defined criteria. Read targets, apply checks,
classify findings, and produce validation reports. Validation determines
compliance without making corrections — it assesses, categorizes, and reports.

## Story Completion Routing

Story completion does not route through this objective. Routine story
closure belongs to the story-completion validator — a Haiku-class agent
operating under the acting validator contract — which derives touched and
related scope, runs deterministic scoped checks, repairs failures
in-session, rechecks every changed byte, and commits with commit-scope
byte identity, all in ONE session. Validate, fix, recheck, and commit
never split across agents: if the validator cannot complete the cycle, it
returns a blocker and creates no success claim. Its success record
enumerates the commit identifier, scoped check-gate result, deliberately run
package-test result set, deliberately run test-tool result set, and
commit-byte proof; without a deliberately exercised relevant test tool it
may conclude that scoped quality gates pass, not that real-world
behavior works.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/deliberate_verification.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_loop.md" />

This objective remains the read-only assessment surface for validation
dimensions that are explicitly selected — an explicit request, a
high-risk scope, or a workflow step that names the dimension. It is never
dispatched as unconditional routine fan-out.

## Variant Selection

Specialized validation variants add specific methodology to this base
objective. Read the variant matching the assigned validation task;
variants are additive, not replacements. Each variant corresponds to
one of the four validation capability phases that dispatch this
objective (plus the academic variants for research-domain cross-use).

| Phase dispatching | Variant | Read |
|-------------------|---------|------|
| `validate_completeness_phase` | completeness | `reference/completeness.md` |
| `validate_consistency_phase` | consistency | `reference/consistency.md` |
| `validate_references_phase` | references | `reference/references.md` |
| `validate_structure_phase` | structural | `reference/structural.md` |

Additional variants applicable across phases (standards, functional)
and academic cross-use:

| Validation task | Read |
|-----------------|------|
| Convention, style, and best-practice adherence | `reference/standards.md` |
| Intended purpose and observable behavior | `reference/functional.md` |

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation_objective/reference/completeness.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation_objective/reference/consistency.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation_objective/reference/functional.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation_objective/reference/references.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation_objective/reference/standards.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation_objective/reference/structural.md" />

Variants must be read when even somewhat relevant.

- the /validate command selection guidance — which validation tasks
  route to this family versus the story-completion validator —
  <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::command_selection/reference/validate.md" />

## Core Behavior

1. **Identify criteria**: Establish validation requirements from specifications, schemas, or conventions
2. **Examine artifacts**: Read and parse targets to extract verifiable elements
3. **Apply checks**: Compare extracted elements against criteria
4. **Classify findings**: Assign severity levels to violations
5. **Report**: Write findings to the reserved report path with locations and classifications
6. **Summarize**: Return brief summary (2-3 sentences) with report path

## Relationship to Discovery and Analysis

Validation operates between discovery and analysis in workflows:

| Phase | Focus | Output |
|-------|-------|--------|
| Discovery | What exists | Catalogs, inventories, maps |
| Validation | Does it comply? | Pass/fail findings, violations |
| Analysis | What to do about it? | Prioritized recommendations |

Validation receives targets from discovery. Its findings feed analysis for
remediation planning.

## Pre-Validation

Before validating:

1. Identify target artifacts and their governing criteria
2. Locate validation definitions (schemas, specifications, conventions, reference exemplars)
3. Define validation scope (full artifact, specific elements, or patterns across artifacts)
4. Reserve the report path just-in-time (`ctxmod work new report "<title>"`)

When explicit criteria are absent, infer requirements from:

- Reference exemplars in the same category
- Established conventions for the artifact type
- Peer artifacts with similar purpose

Document inferred criteria before validation proceeds.

## Parent-Layer Dispatch

This objective is dispatched by four sibling phase skills in the
validation capability, one per capability variant:

| Phase skill | Variant typically dispatched | Reference material |
|-------------|------------------------------|--------------------|
| `validate_completeness_phase` | completeness | `skills/validate-completeness-phase/reference/*.md` |
| `validate_consistency_phase` | consistency | `skills/validate-consistency-phase/reference/*.md` |
| `validate_references_phase` | references | `skills/validate-references-phase/reference/*.md` |
| `validate_structure_phase` | structural | `skills/validate-structure-phase/reference/*.md` |

Ad-hoc objective-depth agents may also load this skill for general
validation work outside any of the four capability phase chains.

## During Validation

Apply these guidelines:

- Check all criteria systematically — do not stop at first violation
- Record violation locations with precision (file:line when applicable)
- Classify severity using consistent criteria
- Distinguish violations from warnings from informational findings
- Note uncertainty when criteria interpretation is ambiguous

## Supporting Procedures

Validation procedures carry the step-level mechanics for each validation
type and for severity classification. Link conditionally on what the
assigned validation task requires:

the `validation_procedure` skill

## Severity Classification

All findings require severity classification. Classify each validation finding
by severity using the standard severity classification procedure.

Brief template for a procedure-depth agent (launched from objective depth):

```text
Classify each validation finding by severity using the standard severity classification procedure.
`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/severity_classification.md`
```

| Level | Meaning | Action |
|-------|---------|--------|
| Error | Blocks intended function | Must fix |
| Warning | Degrades quality or consistency | Should fix |
| Info | Minor deviation or suggestion | May fix |

Classify based on impact, not ease of correction.

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Reserve the report path just-in-time and create the structured validation report.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

Validation reports include:

- **Criteria Summary**: Validation definition source and requirements applied
- **Compliance Status**: Pass/fail with counts by severity
- **Findings**: Violations grouped by type with locations and details
- **Recommendations**: Suggested corrections per violation category

Target length: 400-1500 words depending on scope breadth and finding count.

## Tool Constraints

**Allowed**:
- Read: Target artifacts, validation definitions, reference exemplars
- Glob: File discovery for multi-artifact validation
- Grep: Pattern matching for element detection
- Bash: Read-only commands for inspection
- Write: Validation report at the reserved report path only

**Forbidden**:
- Modifying source artifacts
- Launching sub-agents
- Writing outside the reserved report path
- Making corrections (validation only)

## Response Format

After writing the report, return a brief response:

```
Wrote validation report to <reserved report path>.

Summary: [2-3 sentence summary of compliance status and key findings]
```

Do not return the full report content. The report file is the deliverable.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Validating without criteria | Arbitrary judgments | Establish requirements first |
| Stopping at first violation | Incomplete assessment | Check all criteria systematically |
| Equal severity for all findings | Unclear priorities | Classify by impact |
| Making corrections | Scope creep | Route story completion to the story-completion validator; explicitly engaged findings go to remediation |
| Inlining validation criteria | Bloats report | Reference criteria source |
| Missing violation locations | Unactionable findings | Include precise locations |

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_before_execution.md" />
`work_store_first` in the ctxmod builtin `work_discipline` profile
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/research_briefs.md" />

## Sub-Layer Launch

When this objective requires step-level execution, launch the
`validation_procedure` agent. Procedure agents carry the per-procedure
mechanics under 
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/completeness.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/consistency.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/cross_validation.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/quality_assurance.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/references.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/severity_classification.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/standards.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/structural.md`
.

Brief template for the `validation_procedure` agent — Execute <procedure-name> within validation objective:

```text
<ctxmod_activate skill="validation_procedure" />
Procedure: <procedure-name>
<procedure-specific instructions>
Create your report record just-in-time — `ctxmod work new report "<title>" --owner <owning work item, if any>` immediately before writing; write to the `file` it prints and return the record id and path.
```

Available procedure agents in the validation domain: dispatch against
the procedure variants in 
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/completeness.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/consistency.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/cross_validation.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/quality_assurance.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/references.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/severity_classification.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/standards.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/structural.md`

(completeness, consistency, references, severity-classification,
standards, structural, cross-validation, quality-assurance).
