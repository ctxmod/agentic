# Fan-Out Fan-In Pattern

Distribute work across multiple parallel sub-agents, then aggregate their results into a unified synthesis. Two distinct phases: fan-out launches independent parallel agents, fan-in aggregates all outputs into coherent synthesis. This combines parallel exploration breadth with centralized coherence.

**Applicability gate.** The fan-out half of this pattern is available only to agents whose resolved `allow-parallel-agents` posture is `true`. Sequential dispatch is the default at every depth. The concurrency instruction in your own agent-depth instructions is authoritative and overrides this file: if it tells you to use one agent at a time, run the fan-out steps one at a time in the listed order and treat every "in a single message" instruction here as inapplicable. The fan-in half — collect, extract, resolve conflicts, synthesize — applies unchanged either way, and is the reason this pattern stays useful under sequential dispatch.

```
        Fan-Out
          ↓
    ┌─────┴─────┐
    ↓           ↓           ↓
Agent 1      Agent 2     Agent 3
    ↓           ↓           ↓
Output 1     Output 2    Output 3
    └─────┬─────┘
          ↓
       Fan-In
          ↓
     Synthesis
```

## When to Use

Use when the problem decomposes into independent sub-problems where individual exploration of each is valuable, results must integrate into a coherent whole, and synthesis benefits from comprehensive cross-scope pattern identification.

| Scenario | Fan-Out | Fan-In |
|----------|---------|--------|
| System-wide discovery | Discover each module independently | Synthesize architecture patterns across modules |
| Multi-source research | Research each source separately | Aggregate findings into unified recommendations |
| Comprehensive audit | Audit each domain separately | Synthesize organization-wide themes and risks |
| Comparative analysis | Analyze each alternative separately | Compare and rank alternatives across all dimensions |

**Work item handling**: This pattern coordinates parallel discovery/analysis phases. Work items produced from synthesis execute sequentially—see sequential work item pattern for details.

## Execution

### Main Agent Behavior

**Fan-out phase**:

1. **Plan**: Define scopes for each parallel agent
2. **Prepare**: Create distinct parameter sets per agent
3. **Launch**: Send all agent launches in single message with parallel tool calls — only if you hold the `allow-parallel-agents` permission. Otherwise launch them one at a time, in order, waiting for each.
4. **Wait**: Wait for all agents until all of them are complete

**Fan-in phase**:

5. **Collect**: Read output reports from all agents
6. **Extract**: Pull key findings, locations, recommendations from each report
7. **Identify patterns**: Find themes appearing across multiple agent outputs
8. **Resolve conflicts**: Address contradictions between agents
9. **Assess coverage**: Identify gaps no agent addressed
10. **Synthesize**: Create unified view, prioritize by impact and frequency, produce aggregation report

### Scoping Strategies

Fan-out agents work on independent, non-overlapping scopes. Each agent may use different objective types.

| Strategy | Example |
|----------|---------|
| By module | Auth module, billing module, user module analyzed in parallel |
| By domain | Frontend artifacts, backend artifacts, infrastructure audited concurrently |
| By data source | Academic papers, industry reports, documentation researched separately |
| By concern | Security issues, performance issues, maintainability issues evaluated independently |

### Parameter Passing

**Fan-out parameters** (parallel agents):

```
Agent N Parameters:
  Report: sub-agent reserves just-in-time via `ctxmod work new report "fanout-agent-n" --owner <work item, if any>` immediately before writing; returns the path
  SCOPE={scope-n}
  [Work item context parameters]
```

**Fan-in parameters** (synthesis agent):

```
Synthesis Parameters:
  Report: sub-agent reserves just-in-time via `ctxmod work new report "synthesis" --owner <work item, if any>` immediately before writing; returns the path
  INPUT_REPORTS={fan-out agents' returned report paths}
  SYNTHESIS_TYPE={unified|comparative|aggregative}
  [Work item context parameters]
```

Synthesis agent receives paths to all fan-out outputs. REPORTS_BASE derives from work item context per `ctx:github.com/ctxmod/agentic/context::shared::framework@workflows/reference/params.md`.

### Output Structure

Organize outputs within work item context. Titles distinguish fan-out outputs from synthesis.

```
items/<YYYY-MM>/<DD-HHMM>-<issue id>-<slug>/   # the issue's item directory (ctxmod work store)
└── reports/
    └── <YYYY-MM>/
        ├── <DD-HHMM>-<id>-fanout-agent-1.md
        ├── <DD-HHMM>-<id>-fanout-agent-2.md
        ├── <DD-HHMM>-<id>-fanout-agent-3.md
        └── <DD>-1445-<id>-synthesis.md
```

