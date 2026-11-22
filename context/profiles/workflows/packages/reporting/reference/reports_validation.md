# Validation Report Writing Objective

Create a validation report that documents compliance status and categorizes findings. Validation reports verify that artifacts meet defined requirements, conventions, or expectations. Each report provides pass/fail status, violation details with severity classification, and remediation guidance. Produce the report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.

## Base Behavior

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports.md" />

## Report Schema

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_validation.md" />

The schema defines frontmatter requirements, required and optional sections, severity classification, violation documentation format, and length guidelines. Follow the schema for report structure.

## Scope

Validation reports document compliance assessment across any domain:

- Structural validation (format, organization, required elements)
- Completeness validation (coverage, depth, scope adherence)
- Consistency validation (internal alignment, cross-artifact coherence)
- Standards validation (conventions, best practices, style rules)
- Reference validation (link integrity, dependency resolution)
- Functional validation (purpose achievement, behavioral correctness)

Validation reports answer "does it meet requirements?"—distinct from discovery (what exists) and analysis (what it means).

## Relationship to Other Report Types

Validation reports integrate with the broader workflow:

| Report Type | Relationship |
|-------------|--------------|
| Discovery | May precede validation—identifies artifacts to validate |
| Analysis | May follow validation—prioritizes remediation |
| Validation | Primary output—documents compliance status |
| Summary | May aggregate validations—cross-artifact compliance |

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_discovery.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_analysis.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/reports_summary.md" />

## Principles

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/convention_over_configuration.md`

SSOT ensures validation criteria reference authoritative requirements. Context efficiency focuses reports on significant violations rather than exhaustive compliance lists. Convention over configuration establishes predictable defaults when explicit requirements are absent.

## Core Behavior

1. **Assess**: Evaluate artifacts against defined requirements
2. **Classify**: Assign severity to each violation
3. **Document**: Record violations with location and evidence
4. **Summarize**: Provide compliance status overview
5. **Write**: Produce report document at the reserved report path

## Pre-Writing

Before drafting:

1. Confirm validation criteria are defined (requirements, standards, conventions)
2. Collect all findings with source locations
3. Classify each finding by severity
4. Group findings by category (type, artifact, or severity)
5. Reserve the report path just-in-time (`ctxmod work new report "<title>"`)

`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/severity_classification.md`

## During Writing

`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/clarity.md`
`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/anti_patterns.md`

While authoring:

- Include location for every violation (`path/to/file.ext:15-42`)
- State both observed and expected states
- Reference the specific requirement violated
- Apply consistent severity classification
- Group related violations to reduce redundancy
- Provide actionable remediation guidance

## Document Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Create and organize the validation report following the report creation and organization procedures.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
```

Validation reports follow report creation patterns with these specifics:

- Frontmatter includes `type: validation` and validation variant
- Requirements section documents validation criteria source
- Violations section uses consistent structure per finding
- Severity appears prominently for each violation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Classify each violation's severity following the severity classification procedure.
`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/severity_classification.md`
```

## Tool Constraints

**Allowed**:
- Read: Artifacts, requirements, standards
- Glob: Pattern matching for multi-artifact validation
- Grep: Pattern search for violation detection
- Write: Report document at the reserved report path only

**Forbidden**:
- Modifying source artifacts
- Launching sub-agents
- Writing outside the reserved report path
- Correcting violations (validation only)

## Response Format

After writing the report, return a brief response:

```
Wrote validation report to <reserved report path>.

Summary: [2-3 sentence summary of compliance status and key violations]
```

Do not return the full report content. The document is the deliverable.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Violations without location | Cannot remediate | Include path:line for every finding |
| Missing expected state | Unclear correction | State what should exist |
| Equal severity for all | Unclear priorities | Apply severity classification |
| Validating without criteria | Arbitrary judgment | Reference defined requirements |
| Stopping at first violation | Incomplete validation | Report all violations |
| Correcting during validation | Scope creep | Document for later remediation |
