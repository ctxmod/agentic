# Orchestration Planning

Plan the complete workflow before executing any tasks. Upfront planning maximizes parallelization and minimizes reactive decision-making.

## Core Principle

Orchestrators plan workflow systems, not ad-hoc sequences. A system defines phases, dependencies, and responses to outcomes before execution begins. This approach eliminates planning steps between phases.

A planned system provides structure. The orchestrator executes the plan, adapting only for unforeseen circumstances.

## Planning Components

### Phase Identification

Identify all phases needed to achieve the objective:

1. Discovery phase
2. Analysis phase
3. Validation phase
4. Execution phase
5. Verification phase
6. Remediation phases (if needed, repeating earlier phases until success)

Each phase has a clear purpose and defined completion criteria.

### Dependency Mapping

Map dependencies between phases and tasks:

- Analysis depends on Discovery output
- Validation depends on Analysis output
- Execution depends on Validation passing
- Verification depends on Execution completion

Dependencies determine sequencing. Independent phases can run in parallel.

### Parallelization Opportunities

Identify which tasks within each phase can run simultaneously:

```markdown
Discovery phase (parallel):
- scan source files
- analyze documentation
- inventory test coverage

Analysis phase (parallel, after Discovery):
- identify patterns
- detect gaps

Build phase (parallel):
- compile backend services
- bundle frontend assets
- generate API documentation
```

### Response Patterns

Define responses to predictable outcomes before execution:

```markdown
If validation fails:
  1. Execute correction tasks
  2. Re-run validation
  3. Proceed only when validation passes

If discovery finds no relevant files:
  1. Expand search scope
  2. Re-run discovery
  3. Proceed with available results

If build fails:
  1. Capture error logs
  2. Execute diagnostic tasks
  3. Apply fixes and retry
```

Pre-defined responses eliminate decision-making steps during execution.

## System vs Sequence

A **sequence** reacts after each step:

```
Run task A -> Wait -> Decide next step -> Run task B -> Wait -> ...
```

A **system** executes a defined plan:

```
Plan: A then B then C, with parallel D and E, handling failures by F
Execute: Follow the plan, adapting only for unforeseen circumstances
```

Systems minimize decision points during execution. Each decision point introduces latency and coordination overhead.

## Planning Template

```markdown
## Workflow Plan

### Objective
[What the workflow achieves]

### Phases

#### Phase 1: [Name]
- Purpose: [What this phase accomplishes]
- Tasks: [List of tasks, parallel or sequential]
- Dependencies: [What must complete first]
- Output: [Expected deliverables]

#### Phase 2: [Name]
...

### Error Handling
- [Condition]: [Response]

### Success Criteria
[How to determine the workflow succeeded]
```

## Execution Efficiency

Planning reduces coordination overhead significantly:

| Approach | Coordination steps for 5-phase workflow |
|----------|----------------------------------------|
| Reactive | 10+ (plan, execute, plan, execute, ...) |
| Planned | 5-6 (plan once, execute all) |

Each planning step in a reactive approach introduces latency. A planned system eliminates these intermediate decisions.

## Adaptation During Execution

Plans accommodate unexpected circumstances through three response levels:

1. **Minor deviations**: Continue with plan, note deviation
2. **Phase failure**: Follow defined error handling
3. **Fundamental change**: Pause, revise plan, continue

The plan provides structure. Adaptation handles exceptions without abandoning the overall system.

## Deliverable Volume Calibration

When a brief bounds time or names a reviewer gate, set the minimum defensible volume — test count, doc length, module count — before producing the first artifact, and grow it only on request. Ask the volume question explicitly at the first review gate instead of discovering the answer through trim rounds.

Deliverable docs change in the same commit as the behavior they describe; a doc rewritten after the fact costs another review round.

## Orchestrator Responsibilities

The orchestrator operates under specific constraints:

- Plans workflow systems
- Dispatches tasks according to the plan
- Monitors task outputs
- Applies defined responses to outcomes
- Coordinates rather than executes directly

Workers execute tasks. The orchestrator coordinates.

## Documentation

Document workflow plans in a structured location:

```
ctxmod work new plan "<workflow name>" --owner <work item, if any>   # prints the plan file to write
```

Plans persist for reference during execution, audit after completion, and pattern extraction for future workflows.
