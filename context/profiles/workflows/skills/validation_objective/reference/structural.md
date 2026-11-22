# Structural Validation Objective

Verify that artifacts conform to expected format and structure. Detect missing elements, malformed content, ordering violations, and structural inconsistencies. Produce a validation report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.

## Base Behavior

the `validation_objective` skill

## Scope

Structural validation applies to any artifact with structural expectations:

- Source code (function signatures, class hierarchies, module organization)
- Configuration files (required fields, valid types, schema compliance)
- API definitions (endpoint structure, parameter formats, response shapes)
- CLI tools (command structure, flag definitions, argument patterns)
- Data files (record formats, field presence, type constraints)
- Documentation (section organization, required headings, metadata)
- Specifications (required sections, cross-reference patterns)
- Directory structures (expected files, naming conventions, hierarchy)

The artifact type determines which structural rules apply.

## Core Behavior

1. **Identify requirements**: Determine structural definition for target artifacts
2. **Extract structure**: Parse artifacts to inventory elements and relationships
3. **Compare**: Check extracted structure against requirements
4. **Classify**: Assign severity to each violation
5. **Report**: Write findings to the reserved report path with locations and violation types

## Relationship to Discovery and Analysis

Structural validation operates between discovery and analysis:

| Phase | Input | Output |
|-------|-------|--------|
| Discovery | None | Artifact inventories, structure maps |
| Structural Validation | Artifacts + structure definitions | Compliance findings |
| Analysis | Validation reports | Prioritized remediation recommendations |

Structural validation receives artifacts identified through discovery. Its findings feed analysis for prioritization and planning.

the `discovery_objective` skill
the `analysis_objective` skill

## Principles

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/convention_over_configuration.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`

SSOT ensures structure definitions are authoritative. Convention over configuration provides default expectations when explicit definitions are absent. Context efficiency keeps validation focused on meaningful violations.

## Pre-Validation

Before validating:

1. Identify target artifacts and their governing structure definitions
2. Locate schema, specification, reference exemplar, or convention documentation
3. Define validation scope (full artifact, specific components, or pattern across artifacts)
4. Reserve the report path just-in-time (`ctxmod work new report "<title>"`)

When no explicit definition exists, infer requirements from reference exemplars, category conventions, or established patterns. Document inferred requirements before validation.

## During Validation

Apply the structural validation procedure:

Brief template for a procedure-depth agent (launched from objective depth):

```text
Apply the structural validation procedure to check artifact structure against the governing definition.
`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/structural.md`
```

For each artifact:

1. **Extract structure**: Parse elements, locations, relationships
2. **Check required elements**: Verify presence and correct positioning
3. **Check prohibited elements**: Flag violations
4. **Verify nesting and ordering**: Compare against defined rules
5. **Assess element completeness**: Empty or malformed elements fail validation

Classify all findings by severity:

Brief template for a procedure-depth agent (launched from objective depth):

```text
Classify each structural finding by severity using the standard severity classification procedure.
`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/severity_classification.md`
```

Optional procedures when validation scope warrants:

`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/pattern_detection.md`
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/prioritization.md`

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Reserve the report path just-in-time and create the structured structural validation report.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

Structural validation reports include:

- **Requirements Summary**: Structure definition source and key requirements
- **Compliance Status**: Pass/fail summary with counts by severity
- **Violations**: Grouped by type with specific locations and details
- **Recommendations**: Suggested corrections for each violation category

Target length: 400-1200 words depending on artifact count and violation density.

## Tool Constraints

**Allowed**:
- Read: Target artifacts, structure definitions, reference exemplars
- Glob: File discovery for multi-artifact validation
- Grep: Pattern search for element detection
- Bash: Read-only commands for structure inspection
- Write: Validation report at the reserved report path only

**Forbidden**:
- Modifying source artifacts
- Launching sub-agents
- Writing outside the reserved report path
- Making corrections (validation only)

## Response Format

After writing the report, return a brief response:

```
Wrote structural validation report to <reserved report path>.

Summary: [2-3 sentence summary of compliance status and key violations]
```

Do not return the full report content. The report file is the deliverable.

## Variants

| Variant | Focus |
|---------|-------|
| `schema` | Data structure and type compliance |
| `hierarchy` | Nesting levels and parent-child relationships |
| `interface` | API, CLI, or module interface structure |
| `organization` | File and directory structure patterns |

Variants add specific validation rules to this base objective.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Validating without definition | Arbitrary judgments | Establish requirements first |
| Stopping at first violation | Incomplete validation | Report all violations |
| Equal severity for all findings | Unclear priorities | Classify by impact |
| Proposing corrections in report | Scope creep | Separate validation from remediation |
| Ignoring inferred requirements | Undocumented expectations | Document assumptions |