## Synthesis Techniques

### Pattern Aggregation

Assign findings to thematic categories, track frequency across agents (strong: 4+, moderate: 2-3, weak: 1), build concept-to-agent mapping showing pattern distribution, and prioritize by frequency and impact.

### Conflict Resolution

| Conflict Type | Resolution Strategy |
|---------------|---------------------|
| Factual contradiction | Verify against source material, document both perspectives |
| Prioritization disagreement | Explain criteria differences, present alternative rankings |
| Scope overlap claims | Reclarify boundaries, acknowledge shared responsibility |
| Assessment divergence | Present multiple viewpoints with supporting rationale |

### Gap Analysis

List expected coverage, check each agent's output against it, identify uncovered topics, assess gap significance, and document what synthesis couldn't address.

### Cross-Cutting Insights

- **Systemic patterns**: Issues across multiple scopes suggest systemic causes
- **Unique anomalies**: Single-scope findings indicate localized problems or special cases
- **Boundary effects**: Issues at scope interfaces reveal integration challenges
- **Common solutions**: Remediation patterns applicable across multiple scopes

## Error Handling

**Fan-out failures**: Collect successful outputs, assess whether failed scopes are critical, document gaps, and decide whether to proceed with partial synthesis, retry, or abort. Flag uncertainty from missing data.

**Overwhelming outputs**: Extract top-level findings, group related findings before synthesis, prioritize high-impact patterns, and document that synthesis is selective rather than exhaustive.

**Contradictory outputs**: Document contradictions explicitly, present multiple perspectives with evidence, explain why contradiction exists, and avoid forced reconciliation when disagreement is genuine.

**No clear patterns**: Report diversity without false synthesis, organize by agent or scope rather than artificial themes, and acknowledge when the problem resists unified interpretation.

## Trade-offs

| Advantage | Detail |
|-----------|--------|
| Comprehensive coverage | Parallel agents explore entire problem space |
| Cross-scope insight | Synthesis reveals patterns invisible to individual agents |
| Efficient execution | Independent work completes simultaneously |
| Coherent output | Fan-in integrates distributed findings into unified result |
| Scalability | Add agents as problem complexity grows |

| Disadvantage | Mitigation |
|--------------|------------|
| Synthesis complexity | Allocate dedicated aggregation phase |
| Contradiction risk | Establish resolution strategies upfront |
| Partial failure impact | Document gaps from failed agents explicitly |
| Overwhelming detail | Prioritize high-impact findings |
| Orchestration overhead | Define clear scopes upfront |

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Skipping fan-in synthesis | Fragmented insights, no unified understanding | Always allocate aggregation phase |
| Overlapping fan-out scopes | Redundant work, wasted parallelism | Clear, non-overlapping scope boundaries |
| Mixing dependent work | Sequential execution disguised as parallel | Ensure fan-out agents are truly independent |
| Fanning out without the `allow-parallel-agents` permission | Conflicting concurrent work, lost context inheritance | Run the fan-out steps sequentially; fan-in is unchanged |
| Launching agents sequentially **when permitted and independent** | Wastes parallelism | Single message with parallel tool calls |
| Linear synthesis without pattern analysis | Misses cross-cutting insights | Apply pattern aggregation during fan-in |

## Synthesis Variants

| Variant | Use Case | Output | Pattern Focus |
|---------|----------|--------|---------------|
| Unified | Comprehensive understanding of complex system | Single report organized by theme | Strongest patterns across all agents |
| Comparative | Evaluating alternatives, comparing domains | Comparison matrix, pros/cons, recommendation | Differences and similarities between agents |
| Aggregative | Data collection, metric aggregation, inventory building | Consolidated inventories, unified metrics | Minimal; completeness over insight |

## Pattern Combinations

Fan-out fan-in extends basic parallel by adding explicit aggregation. Most complex workflows embed fan-out fan-in within sequential phases:

```
Phase 1: Discovery (Fan-Out Fan-In)
  Fan-Out: Agent 1 (Module A) | Agent 2 (Module B) | Agent 3 (Module C)
  Fan-In: Synthesize architecture patterns

Phase 2: Analysis (Sequential)
  Analyze synthesized findings

Phase 3: Implementation (Fan-Out Fan-In)
  Fan-Out: Agent 1 (Frontend changes) | Agent 2 (Backend changes) | Agent 3 (Infra changes)
  Fan-In: Verify integration and consistency
```

After synthesis, workflow proceeds to subsequent sequential phases. Synthesis output may also trigger a validation loop before proceeding.
