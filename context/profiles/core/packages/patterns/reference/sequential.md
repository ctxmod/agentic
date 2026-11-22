# Sequential Pattern

Execute phases in linear order where each stage consumes output from the previous stage. Work flows through a pipeline with explicit handoffs between stages.

```
Phase 1 → Phase 2 → Phase 3 → Phase 4
  ↓         ↓         ↓         ↓
Output 1 → Output 2 → Output 3 → Output 4
```

## When to Use

Use when each phase requires completion of the previous phase's output, work benefits from progressive refinement through distinct non-overlapping stages, dependencies are one-directional, and later stage quality depends on earlier stage thoroughness.

| Scenario | Phases |
|----------|--------|
| Understanding a system | Discovery → Analysis → Documentation |
| Fixing a problem | Discovery → Validation → Analysis → Implementation |
| Creating artifacts | Research → Planning → Creation → Validation |

## Phases

Sequential patterns have 3-6 phases. Fewer phases indicate simplicity; more phases suggest complexity that might benefit from decomposition. Each phase requires an objective (what it accomplishes), input (from previous phase), output (for next phase), and agent (which sub-agent executes it).

| Type | Purpose | Output |
|------|---------|--------|
| Discovery | Gather information | Reports with findings |
| Validation | Assess compliance | Reports with violations |
| Analysis | Interpret findings | Reports with recommendations |
| Planning | Create approach | Work items, specifications |
| Implementation | Apply changes | Modified artifacts |
| Verification | Confirm outcomes | Reports with verification results |

## Execution

### Main Agent Behavior

1. **Plan**: Define all phases upfront with their sequence
2. **Launch**: Execute first phase with appropriate parameters
3. **Wait**: Do not proceed until phase completes
4. **Extract**: Read phase output report
5. **Verify**: Confirm phase produced expected output
6. **Launch Next**: Pass previous output as input to next phase
7. **Repeat**: Continue until final phase completes

### Parameter Passing

Each phase reserves its report path just-in-time (`ctxmod work new report "<title>"`, with `--owner` set to the most specific work item when one exists) immediately before writing, receives the previous phase's returned report path as INPUT_REPORT, and shared work item context parameters (ISSUE_PATH, STORY_PATH, etc.).

```
Phase N Parameters:
  Report: sub-agent reserves just-in-time via `ctxmod work new report "{NN}-{phase-name}" --owner <work item, if any>` immediately before writing; returns the path
  INPUT_REPORT={Phase N-1 returned report path}
  [Work item context parameters]
```

REPORTS_BASE derives from work item context per `ctx:github.com/ctxmod/agentic/context::shared::framework@workflows/reference/params.md`.

### Output Structure

Organize sequential outputs within work item context. Numbered slugs preserve execution order; the month directory and the `<DD-HHMM>` entry stamp group outputs by session.

```
items/<YYYY-MM>/<DD-HHMM>-<issue id>-<slug>/   # the issue's item directory (ctxmod work store)
└── reports/
    └── <YYYY-MM>/
        ├── <DD-HHMM>-<id>-01-discovery.md
        ├── <DD>-1432-<id>-02-analysis.md
        ├── <DD>-1445-<id>-03-implementation.md
        └── <DD>-1500-<id>-04-verification.md
```

## Error Handling

**Phase failure**: Stop execution, read failure output, report to user with explanation, propose remediation, and await decision to retry, modify approach, or abort.

**Partial output**: Assess whether remaining output is adequate, document gaps, decide whether to proceed or retry based on gap criticality, and inform user of the situation and decision rationale.

## Trade-offs

| Advantage | Detail |
|-----------|--------|
| Clear progression | Each stage builds visibly on previous work |
| Simple reasoning | Linear flow is easy to understand and debug |
| Quality gates | Each phase validates input before proceeding |
| Isolated concerns | Each phase handles one aspect of the problem |
| Composability | Phases can be reused in other workflows |

| Disadvantage | Mitigation |
|--------------|------------|
| Sequential execution time | Accept as cost of pipeline quality |
| Blocking dependencies | Design efficient phases |
| Single point of failure | Implement robust error handling |

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Skipping phase outputs | Lose traceability | Always generate phase reports |
| Parallelizing dependent phases | Race conditions, incorrect results | Use parallel pattern for independent work |
| Mixing objectives within phases | Loss of focus, unclear outputs | One primary objective per phase |
| Excessive phases (6+) | Overhead, context loss | Combine related phases or decompose workflow |
| Omitting intermediate verification | Errors propagate undetected | Insert validation phases after critical operations |

## Pattern Combinations

Sequential phases may launch parallel sub-workflows internally, embed fan-out-fan-in for comprehensive analysis, or trigger validation loops at any phase. These combinations enable complex workflows while maintaining clear structure.
