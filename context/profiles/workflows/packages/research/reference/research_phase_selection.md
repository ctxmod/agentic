# Research Phase

Gather information from external sources--web resources, academic papers, documentation, standards bodies--to inform decisions and validate approaches. Research supplements internal discovery with authoritative external context.

## When to Use

Use research phase when:

- Decisions benefit from external best practices
- Current approaches require validation against industry standards
- Domain-specific expertise is needed beyond project context
- Alternatives must be evaluated with external evidence
- Standards or specifications must be consulted

Research differs from discovery: discovery examines internal artifacts; research consults external sources.

## Variants

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research/reference/research_phase_selection.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research/reference/research_docs.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research/reference/research_general.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research/reference/research_multi_source.md" />
- `ctx:github.com/ctxmod/agentic/context::shared::scholarship@academic/reference/research_objective_academic.md`

## Model Selection

**Primary**: Sonnet

Research focuses on information retrieval and summarization. Sonnet provides sufficient capability for most research tasks.

**Exception**: Use Opus for complex academic synthesis or when evaluating nuanced technical trade-offs requiring deep reasoning.

## Coordination Pattern

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

Research typically uses parallel pattern when multiple independent sources require consultation. Launch parallel research agents for different source types or topics.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />

When research spans many sources requiring synthesis, use fan-out fan-in with a dedicated synthesis sub-agent.

## Context Requirements

**Parameters provided by main agent**:

| Parameter | Required | Purpose |
|-----------|----------|---------|
| Report (reserved) | Yes | Sub-agent creates the report record just-in-time with `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing |
| RESEARCH_QUESTIONS | Yes | Specific questions to answer |
| Work item context | If applicable | EPIC_PATH, STORY_PATH, ISSUE_PATH |

## Transition Criteria

Research completes and transitions when:

- Research questions have been addressed
- Findings are supported by credible sources
- Contradictions between sources are documented
- Synthesis provides actionable guidance

If critical questions remain unanswered, note limitations and proceed with documented uncertainty.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Research without questions | Unfocused, produces noise | Define specific research questions |
| Single-source reliance | Bias, incomplete perspective | Consult multiple sources |
| Unsourced claims | Unverifiable, unreliable | Always cite sources |
| Research as procrastination | Delays action unnecessarily | Bound research scope and duration |
| Ignoring contradictions | False confidence | Document conflicting findings |
