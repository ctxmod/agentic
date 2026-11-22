# References Validation Objective

Verify that all cross-references resolve to valid targets. Detect broken links, missing dependencies, invalid identifiers, and orphaned references. Produce a validation report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.

## Base Behavior

the `validation_objective` skill

## Scope

Reference validation applies to any artifact containing pointers to other resources:

- Source code (imports, includes, module references, type references)
- Configuration (file paths, resource identifiers, external references)
- APIs (schema references, endpoint links, type definitions)
- CLI tools (help text links, configuration references, file paths)
- Documentation (cross-references, links, citations)
- Build systems (dependency declarations, artifact references)
- Data (foreign keys, resource URIs, external identifiers)
- Any artifact with dependencies or cross-references

Reference types vary: file paths, URLs, identifiers, symbolic names, or custom reference schemes.

## Core Behavior

1. **Extract references**: Parse cross-references using appropriate patterns
2. **Classify**: Group references by type and resolution method
3. **Resolve**: Attempt resolution for each reference
4. **Verify targets**: Confirm target validity beyond existence
5. **Identify orphans**: Detect missing expected references
6. **Report**: Write findings to the reserved report path with resolution status

## Relationship to Discovery and Analysis

Reference validation ensures dependency integrity:

| Phase | Contribution |
|-------|--------------|
| Discovery | Identifies artifacts and their reference patterns |
| Reference Validation | Verifies link and dependency integrity |
| Analysis | Prioritizes broken reference remediation |

Reference validation often runs after structural validation, as valid structure enables reference extraction.

the `discovery_objective` skill
the `analysis_objective` skill

## Principles

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`

SSOT ensures references point to authoritative sources. Reference over duplication motivates maintaining valid cross-references. Context efficiency guides which references warrant validation.

## Pre-Validation

Before validating:

1. Identify target artifacts containing references
2. Document reference patterns (syntax for imports, links, paths, identifiers)
3. Establish resolution methods for each reference type
4. Determine bidirectional or dependency-graph expectations
5. Reserve the report path just-in-time (`ctxmod work new report "<title>"`)

Reference types include:
- **File paths**: Relative or absolute paths to local resources
- **Identifiers**: Named references resolved through lookup
- **URLs**: Links to external resources
- **Dependencies**: Package, module, or library references
- **Symbolic**: Anchors, aliases, or custom reference schemes

## During Validation

Apply the references validation procedure:

Brief template for a procedure-depth agent (launched from objective depth):

```text
Apply the references validation procedure to resolve each cross-reference and verify target validity.
`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/references.md`
```

For each artifact:

1. **Extract references**: Use patterns appropriate to artifact type
2. **Classify by type**: Determine resolution method per reference
3. **Resolve references**: Apply type-appropriate resolution
4. **Verify target validity**: Check type correctness, accessibility, content presence
5. **Check completeness**: Verify expected references exist

Classify all findings by severity:

Brief template for a procedure-depth agent (launched from objective depth):

```text
Classify each reference finding by severity using the standard severity classification procedure.
`ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/severity_classification.md`
```

Optional procedures when validation scope warrants:

`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/import_extraction.md`
`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/graph_construction.md`
`ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/impact_scoring.md`

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Reserve the report path just-in-time and create the structured reference validation report.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

Reference validation reports include:

- **Reference Types Found**: Classification summary with counts
- **Resolution Status**: Pass/fail summary by type
- **Broken References**: Failed resolutions with source location, target, and failure reason
- **Missing References**: Expected references not found
- **Dependency Issues**: Circular dependencies, version conflicts (when applicable)

Target length: 400-1200 words depending on reference count and failure density.

## Tool Constraints

**Allowed**:
- Read: Target artifacts, reference targets
- Glob: File discovery and path resolution
- Grep: Reference extraction using patterns
- Bash: Read-only commands for resolution checking
- Write: Validation report at the reserved report path only

**Forbidden**:
- Modifying source artifacts
- Launching sub-agents
- Writing outside the reserved report path
- Making corrections (validation only)
- Network requests without explicit authorization

## Response Format

After writing the report, return a brief response:

```
Wrote reference validation report to <reserved report path>.

Summary: [2-3 sentence summary of resolution status and key failures]
```

Do not return the full report content. The report file is the deliverable.

## Variants

| Variant | Focus |
|---------|-------|
| `imports` | Source code imports and includes |
| `dependencies` | Package and module dependencies |
| `paths` | File and directory path references |
| `external` | URLs and external resource accessibility |

Variants narrow validation scope to specific reference types.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Validating external resources without permission | Network access may be restricted | Flag for manual verification |
| Ignoring optional references | Broken optional links still degrade quality | Report as warnings |
| Missing transitive resolution | Redirects may hide failures | Follow to final target |
| Platform-specific assumptions | Path rules vary by system | Match target platform rules |
| Reporting target without source | Hard to locate for remediation | Include source location |
