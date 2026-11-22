---
description: "Step-level validation mechanics — completeness, consistency, references, severity classification, standards, structural, and academic cross-validation and quality-assurance procedures."
binds_to_depth: procedure
---

# Validation Procedures

Execute a single, bounded validation procedure and return its result. Each
procedure below is a self-contained set of prerequisites, steps, and decision
points; the variant material lives in this skill's `reference/` directory.
Where flat (general) and academic variants exist, read only the variant
matching the assigned source type.

## Procedure Selection

Read the reference file matching the assigned task and source type. Do not
read all variants — each is independently applicable and reading extras
wastes context.

| Assigned task | Read (general) | Read (academic) |
|---------------|----------------|-----------------|
| Verify required element presence and coverage | `reference/completeness.md` | — |
| Verify cross-artifact alignment and terminology uniformity | `reference/consistency.md` | — |
| Verify cross-reference resolution and link integrity | `reference/references.md` | — |
| Assign severity levels to validation findings | `reference/severity-classification.md` | — |
| Verify convention, style, and best-practice adherence | `reference/standards.md` | — |
| Verify schema compliance and format requirements | `reference/structural.md` | — |
| Cross-check findings against independent evidence | — | `reference/cross-validation.md` |
| Apply structured checklists at source/citation/finalization decision points | — | `reference/quality-assurance.md` |

### Completeness Validation

Verify that an artifact covers all required topics, cases, or elements within
its defined scope. Detect gaps, missing coverage, and incomplete treatment.

For completeness validation, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation_procedure/reference/completeness.md" />

### Consistency Validation

Verify that content maintains alignment within an artifact and across related
artifacts. Detect contradictions, terminology drift, and conflicting guidance.

For consistency validation, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation_procedure/reference/consistency.md" />

### References Validation

Verify that all cross-references in an artifact resolve to valid targets.
Applies to any content containing links, citations, or pointers to other
resources.

For reference validation, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation_procedure/reference/references.md" />

### Severity Classification

Assign severity levels to validation findings to prioritize remediation.
Provides consistent categorization across validation types.

For severity classification, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation_procedure/reference/severity_classification.md" />

### Standards Validation

Verify that written content adheres to defined style, clarity, and expression
guidelines. Applies to any prose content governed by writing standards.

For standards validation, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation_procedure/reference/standards.md" />

### Structural Validation

Verify that artifacts conform to their defined format and structure
requirements. Applies to any artifact with structural expectations:
documents, configurations, data files, or reports. Cite both working-tree
and HEAD blob hashes for byte-identity claims (never a bare "matches
baseline").

For structural validation, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation_procedure/reference/structural.md" />

### Cross-Validation

Academic-only. Cross-validation confirms findings by checking them against
independent evidence. Single-source findings carry inherent risk; multi-
source validation builds confidence through data triangulation, method
triangulation, and source triangulation, plus replication assessment.

For academic cross-validation, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation_procedure/reference/cross_validation.md" />

### Quality Assurance

Academic-only. Systematic checklists for validating research sources and
outputs at decision points: before relying on a source, before citing a
finding, and before finalizing a document. Checklists catch systematic
errors that ad-hoc review misses.

For academic quality assurance, see:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::validation_procedure/reference/quality_assurance.md" />

## Execution Discipline

- Read only the variant matching the assigned source type; do not read
  all variants.
- Execute only the assigned procedure; do not expand into adjacent steps.
- Report results to the caller-provided path; return a summary, not the
  full body.
- Reference findings with `file:line` ranges rather than inlining content.
- Note explicitly when a procedure's prerequisites are unmet rather than
  improvising a substitute.
- For byte-identity claims, cite both working-tree and HEAD blob hashes
  alongside the producing command.

## Parent-Layer Dispatch

This procedure is dispatched by the shared `validation_objective`,
which in turn is dispatched by four sibling phase skills in the
validation capability:

| Phase skill | Procedures typically dispatched |
|-------------|---------------------------------|
| `validate_completeness_phase` | completeness, severity-classification |
| `validate_consistency_phase` | consistency, severity-classification |
| `validate_references_phase` | references, severity-classification |
| `validate_structure_phase` | structural, severity-classification |

Ad-hoc procedure-depth agents may also load this skill for general
validation step mechanics outside any of the four capability phase
chains.

- the /validate command selection guidance — which validation tasks
  route to this family versus the story-completion validator —
  <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::command_selection/reference/validate.md" />

## Cross-Cutting Reference

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/research_briefs.md" />
