# Standards Validation Objective

Verify that artifacts adhere to defined conventions, standards, and best practices. Detect violations of naming conventions, formatting rules, pattern requirements, and quality guidelines. Produce a validation report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.

## Base Behavior

the `validation_objective` skill

## Scope

Standards validation applies to any artifact governed by conventions:

- Source code (naming conventions, formatting rules, idiom compliance, linting rules)
- APIs (naming patterns, versioning conventions, response format standards)
- CLI tools (flag conventions, output formatting, error message patterns)
- Configuration (key naming, value formats, organizational patterns)
- Data (field naming, encoding standards, format conventions)
- Documentation (terminology, style guidelines, formatting rules)
- Commits and changes (message formats, change categorization)
- Any artifact with defined quality or style expectations

Standards vary by context—language style guides, organizational conventions, domain-specific rules, or project-level agreements.

## Core Behavior

1. **Identify standards**: Locate applicable conventions, style guides, linting rules
2. **Extract elements**: Parse artifact components subject to standards
3. **Check compliance**: Match elements against standard requirements
4. **Measure metrics**: Assess quantitative standards where defined
5. **Classify**: Assign severity to each violation
6. **Report**: Write findings to the reserved report path with locations and categories

## Relationship to Discovery and Analysis

Standards validation complements structural validation:

| Validation Type | Focus |
|-----------------|-------|
| Structural | Required elements present and correctly organized |
| Standards | Elements conform to conventions and quality guidelines |

Both feed analysis for unified remediation planning.

the `discovery_objective` skill
the `analysis_objective` skill

## Principles

`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/dry.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`

Context efficiency ensures validation identifies patterns that reduce clarity or increase noise. DRY detects redundancy violations. KISS identifies unnecessary complexity.

## Pre-Validation

Before validating:

1. Identify target artifacts containing elements subject to standards
2. Locate applicable standards (style guides, linting configs, convention docs)
3. Determine which standards apply to which artifact types
4. Establish thresholds for quantitative standards lacking explicit limits
5. Reserve the report path just-in-time (`ctxmod work new report "<title>"`)

Standards types include:
- **Naming**: Identifiers, files, resources follow conventions
- **Formatting**: Layout, spacing, organization meet rules
- **Patterns**: Required idioms, prohibited anti-patterns
- **Quality**: Complexity limits, size constraints, coverage thresholds

## During Validation

Apply the standards validation procedure:

Brief template for a procedure-depth agent (launched from objective depth):

```text
Apply the standards validation procedure to check artifact elements against applicable conventions and quality guidelines.
`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/standards.md`
```

For each artifact:

1. **Extract elements**: Identify components subject to standards
2. **Check naming**: Verify identifiers match conventions
3. **Check formatting**: Verify layout and organization
4. **Check patterns**: Match against required/prohibited patterns
5. **Measure metrics**: Compute quantitative measures against thresholds

Classify all findings by severity:

Brief template for a procedure-depth agent (launched from objective depth):

```text
Classify each standards finding by severity using the standard severity classification procedure.
`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/severity_classification.md`
```

Optional procedures when validation scope warrants:

`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/pattern_detection.md`
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/pattern_aggregation.md`

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Reserve the report path just-in-time and create the structured standards validation report.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

Standards validation reports include:

- **Standards Applied**: Which conventions governed this validation
- **Compliance Status**: Pass/fail summary with violation counts by category
- **Violations**: Specific deviations with locations, expected values, and actual values
- **Metric Analysis**: Measured values vs thresholds for quantitative standards
- **Recommendations**: Priority-ordered corrections

Target length: 400-1200 words depending on artifact count and violation density.

## Tool Constraints

**Allowed**:
- Read: Target artifacts, standards documentation
- Glob: File discovery for multi-artifact validation
- Grep: Pattern matching for convention checking
- Bash: Read-only commands (linters, formatters in check mode)
- Write: Validation report at the reserved report path only

**Forbidden**:
- Modifying source artifacts
- Launching sub-agents
- Writing outside the reserved report path
- Making corrections (validation only)

## Response Format

After writing the report, return a brief response:

```
Wrote standards validation report to <reserved report path>.

Summary: [2-3 sentence summary of compliance status and prominent violations]
```

Do not return the full report content. The report file is the deliverable.

## Variants

| Variant | Focus |
|---------|-------|
| `naming` | Identifier and resource naming conventions |
| `formatting` | Layout, spacing, and organization rules |
| `patterns` | Required idioms and prohibited anti-patterns |
| `metrics` | Quantitative thresholds (complexity, size, coverage) |

Variants specialize standards checking to specific convention types.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Applying standards without context | May be inapplicable | Verify standards govern this artifact |
| Metric absolutism | Context affects appropriateness | Use metrics as indicators |
| Flagging intentional exceptions | Documented deviations are valid | Check for exception markers |
| Reporting without specifics | Hard to remediate | Include expected vs actual |
| Conflating standards sources | Different authorities | Attribute each rule to its source |
