# Analysis Phase

Interpret discovery and research findings to produce actionable recommendations. Analysis transforms raw information into structured understanding that guides planning and execution.

## When to Use

Use analysis phase when:

- Discovery findings require interpretation
- Multiple sources need synthesis
- Gaps must be identified before planning
- Recommendations should guide subsequent phases
- Complex findings need structured breakdown

Analysis bridges discovery and planning. It adds meaning to findings.

## Variants

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_phase_selection.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_academic_synthesis.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_best_practices_synthesis.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_comparative.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_general.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_prioritization.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_recommendation.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::analysis/reference/analysis_synthesis.md" />

## Model Selection

**Primary**: Sonnet

Analysis focuses on interpretation and organization. Sonnet handles most analysis tasks effectively.

**Exception**: Use Opus for analysis involving complex trade-offs, subtle patterns, or novel problem domains requiring deep reasoning.

## Coordination Pattern

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/sequential.md" />

Analysis typically follows sequential pattern, proceeding after discovery completes. Analysis requires discovery output as input.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />

For multi-dimensional analysis or when parallel discovery agents produced outputs, use fan-out fan-in to analyze each dimension and synthesize findings.

## Context Requirements

**Parameters provided by main agent**:

| Parameter | Required | Purpose |
|-----------|----------|---------|
| Report (reserved) | Yes | Sub-agent creates the report record just-in-time with `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing |
| INPUT_REPORT | Yes | Discovery report to analyze |
| TARGET_SPEC | For gap analysis | Target state specification |
| Work item context | If applicable | EPIC_PATH, STORY_PATH, ISSUE_PATH |

## Transition Criteria

Analysis completes and transitions when:

- Findings are interpreted with meaning
- Patterns and gaps are identified
- Recommendations are actionable
- Confidence levels are assessed
- Open questions are documented

If analysis reveals blockers, escalate before proceeding.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Analysis without findings | Nothing to interpret | Complete discovery first |
| Restating findings | No added value | Interpret and recommend |
| Unsupported conclusions | Unreliable guidance | Ground interpretations in findings |
| Ignoring contradictions | False coherence | Document conflicting signals |
| Excessive hedging | Unusable recommendations | Provide actionable guidance |
