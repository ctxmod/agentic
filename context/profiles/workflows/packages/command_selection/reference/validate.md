# Validate Command Selection Guidance

Selection guidance for the `/validate` command family. Routine story
completion does NOT route here: it routes to the story-completion
validator — a Haiku-class agent that derives scope, runs deterministic
oracles, repairs scoped failures in-session, rechecks every changed
byte, and commits in one session. The specialized commands below are
reserved for explicit high-risk needs or explicit user requests; they
are not the normal path. Execution lives in the validation phase skills
(`validate_structure_phase`, `validate_references_phase`,
`validate_consistency_phase`, `validate_completeness_phase`) and the
shared `validation_objective`/`validation_procedure` layers.

> **Concurrency gate.** Where this document describes agents running "in parallel" or "concurrently", that shape is available only to agents whose resolved `allow-parallel-agents` posture is `true` — the read-only discovery, analysis, research, review, and validation phase agents. The concurrency instruction in your own agent-depth instructions is authoritative: if it tells you to use one agent at a time, run the same steps sequentially in the listed order. The phase decomposition, report paths, and synthesis steps are unchanged either way.

## Routing

| Situation | Route |
|-----------|-------|
| Story completion | Story-completion validator — one Haiku-class session: validate, fix, recheck, commit |
| Simple task, low instruction-following risk | Local deterministic checks in-tree; no validator dispatch |
| User explicitly requests comprehensive review | `/validate` |
| Explicit high-risk single dimension | `/validate:<dimension>` |

The story-completion path is the acting validator contract, not this
command family: deterministic oracles over the touched and related
scope, in-session repair, and commit-scope byte identity. Subjective
four-way validator panels are not a quality gate for ordinary work.

## Variant Comparison

`/validate` dispatches to the comprehensive workflow covering
**structure**, **references**, **consistency**, and
**completeness**. Use specialized commands for single-dimension
validation. Both are reserved for explicit engagements.

| Command | Scope | When to Use |
|---------|-------|-------------|
| `/validate` | All dimensions | Explicitly requested quality gates, pre-publication, compliance assessment |
| `/validate_structure` | Schema compliance | Format verification, structural standards |
| `/validate_references` | Link integrity | Cross-reference verification, link checking |
| `/validate_consistency` | Cross-artifact alignment | Terminology audits, naming conventions |
| `/validate_completeness` | Coverage verification | Acceptance criteria, required elements |

## Work Item Context

Reports nest within work item if context provided.

When no context provided:
- Validates against schema requirements and conventions
- Reports write to the store's top-level `reports/` (no `--owner`)
- Explicit path argument recommended

## Workflow Shape

When `/validate` is explicitly invoked, the comprehensive validation
workflow (`validation-workflow` agent) executes four phases:

1. **Scope Discovery** (sequential): Identify artifacts and applicable
   criteria
2. **Parallel Validation** (parallel): Four dimensions run concurrently
   — structure, references, consistency, completeness
3. **Finding Aggregation** (sequential): Deduplicate and identify
   systemic patterns
4. **Report Synthesis** (sequential): Unified validation report

If `--remediate` flag is specified, the workflow adds a remediation
loop after Phase 4.

## Resource Cost Notes

Comprehensive validation runs **4 parallel validation agents** across
all dimensions. That cost is why it is reserved: reach for it only
where incomplete validation carries explicit, material risk. The
aggregation phase (Phase 3) deduplicates findings and identifies
systemic issues before synthesis.

## Acceptance Criteria Thresholds

| Metric | Pass Threshold |
|--------|----------------|
| Error findings | 0 |
| Warning findings | ≤ 5 |
| Coverage percentage | ≥ 90% |

## Cross-Capability Guidance

For large scopes, consider decomposing into artifact categories and
running multiple specialized validation commands before a unified
synthesis.

The `--remediate` flag enables automatic correction through the
validation loop pattern. Without this flag, validation is read-only
and produces reports without modifications.

Validation often follows implementation: run `/implement-fix` to
correct findings, then re-validate to confirm the fix landed cleanly.

## Execution Pointers

- Story completion → the acting story-completion validator contract
  (deterministic oracles, in-session repair, commit); do not fan out
  this command family for routine closure
- Comprehensive validation → launch `validation-workflow` (workflow
  agent); the workflow skill is preloaded
- Per-dimension validation → launch the matching phase skill
  (`validate_structure_phase`, `validate_references_phase`,
  `validate_consistency_phase`, `validate_completeness_phase`)
- All variants share `validation_objective` and `validation_procedure`
  layers

See validation_objective (the `validation_objective` skill)
and `ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`.
