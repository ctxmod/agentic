# Completeness Validation Objective

Verify that artifacts cover all required elements within their defined scope. Detect gaps, missing coverage, and insufficient depth. Produce a validation report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.

## Base Behavior

the `validation_objective` skill

## Scope

Completeness validation applies to any artifact with coverage requirements:

- Source code (required methods, interface implementation, test coverage)
- APIs (endpoint coverage, parameter handling, error cases)
- CLI tools (command coverage, flag handling, help completeness)
- Configuration (required settings, environment coverage)
- Documentation (topic coverage, required sections)
- Specifications (case coverage, requirement traceability)
- Test suites (scenario coverage, edge case handling)
- Data (field population, record completeness)
- Any artifact where "all required X" can be defined

Completeness criteria derive from specifications, contracts, conventions, or explicit requirements.

## Core Behavior

1. **Define criteria**: Establish completeness requirements from scope documentation
2. **Extract coverage**: Inventory elements present in artifacts
3. **Map coverage**: Compare extracted elements against requirements
4. **Identify gaps**: Document missing and partially covered items
5. **Assess distribution**: Evaluate coverage balance across criteria
6. **Report**: Write findings to the reserved report path with coverage map and gap analysis

## Relationship to Discovery and Analysis

Completeness validation assesses coverage depth:

| Validation Type | Question Answered |
|-----------------|-------------------|
| Structural | Does it have the right format? |
| Completeness | Does it cover everything required? |
| Standards | Does it follow conventions? |

Completeness validation often requires discovery to identify scope boundaries and analysis to prioritize gap remediation.

the `discovery_objective` skill
the `analysis_objective` skill

## Principles

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`

SSOT ensures scope definitions are authoritative. DRY prevents flagging intentional references as gaps. Context efficiency focuses validation on meaningful coverage, not exhaustive enumeration.

## Pre-Validation

Before validating:

1. Locate scope definition (requirements list, interface contract, coverage criteria)
2. Identify target artifacts within scope
3. Determine criteria types: element coverage, case coverage, depth requirements
4. Establish benchmarks using peer artifacts when quantification is absent
5. Reserve the report path just-in-time (`ctxmod work new report "<title>"`)

Completeness criteria vary by artifact type:
- **Interface compliance**: Required methods, properties, or endpoints implemented
- **Case coverage**: Scenarios, conditions, edge cases handled
- **Element coverage**: Required components, fields, or sections present
- **Depth requirements**: Level of detail per element

## During Validation

Apply the completeness validation procedure:

Brief template for a procedure-depth agent (launched from objective depth):

```text
Apply the completeness validation procedure to map artifact coverage against requirements and identify gaps.
`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/completeness.md`
```

For each artifact:

1. **Extract covered elements**: Identify what the artifact provides
2. **Map to criteria**: Mark each requirement as covered, partial, or missing
3. **Assess depth**: Compare detail level against requirements or benchmarks
4. **Identify gaps**: List uncovered items with relationship to existing content
5. **Evaluate distribution**: Flag uneven coverage across criteria

Classify all findings by severity:

Brief template for a procedure-depth agent (launched from objective depth):

```text
Classify each completeness finding by severity using the standard severity classification procedure.
`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/severity_classification.md`
```

Optional procedures when validation scope warrants:

`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/broad_scan.md`
`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/pattern_detection.md`
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/confidence_assessment.md`

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Reserve the report path just-in-time and create the structured completeness validation report.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

Completeness validation reports include:

- **Scope Definition**: Completeness criteria used for validation
- **Coverage Map**: Each requirement with coverage status (covered/partial/missing)
- **Gap Analysis**: Missing items with impact assessment
- **Distribution Assessment**: Balance evaluation across criteria categories
- **Recommendations**: Priority-ordered items to address gaps

Target length: 500-1500 words depending on scope breadth and gap count.

## Tool Constraints

**Allowed**:
- Read: Target artifacts, scope documentation, peer artifacts for benchmarks
- Glob: File discovery for multi-artifact scope
- Grep: Content extraction for coverage mapping
- Bash: Read-only commands for coverage analysis
- Write: Validation report at the reserved report path only

**Forbidden**:
- Modifying source artifacts
- Launching sub-agents
- Writing outside the reserved report path
- Adding content (validation only)

## Response Format

After writing the report, return a brief response:

```
Wrote completeness validation report to <reserved report path>.

Summary: [2-3 sentence summary of coverage status and key gaps]
```

Do not return the full report content. The report file is the deliverable.

## Variants

| Variant | Focus |
|---------|-------|
| `interface` | Contract and interface implementation completeness |
| `cases` | Scenario, condition, and edge case coverage |
| `elements` | Required component and field presence |
| `depth` | Detail level per covered element |

Variants specialize completeness criteria to specific dimensions.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Flagging beyond scope | Creates false gaps | Anchor to defined criteria |
| Treating delegation as gaps | Intentional indirection | Recognize reference patterns |
| Requiring exhaustive coverage | Diminishing returns | Focus on defined requirements |
| Ignoring implicit coverage | Overstates gaps | Accept clear semantic equivalence |
| Equal weight to all gaps | Unclear priorities | Assess impact per gap |
