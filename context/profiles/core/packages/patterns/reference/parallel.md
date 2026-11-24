# Parallel Pattern

Launch multiple sub-agents concurrently on independent concerns. The dispatching agent waits for all to complete before proceeding.

**Applicability gate.** This pattern is available only to agents whose resolved `allow-parallel-agents` posture is `true`. Sequential dispatch is the default at every depth. The concurrency instruction in your own agent-depth instructions is authoritative and overrides this file: if it tells you to use one agent at a time, run the steps below one at a time in the listed order and treat every "in a single message" instruction here as inapplicable. Encountering this pattern in a skill's reference list is not permission to fan out.

```
        Launch
         ↓
    ┌────┴────┐
    ↓         ↓         ↓
Agent 1    Agent 2    Agent 3
    ↓         ↓         ↓
Output 1   Output 2   Output 3
    └────┬────┘
         ↓
      Aggregate
```

## When to Use

Use when you hold the `allow-parallel-agents` permission and multiple agents work on independent aspects with clearly bounded scopes, no agent requires another's output, work partitions by domain, artifact type, or concern, and time efficiency justifies the added orchestration complexity. Every scenario below assumes read-only agents; agents that modify artifacts stay sequential even under the permission.

| Scenario | Parallel Agents |
|----------|----------------|
| Comprehensive discovery | One agent per module/domain |
| Multi-criteria validation | One agent per validation category |
| Comparative analysis | One agent per alternative evaluated |
| Documentation generation | One agent per independent document type |

**Exception**: Work item creation/execution is always sequential (issues within stories, tasks within issues) because each work item produces context that informs subsequent items. See sequential work item pattern.

## Partitioning Strategy

### Independence Validation

Before launching, enumerate planned agents and check pairwise for: output dependencies, write conflicts on shared artifacts, and resource contention if agents use the same tools. Any dependency requires the sequential or fan-out-fan-in pattern instead.

### Partitioning Dimensions

| Dimension | Example |
|-----------|---------|
| By module | `auth/`, `billing/`, `user/` analyzed simultaneously |
| By artifact type | Source, documentation, configuration analyzed separately |
| By criteria | Structural, standards, reference validation concurrently |
| By concern | Frontend, backend, infrastructure changes in parallel |
| By dataset | Different data sources researched simultaneously |

### Isolation

Parallel agents require: separate output paths (each agent reserves its own report path just-in-time), non-overlapping scopes over disjoint artifact sets, no shared state, and — when agents write artifacts — an isolated checkout per agent (a linked worktree, or a clean-HEAD clone or archive+init snapshot where the repository bans worktrees).

## Execution

### Main Agent Behavior

1. **Plan**: Define agents and their independent scopes
2. **Prepare**: Create distinct parameter sets per agent
3. **Launch**: Send all agent launches in a single message with parallel tool calls
4. **Collect**: Gather output reports after all agents complete
5. **Aggregate**: Synthesize findings across reports
6. **Proceed**: Use aggregated results for the next phase

### Parameter Passing

Each parallel agent reserves its own report path just-in-time (`ctxmod work new report "<title>"`) and receives SCOPE, plus shared work item context parameters. Unlike sequential patterns, parallel agents have no INPUT_REPORT between them.

```
Agent N Parameters:
  Report: sub-agent reserves just-in-time via `ctxmod work new report "agent-n" --owner <work item, if any>` immediately before writing; returns the path
  SCOPE={scope-n}
  [Work item context parameters]
```

REPORTS_BASE derives from work item context per `ctx:github.com/ctxmod/agentic/context::shared::framework@workflows/reference/params.md`.

### Output Structure

Organize outputs within work item context. Agent names in the titles distinguish parallel outputs; shared creation times indicate parallel launch.

```
items/<YYYY-MM>/<DD-HHMM>-<issue id>-<slug>/   # the issue's item directory (ctxmod work store)
└── reports/
    └── <YYYY-MM>/
        ├── <DD-HHMM>-<id>-agent-1-discovery.md
        ├── <DD-HHMM>-<id>-agent-2-analysis.md
        ├── <DD-HHMM>-<id>-agent-3-validation.md
        └── <DD-HHMM>-<id>-synthesis.md
```

## Aggregation

After completion, the main agent synthesizes across outputs:

1. Read all reports; extract key findings, locations, and recommendations
2. Identify cross-cutting patterns: common issues across scopes, unique per-scope findings, and recurring themes
3. Resolve contradictions between agents
4. Note coverage gaps and interdependencies spanning scope boundaries
5. Prioritize findings by impact and write a synthesis report

## Error Handling

**Agent failure**: Let remaining agents complete, collect successful outputs, assess whether the failed agent's work is critical, report partial results with explanation, and propose options (retry, proceed without, or abort). Await user decision.

**Timeout**: Check if slow agents are progressing or stuck, let timely agents complete first, then decide whether to wait, proceed without, or retry. Document the rationale.

## Trade-offs

| Advantage | Detail |
|-----------|--------|
| Time efficiency | Independent work completes simultaneously |
| Bounded scope | Each agent focuses on one concern |
| Resilience | One failure doesn't block others |
| Scalability | Add agents as complexity grows |
| Natural partitioning | Many problems decompose into independent concerns |

| Disadvantage | Mitigation |
|--------------|------------|
| Complex aggregation | Allocate explicit aggregation phase |
| Dependency risks | Validate independence before launch |
| Resource contention | Design scopes to minimize tool overlap |
| Debugging complexity | Isolate failures post-execution |

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Applying this pattern without the `allow-parallel-agents` permission | Conflicting concurrent work, lost context inheritance | Run the same steps sequentially in the listed order |
| Launching agents sequentially **when permitted and independent** | Wastes parallelism | Single message with parallel tool calls |
| Overlapping scopes | Conflicts, redundant work | Clear, non-overlapping boundaries |
| Mixing dependent work | Race conditions | Sequential pattern for dependent phases |
| Skipping aggregation | Fragmented insights | Always synthesize parallel outputs |
| Too many agents (8+) | Diminishing returns | Broader scopes with fewer agents |
| **Parallel work item execution** | Work items (issues, tasks) must execute sequentially. Each work item reads all previous work item reports. Parallelization causes misalignment and duplication. | Sequential work item execution with report passing |

## Pattern Combinations

Sequential phases may contain parallel sub-phases, parallel execution is a special case of fan-out-fan-in, and each parallel agent may trigger its own validation loops.

```
Phase 1 (Sequential)
  ↓
Phase 2 (Parallel): Agent A | Agent B | Agent C
  ↓
Phase 3 (Aggregation)
  ↓
Phase 4 (Sequential)
```
