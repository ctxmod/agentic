# Functional Validation Objective

Verify that artifacts achieve their intended purpose and behave correctly. Test that components produce expected outcomes, specifications can be followed, and artifacts serve their consumers. Produce a validation report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.

## Base Behavior

the `validation_objective` skill

## Scope

Functional validation applies to any artifact with observable behavior or purpose:

- Source code (functions return expected values, classes behave as specified)
- APIs (endpoints respond correctly, error handling works)
- CLI tools (commands produce expected output, flags modify behavior)
- Configuration (settings produce expected system behavior)
- Documentation (instructions produce expected outcomes when followed)
- Specifications (requirements are implementable, constraints are achievable)
- Build systems (builds succeed, outputs are correct)
- Data pipelines (transformations produce expected results)
- Any artifact where "does it work?" can be tested

Functional validation answers "does it achieve its purpose?" after other validations answer "is it correct?"

## Core Behavior

1. **Identify purpose**: Determine intended function from artifact context
2. **Define success criteria**: Establish what correct behavior means
3. **Execute validation**: Test artifact against success criteria
4. **Document outcomes**: Record pass/fail status with evidence
5. **Assess impact**: Evaluate consequences of failures
6. **Report**: Write findings to the reserved report path with functional assessment

## Relationship to Other Validation Types

| Validation | Question |
|------------|----------|
| Structural | Does it have correct format? |
| Standards | Does it follow conventions? |
| Completeness | Does it cover required elements? |
| Consistency | Does it align with related artifacts? |
| References | Do dependencies resolve? |
| **Functional** | **Does it achieve its purpose?** |

Functional validation often depends on other validations passing—an artifact cannot function correctly if structurally broken.

the `discovery_objective` skill
the `analysis_objective` skill

## Principles

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/convention_over_configuration.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`

KISS ensures artifacts remain operable by their intended consumers. Convention over configuration establishes predictable behavior. Context efficiency focuses validation on meaningful functional outcomes.

## Pre-Validation

Before validating:

1. Identify artifact purpose from context, metadata, or specification
2. Determine intended consumers and their capabilities
3. Establish success criteria: what outcomes indicate correct function
4. Identify test scenarios covering primary use cases
5. Reserve the report path just-in-time (`ctxmod work new report "<title>"`)

Functional criteria vary by artifact type:

| Artifact Type | Functional Criteria |
|---------------|---------------------|
| Code | Executes correctly, returns expected values |
| API | Responds correctly, handles errors, meets performance |
| CLI | Commands succeed, output is correct, errors are clear |
| Configuration | System behaves as configured |
| Documentation | Following it produces expected outcome |
| Specification | Requirements are implementable |

## During Validation

Apply functional testing appropriate to artifact type:

Brief template for a procedure-depth agent (launched from objective depth):

```text
Apply the structural and completeness validation procedures as prerequisites to functional testing of the artifact.
`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/structural.md`
`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/completeness.md`
```

For each artifact:

1. **Parse purpose**: Extract intended outcome from artifact
2. **Identify scenarios**: Primary use cases that must succeed
3. **Execute scenarios**: Run commands, call functions, follow instructions
4. **Document outcomes**: Record actual vs expected results
5. **Assess failure impact**: Determine severity of deviations

Classify all findings by severity:

Brief template for a procedure-depth agent (launched from objective depth):

```text
Classify each functional finding by severity using the standard severity classification procedure.
`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/severity_classification.md`
```

Optional procedures when validation scope warrants:

`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/boundary_validation.md`
`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/entry_point_detection.md`
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/impact_scoring.md`
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/prioritization.md`

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Reserve the report path just-in-time and create the structured functional validation report.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

Functional validation reports include:

- **Purpose Statement**: Artifact's intended function
- **Test Scenarios**: Use cases validated with expected outcomes
- **Results**: Pass/fail status per scenario with evidence
- **Failure Analysis**: Root causes for functional failures
- **Impact Assessment**: Consequences of failures for consumers
- **Recommendations**: Corrections to restore function

Target length: 500-1500 words depending on scenario count and failure complexity.

## Tool Constraints

**Allowed**:
- Read: Target artifacts, specifications, related context
- Glob: File discovery for multi-artifact validation
- Grep: Content extraction for scenario analysis
- Bash: Read-only commands, test execution, behavior verification
- Write: Validation report at the reserved report path only

**Forbidden**:
- Modifying source artifacts (except temporary test artifacts)
- Launching sub-agents
- Writing outside the reserved report path
- Fixing functional issues (validation only)

## Response Format

After writing the report, return a brief response:

```
Wrote functional validation report to <reserved report path>.

Summary: [2-3 sentence summary of functional status and key failures]
```

Do not return the full report content. The report file is the deliverable.

## Variants

| Variant | Focus |
|---------|-------|
| `execution` | Code and command execution correctness |
| `behavior` | Observable behavior matches specification |
| `integration` | Cross-artifact interaction correctness |
| `usability` | Consumer can achieve intended outcome |

Variants specialize functional testing to artifact categories.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Testing without purpose clarity | No success criteria | Establish purpose first |
| Assuming structural validity implies function | Different concerns | Validate function explicitly |
| Testing edge cases before primary use | Misplaced priority | Start with core scenarios |
| Conflating author intent with consumer need | May diverge | Test against consumer outcomes |
| Destructive testing without recovery | Leaves artifacts broken | Ensure reversibility |
