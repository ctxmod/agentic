# Orchestration Patterns

Patterns define how main agents coordinate multiple sub-agents. They specify execution flow, agent relationships, and output aggregation.

> **Concurrency gate.** Where this document describes agents running "in parallel" or "concurrently", that shape is available only to agents whose resolved `allow-parallel-agents` posture is `true` — the read-only discovery, analysis, research, review, and validation phase agents. The concurrency instruction in your own agent-depth instructions is authoritative: if it tells you to use one agent at a time, run the same steps sequentially in the listed order. The phase decomposition, report paths, and synthesis steps are unchanged either way.

## Pattern Overview

| Pattern | Purpose | Key Characteristic |
|---------|---------|-------------------|
| Sequential | Linear handoff between phases | Each phase depends on previous output |
| Parallel | Concurrent independent execution | Agents work simultaneously on separate concerns |
| Validation Loop | Iterate until quality criteria met | Repeated validate-remediate cycles |
| Fan-Out Fan-In | Parallel work with aggregation | Distributed exploration, centralized synthesis |

## Before Choosing a Multi-Agent Pattern

Every pattern here costs more than one agent doing the work. Choose the simplest structure that meets the requirement, and add agents only when the gain is measured.

- **A workflow or an agent.** A *workflow* runs model calls along code-defined paths; an *agent* directs its own steps and tool use. Many tasks need neither — a single well-contexted call, or one agent with good tools, is the baseline every pattern must beat.
- **Token cost.** One published measurement puts a single agent at about 4× the tokens of a chat exchange and a multi-agent system at about 15×; token volume alone explained most of the performance variance on that research benchmark. Multi-agent pays off on high-value, breadth-first work whose parts are genuinely independent.
- **Shared context is the limit.** Multi-agent structures fit poorly when every agent needs the same context or when sub-tasks depend on each other — most coding work is of this kind. Each agent's actions carry implicit decisions; parallel agents that cannot see each other's decisions make conflicting ones, and the merge inherits the conflict. That is why this module keeps file-modifying work sequential and reserves concurrency for independent, read-only work.
- **Harness parts encode assumptions.** Each orchestration component exists because the model was assumed unable to do something alone. Those assumptions go stale as models improve; when the model changes, remove components one at a time and keep only those whose removal measurably hurts.

## Vocabulary Map

Vendor guidance names a common set of composable patterns; this module's patterns map onto them as follows.

| Common name | Shape | Here |
|---|---|---|
| Prompt chaining | Fixed sequence of steps, optional programmatic gates between them | Sequential |
| Routing | Classify the input, send it to a specialized path | Command and skill selection (the `command_selection` package of the agentic `workflows` profile) |
| Parallelization — sectioning | Independent sub-tasks run at once | Parallel |
| Parallelization — voting | The same task run several times for confidence | A multi-dimension validation panel — an explicit high-risk choice, never the default |
| Orchestrator-workers | A lead decomposes the task at run time, delegates, and synthesizes | Fan-Out Fan-In with run-time scoping |
| Evaluator-optimizer | One model generates, another evaluates against clear criteria, iterate | Validation Loop (deterministic oracles first) |
| Handoff vs. agent-as-tool | Control passes to the specialist, or the manager keeps ownership of the result | This module's delegation is agent-as-tool: the sub-agent returns a summary and a report path and the parent keeps ownership |

## Selection Guide

### Decision Tree

```
Are sub-tasks independent of each other's output?
├── No → Sequential
└── Yes → Do results need aggregation into synthesis?
    ├── Yes → Fan-Out Fan-In
    └── No → Parallel
```

Add a Validation Loop when quality must meet defined criteria before proceeding.

### Use Case Matrix

| Pattern | Scenarios |
|---------|-----------|
| Sequential | Understanding systems, creating artifacts (linear research-to-creation flow) |
| Sequential + Validation Loop | Fixing problems (discovery through implementation, iterate until resolved) |
| Parallel | Multi-criteria validation, multi-domain changes (independent concerns) |
| Fan-Out Fan-In | Comprehensive audits, system-wide discovery, comparative analysis (parallel exploration with synthesis) |
| Validation Loop | Quality improvement, refactoring (iterate until compliant) |

