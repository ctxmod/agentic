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
