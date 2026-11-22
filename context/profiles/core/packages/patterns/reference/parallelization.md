# Parallelization

Sequential dispatch is the default at every agent depth. Parallelization is an opt-in capability: it applies only to agents whose resolved `allow-parallel-agents` posture is `true`, and only to independent, read-only work. This file describes how to parallelize correctly **once you have been granted the permission** — it does not grant it.

`allow-parallel-agents` names the permission an agent declaration carries: whether the agent may launch sub-agents concurrently. In this module, the agents that hold it say so in their declaration ("May launch parallel sub-agents"); agents that modify files never do.

**Check before applying anything here.** The concurrency instruction in your own agent-depth instructions is authoritative. If it says to use one agent at a time, that governs, and every criterion, diagram, and efficiency argument below is inapplicable to you: run the work sequentially in the listed order. Reading this file is not permission to fan out.

When the permission is granted, execute independent tasks simultaneously to maximize work per turn. Each turn carries coordination overhead — sequential execution of genuinely independent, read-only work wastes turns, increases latency, and reduces reasoning budget.

## Independence Criteria

**Parallel when** (and only when the permission is granted): outputs don't depend on each other, no overlapping file modifications, no cross-results needed within the parallel set, and no alignment dependencies (parallel tasks don't produce context that subsequent tasks need to incorporate).

**Sequential when**: the permission is absent, one task needs another's output, tasks modify the same files or packages, tasks produce alignment context for subsequent tasks, or work items (issues, tasks) within a parent hierarchy—always execute sequentially with report passing.

Before execution, confirm you hold the permission. If you do, apply these criteria: list all task objectives, check pairwise dependencies, group independent objectives for parallel launch, and order dependent groups sequentially. If you do not, order everything sequentially.

## Parallel Patterns

Discovery and validation phases are the ones that naturally parallelize, which is why the read-only discovery, analysis, research, review, and validation phase agents are the ones granted `allow-parallel-agents: true`. For those agents, scan source files, documentation, and test coverage simultaneously and validate independent reports concurrently. Analysis depends on discovery results, so it follows sequentially regardless of the permission:

```
Phase 1: Discovery (parallel)
├── scan source files
├── scan documentation
└── scan test coverage

Phase 2: Analysis (sequential, depends on Phase 1)
└── synthesize findings
```

## Turn Efficiency

Sequential execution of N independent tasks takes N+1 turns; parallel takes 2 regardless of count:

```
Sequential (4 turns):          Parallel (2 turns):
Turn 1: task A → result        Turn 1: tasks A,B,C → all results
Turn 2: task B → result        Turn 2: process results
Turn 3: task C → result
Turn 4: process results
```

**Efficiency analysis**: This analysis assumes no alignment costs. Parallel work item execution creates misalignment that requires additional turns to resolve, negating efficiency gains. Work items execute sequentially because alignment costs exceed parallelization benefits.

## Anti-Patterns

| Anti-Pattern | Consequence |
|--------------|-------------|
| Sequential when parallel is possible **and permitted** | Wasted turns, increased latency |
| Parallel without the `allow-parallel-agents` permission | Conflicting concurrent work, lost context inheritance |
| Parallel when dependencies exist | Race conditions, inconsistent results |
| Parallel code changes without isolated clean-HEAD checkouts | File conflicts, corrupted state |

## Orchestration Planning

Plan the complete workflow before execution: identify all phases, their dependencies, and — if you hold the permission — their parallelization potential. One planning turn maximizes efficiency and minimizes reactive coordination. Without the permission, the plan is simply an ordered sequence.
