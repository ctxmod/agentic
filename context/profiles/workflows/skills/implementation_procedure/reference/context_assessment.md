# Context Assessment Procedure

Determine whether current understanding supports safe modification of the target system. This procedure prevents premature action by establishing explicit sufficiency criteria before changes begin.

## Prerequisites

- Target components identified (files, modules, artifacts, or system elements)
- Initial exploration completed (structure mapped, related elements located)
- Modification scope defined (what will change, approximate boundaries)

## Steps

### 1. Define Sufficiency Criteria

Establish what understanding is required before modification:

| Criterion | Question | Evidence Required |
|-----------|----------|-------------------|
| **Behavior** | What does the target do? | Observed inputs, outputs, side effects |
| **Dependencies** | What relies on this target? | Consumers list with usage patterns |
| **Interfaces** | What contracts exist? | Signatures, schemas, expectations |
| **Constraints** | What invariants must hold? | Documented rules, implicit assumptions |
| **Recovery** | How to restore prior state? | Backup mechanism, version control status |

### 2. Audit Current Knowledge

For each criterion, assess current state:

- **Complete**: All evidence gathered, verified by examination
- **Partial**: Some evidence exists, gaps identified
- **Missing**: No evidence gathered

Document specific gaps: "Dependency: Partial—found 3 consumers via search, but indirect references not traced."

### 3. Quantify Risk of Gaps

Evaluate each gap against modification scope:

**High risk**: Gap directly affects modified elements
- Missing consumer of function being changed
- Unknown constraint violated by planned modification
- Undocumented interface dependency

**Medium risk**: Gap indirectly affects modified elements
- Potential side effects in related subsystem
- Partial understanding of adjacent behavior

**Low risk**: Gap unlikely to affect modification
- Unrelated system components
- Read-only dependencies

### 4. Determine Sufficiency

Apply decision framework:

| High-Risk Gaps | Medium-Risk Gaps | Decision |
|----------------|------------------|----------|
| 0 | 0-2 | **Proceed**: Context sufficient |
| 0 | 3+ | **Proceed with caution**: Document assumptions |
| 1+ | Any | **Block**: Additional discovery required |

### 5. Document Assessment

Record the evaluation:

```markdown
## Context Assessment

### Sufficiency Status
[Proceed / Proceed with caution / Block]

### Knowledge State
| Criterion | Status | Evidence | Gaps |
|-----------|--------|----------|------|
| Behavior | Complete | Examined source, traced execution | None |
| Dependencies | Partial | Found 4 consumers | Indirect refs unknown |

### Risk Evaluation
[High/medium/low risk gaps with rationale]

### Assumptions
[Explicit assumptions if proceeding with gaps]

### Required Actions
[If blocked: specific discovery needed]
```

## Decision Points

**Ambiguous dependency impact**: Treat as high-risk until verified. Search for additional consumers or trace execution paths.

**Time pressure to proceed**: Document assumptions explicitly. Smaller change scope reduces gap risk. Never skip assessment for urgency.

**No clear sufficiency threshold**: Default to stricter criteria for critical systems, relaxed for isolated components.

**Conflicting evidence**: Block until resolved. Contradictions indicate incomplete understanding.

## Success Criteria

- All sufficiency criteria evaluated with evidence citations
- Gaps quantified with risk levels
- Clear proceed/block decision recorded
- Assumptions documented when proceeding with partial knowledge
- Blocked assessments specify what additional discovery is required

## Common Issues

**Overconfidence from familiarity**: Prior knowledge of similar systems does not substitute for examination of this system. Verify assumptions against actual artifacts.

**Hidden dependencies**: Indirect references (configuration files, environment variables, runtime injection) evade code search. Check configuration and runtime patterns.

**Incomplete consumer search**: Pattern matching misses consumers using aliases, dynamic references, or external invocations. Broaden search patterns or trace from entry points.

**Underestimating constraint complexity**: Simple modifications can violate implicit invariants (ordering, uniqueness, timing). Identify constraints through behavior observation, not just documentation.

**Skipping recovery verification**: Assuming version control suffices without confirming clean working state, committed baseline, or backup existence. Verify recovery mechanism before modifying.
