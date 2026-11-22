# Discovery Phase

Gather information needed to inform subsequent workflow phases. Discovery establishes context, identifies relevant artifacts, and produces findings that guide later actions.

## When to Use

Use discovery phase when:

- Subsequent phases require understanding of current state
- Multiple sources must be examined to establish context
- Artifact inventory is needed before action
- Patterns or conventions must be identified for consistency
- Dependencies or relationships affect later decisions

Discovery is foundational. Most workflows begin with discovery because uninformed action leads to poor outcomes.

## Variants

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_phase_selection.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_academic.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_baseline.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_external.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_findings_resolution.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_general.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_multi_source.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_precommit.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_task_resolution.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_workflow_reference.md" />

## Model Selection

**Primary**: Sonnet

Discovery focuses on information retrieval and organization rather than complex reasoning. Sonnet provides sufficient capability at lower cost.

**Exception**: Use Opus when discovery requires deep architectural reasoning or when discovered information is unusually complex.

## Coordination Pattern

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

Discovery typically uses parallel pattern when multiple independent aspects require investigation. Launch parallel discovery agents for non-overlapping domains, then aggregate findings.

For single-domain discovery, sequential execution suffices.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />

When discovery spans many domains requiring synthesis, use fan-out fan-in with a dedicated synthesis sub-agent.

## Context Requirements

**Parameters provided by main agent**:

| Parameter | Required | Purpose |
|-----------|----------|---------|
| Report (reserved) | Yes | Sub-agent creates the report record just-in-time with `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing |
| SCOPE | Yes | What to examine |
| Work item context | If applicable | EPIC_PATH, STORY_PATH, ISSUE_PATH |

## Transition Criteria

Discovery completes and transitions when:

- All scoped areas have been examined
- Findings are documented with sufficient detail for subsequent phases
- Gaps are explicitly noted
- No critical information remains unknown

If critical information cannot be discovered, escalate to user before proceeding.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Skipping discovery | Uninformed subsequent phases | Always gather context first |
| Unbounded discovery | Scope creep, context exhaustion | Define clear boundaries |
| Shallow discovery | Insufficient context for quality decisions | Investigate until findings support confident action |
| Discovery without structure | Disorganized findings hard to consume | Use consistent report format |
| Ignoring gaps | False confidence in completeness | Document what wasn't found |