Note: Work item execution (issues within stories, tasks within issues) is always sequential regardless of pattern choice. Each work item receives all previous work item reports as compulsory reads.

## Pattern Characteristics

Detailed pattern implementations:

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/sequential.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/validation_loop.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />

| Pattern | Strengths | Weaknesses | Use When |
|---------|-----------|------------|----------|
| Sequential | Clear progression, simple debugging, quality gates, isolated concerns | No parallelism, single failure point, blocking phases | Progressive refinement through dependent stages |
| Parallel | Time efficiency, natural partitioning, resilience, scalability | Complex aggregation, dependency risks, resource contention, debugging difficulty | Independent sub-tasks, time efficiency matters |
| Validation Loop | Quality convergence, explicit gates, traceability, corrective feedback | Multi-iteration time, over-correction risk, stuck loops, false positives | Quality criteria, systematically correctable issues |
| Fan-Out Fan-In | Comprehensive coverage, cross-scope insight, coherent output, scalable | Synthesis complexity, contradiction risk, partial failure impact, detail volume | Independent sub-problems requiring coherent integration |

## Pattern Combinations

- **Sequential + Embedded Validation Loop**: Validation loops within specific sequential phases for quality-critical steps
- **Sequential with Fan-Out Fan-In**: Fan-out fan-in embedded within one phase of a sequential workflow
- **Parallel Validation Loops with Synthesis**: Multiple validation loops executing in parallel, outputs aggregated
- **Nested Fan-Out Fan-In**: Hierarchical fan-out at multiple levels with synthesis at each level

## Design Principles

**Layering**: Start with Sequential patterns. Add parallelism for independent opportunities, validation loops for critical quality gates, or fan-out fan-in for comprehensive coverage requiring synthesis. Complexity increases only when simpler patterns prove insufficient.

**Complexity Management**: Use sequential as top-level structure. Embed parallel, fan-out fan-in, or validation loops within specific phases. Keep instances simple: 3-6 agents per pattern, 3-4 iterations per validation loop.

## Anti-Patterns

| Anti-Pattern | Problem | Solution |
|--------------|---------|----------|
| Premature parallelization | Complexity without benefit. Parallel work item execution is always incorrect—work items require sequential processing with report passing. | Start sequential, parallelize proven bottlenecks |
| Missing synthesis | Fragmented insights | Always aggregate parallel outputs |
| Ignoring quality | Error propagation | Insert validation loops before critical phases |
| Over-orchestration | More complex than problem | Simplest pattern that meets requirements |
| Mixed patterns in single phase | Unclear execution flow | One primary pattern per phase |

Long-running work that spans several sessions adds its own structure — initializer and worker sessions, progress artifacts, clean state per session: <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/long_running_sessions.md" />.

## Sources

- https://www.anthropic.com/engineering/building-effective-agents — workflows versus agents; prompt chaining, routing, parallelization (sectioning, voting), orchestrator-workers, evaluator-optimizer; start simple (Anthropic Engineering, 2024-12-19; accessed 2026-11-22)
- https://www.anthropic.com/engineering/multi-agent-research-system — ~4× and ~15× token multipliers, token usage explaining ~80% of benchmark variance, unsuitability for shared-context and most coding work (Anthropic Engineering, 2025-06-13; accessed 2026-11-22)
- https://cognition.com/blog/dont-build-multi-agents — "share context"; "actions carry implicit decisions, and conflicting decisions carry bad results" (Cognition, 2025-06-12; accessed 2026-11-22)
- https://developers.openai.com/api/docs/guides/agents/orchestration — handoffs versus agents-as-tools; start with one agent and add specialists only when they clearly help (OpenAI docs; accessed 2026-11-22)
- https://www.anthropic.com/engineering/harness-design-long-running-apps — harness components encode assumptions that go stale; remove them one at a time to find the load-bearing ones (Anthropic Engineering, 2026-03-24; accessed 2026-11-22)
