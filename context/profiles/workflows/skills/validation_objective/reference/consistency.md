# Consistency Validation Objective

Verify that content maintains alignment within an artifact and across related artifacts. Detect contradictions, terminology drift, conflicting behavior, and misaligned definitions. Produce a validation report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.

## Base Behavior

the `validation_objective` skill

## Scope

Consistency validation applies to any set of artifacts that should align:

- Source code (naming patterns, coding idioms, architectural patterns)
- APIs (naming conventions, response formats, error handling across endpoints)
- CLI tools (flag patterns, output formats, behavior consistency)
- Configuration (value formats, key naming, organizational patterns)
- Documentation (terminology, stated behavior vs actual behavior)
- Specifications (requirement alignment, cross-reference accuracy)
- Data (schema alignment, format consistency across sources)
- Any artifacts where divergence indicates a defect

Consistency spans internal (within one artifact) and external (across related artifacts).

## Core Behavior

1. **Identify scope**: Determine internal consistency, cross-artifact consistency, or both
2. **Extract assertions**: Parse definitions, behaviors, patterns, and values
3. **Group assertions**: Cluster related statements by topic or element
4. **Compare within groups**: Identify conflicts, drift, and ambiguity
5. **Check alignment**: Verify stated behavior matches actual behavior
6. **Report**: Write findings to the reserved report path with conflict details

## Relationship to Discovery and Analysis

Consistency validation detects drift that individual artifact validation misses:

| Validation Focus | Scope |
|------------------|-------|
| Structural | Single artifact format |
| Standards | Single artifact conventions |
| Consistency | Relationships across artifacts |

Discovery identifies related artifacts. Analysis prioritizes contradiction resolution.

the `discovery_objective` skill
the `analysis_objective` skill

## Principles

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`

SSOT prevents contradictions through single authoritative sources. DRY ensures definitions appear once, eliminating drift potential. Reference over duplication maintains consistency by avoiding copies.

## Pre-Validation

Before validating:

1. Define validation scope (internal, cross-artifact, or both)
2. Identify artifact set within scope
3. Determine consistency dimensions (naming, behavior, values, patterns)
4. Establish what alignment means for each dimension
5. Reserve the report path just-in-time (`ctxmod work new report "<title>"`)

Consistency dimensions vary by artifact type:
- **Naming**: Same concepts use same identifiers
- **Behavior**: Same operations produce same results
- **Values**: Same settings have same values across contexts
- **Patterns**: Same patterns applied consistently
- **Definitions**: Stated definitions match actual implementation

## During Validation

Apply the consistency validation procedure:

Brief template for a procedure-depth agent (launched from objective depth):

```text
Apply the consistency validation procedure to detect conflicts, drift, and alignment issues within and across artifacts.
`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/consistency.md`
```

For each scope unit:

1. **Extract assertions**: Identify definitions, behaviors, patterns, values
2. **Group by topic**: Cluster assertions about same concepts
3. **Compare within groups**: Classify as identical, compatible, conflicting, or ambiguous
4. **Detect drift**: Same identifier with different meanings, same concept with different identifiers
5. **Verify alignment**: Compare stated behavior with actual behavior

Classify all findings by severity:

Brief template for a procedure-depth agent (launched from objective depth):

```text
Classify each consistency finding by severity using the standard severity classification procedure.
`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/severity_classification.md`
```

Optional procedures when validation scope warrants:

`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/consumer_search.md`
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/contradiction_resolution.md`
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/pattern_aggregation.md`

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Reserve the report path just-in-time and create the structured consistency validation report.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

Consistency validation reports include:

- **Scope**: Artifacts and dimensions validated
- **Conflicts Found**: Assertion pairs with conflict type and locations
- **Drift Detected**: Identifiers or patterns used inconsistently
- **Alignment Issues**: Stated vs actual behavior mismatches
- **Recommendations**: Resolution approach for each conflict

Target length: 500-1500 words depending on artifact count and conflict density.

## Tool Constraints

**Allowed**:
- Read: Target artifacts, reference implementations
- Glob: File discovery for multi-artifact scope
- Grep: Assertion extraction and pattern search
- Bash: Read-only commands for behavior verification
- Write: Validation report at the reserved report path only

**Forbidden**:
- Modifying source artifacts
- Launching sub-agents
- Writing outside the reserved report path
- Resolving conflicts (validation only)

## Response Format

After writing the report, return a brief response:

```
Wrote consistency validation report to <reserved report path>.

Summary: [2-3 sentence summary of consistency status and key conflicts]
```

Do not return the full report content. The report file is the deliverable.

## Variants

| Variant | Focus |
|---------|-------|
| `internal` | Within single artifact |
| `cross-artifact` | Across related artifacts |
| `naming` | Identifier and pattern consistency |
| `behavior` | Operational consistency |

Variants narrow consistency checking to specific dimensions.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Flagging intentional variation | Context-specific differences are valid | Check scope qualifiers |
| Missing semantic equivalence | Different form, same meaning | Normalize before comparison |
| Resolving conflicts during validation | Premature without analysis | Document for later resolution |
| Ignoring version differences | Artifacts from different versions may differ | Establish version scope |
| Treating specialization as conflict | General and specific coexist | Recognize hierarchy |
