# Review Command Selection Guidance

Selection guidance for the `/review` command — the unified entry
point for comprehensive quality review combining multi-criteria
validation with synthesis and remediation planning. This document
captures variant-selection logic, validation-criteria comparison, and
"which command when" guidance. Execution lives in the review phase
skills and the `validation_objective`/`validation_procedure` layers
(review reuses validation machinery).

> **Concurrency gate.** Where this document describes agents running "in parallel" or "concurrently", that shape is available only to agents whose resolved `allow-parallel-agents` posture is `true` — the read-only discovery, analysis, research, review, and validation phase agents. The concurrency instruction in your own agent-depth instructions is authoritative: if it tells you to use one agent at a time, run the same steps sequentially in the listed order. The phase decomposition, report paths, and synthesis steps are unchanged either way.

## Variant Comparison

`/review` handles both artifact scopes and pull request changes. Use
specialized commands for narrower scope.

| Command | Scope | When to Use |
|---------|-------|-------------|
| `/review` | All criteria, any scope | Comprehensive quality gates, release readiness |
| `/review_audit` | All criteria, artifact scope | Quality baseline, technical debt assessment |
| `/review_pr` | Changed artifacts, PR focus | Code review, change validation |

## Work Item Context

Reports nest within work item if context provided. Comprehensive
reviews support significant quality assessments before releases,
merges, or major changes.

Without work item context, reports write to the store's top-level `reports/` (no `--owner`).

## Validation Criteria

Comprehensive review validates all criteria. Each criterion has a
characteristic severity range; the comprehensive command covers them
all, while specialized commands focus on subsets.

| Criterion | Focus | Severity Range |
|-----------|-------|----------------|
| Structural | Format, schema, required sections | Error to Warning |
| Standards | Conventions, style, naming | Warning to Suggestion |
| References | Links, imports, dependencies | Error to Warning |
| Consistency | Cross-artifact alignment, terminology | Warning to Suggestion |
| Completeness | Coverage, required elements | Error to Warning |

## Workflow Shape

The comprehensive review workflow executes six phases:

1. **Scope Analysis** (Sonnet, sequential): Determine review mode
   (PR identifier vs artifact path) and establish artifact scope
2. **Context Gathering** (Sonnet, parallel): Gather standards,
   dependencies, and requirements in parallel
3. **Multi-Criteria Validation** (Sonnet, parallel): Five validators
   run concurrently — structural, standards, references, consistency,
   completeness
4. **Findings Aggregation** (Sonnet, sequential): Group findings
   thematically across validators; identify cross-validator
   corroboration
5. **Quality Synthesis** (Opus, sequential): Comprehensive assessment
   with quality verdict
6. **Remediation Planning** (Sonnet, sequential): Prioritized
   remediation plan with effort estimates

## Resource Cost Notes

Comprehensive review runs **5 parallel validators** after context
gathering, providing broad coverage. This consumes more resources
than specialized commands. Use for quality gates where incomplete
assessment carries risk.

The aggregation phase (Phase 4) groups findings thematically before
synthesis, reducing the cognitive load on the synthesis agent and
improving coherence of the final assessment.

For extremely large scopes, consider decomposing into domain-specific
audits before a unified synthesis.

## Quality Verdicts

| Verdict | Criteria |
|---------|----------|
| Pass | No errors, warnings acceptable |
| Conditional Pass | Minor errors with clear remediation path |
| Fail | Blocking errors or systemic issues |

## Decision Guide

| Situation | Recommended Command |
|-----------|---------------------|
| Release readiness gate | `/review` |
| Comprehensive quality baseline | `/review_audit` |
| Code review on a pull request | `/review_pr` |
| Single-criterion check (e.g. only references) | Specialized `/validate:*` command |
| Extremely large scope | Decompose into domain-specific audits |

## Cross-Capability Guidance

Review is downstream of implementation: run `/implement-fix` after a
review to address findings, or use the `/review` output's remediation
plan directly. Review's validators are the same machinery as
`/validate` — the difference is scope (any artifact vs pre-publication
gate) and the synthesis + remediation planning phases that review
adds.

## Execution Pointers

- Comprehensive review → orchestrate via main agent using the
  six-phase workflow described above; the review workflow is
  preloaded into the workflow agent
- PR review → `/review_pr` extracts changed files as the artifact set
- Audit → `/review_audit` runs the same validators against an
  explicit artifact scope

The review workflow composes the
fan-out fan-in pattern (<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />) for context
gathering and multi-criteria validation, then sequential synthesis.
See `ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`.
