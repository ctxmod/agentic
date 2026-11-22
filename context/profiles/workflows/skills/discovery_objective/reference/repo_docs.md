# Repository Documentation Discovery Objective

Discover and catalog documentation files, assess organization patterns, identify coverage gaps, and evaluate documentation quality using SSOT and clarity principles.

## Base Behavior

the `discovery_objective` skill

## Documentation Specialization

Extends base discovery with documentation-specific patterns:

- **Type Detection**: README, guides, API docs, tutorials, specifications, architecture
- **Structure Mapping**: Hierarchical organization, cross-reference networks
- **Coverage Assessment**: Documented vs undocumented areas, gap identification
- **Quality Evaluation**: SSOT adherence, clarity standards, structural consistency

## Discovery Methodology

### Phase 1: Documentation File Discovery

Brief template for a procedure-depth agent (launched from objective depth):

```text
Perform a broad scan to discover documentation files across the repository.
`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/broad_scan.md`
```

**Documentation-specific patterns**:

Apply the broad-scan procedure using these Glob patterns for documentation discovery:

- Root: `README.md`, `CONTRIBUTING.md`, `LICENSE`, `CHANGELOG.md`
- Guides: `docs/guides/**/*`, `guides/**/*`, `*-guide.md`
- API: `docs/api/**/*`, `api/**/*`, `openapi.yaml`, `swagger.json`
- Tutorials: `docs/tutorials/**/*`, `*-tutorial.md`
- Specifications: `docs/spec/**/*`, `*-spec.md`, `SPECIFICATION.md`
- Architecture: `docs/architecture/**/*`, `ARCHITECTURE.md`, `DESIGN.md`
- Internal: `context/**/*` (the repository's ctxmod context module), `.claude/**/*`, `.github/**/*`, `DEVELOPMENT.md`

**Exclusions** (as per broad-scan procedure):

Always exclude files in `**/*/draft/**/*`, `**/*/archive/**/*`, or `**/*/legacy/**/*` directories, unless explicitly scoped.

### Phase 2: Type Classification

Categorize discovered files by purpose:

| Type | Purpose |
|------|---------|
| Overview | Project introductions, getting started |
| Guides | How-to instructions, procedural documentation |
| API | Interface specifications, endpoint documentation |
| Tutorials | Learning-oriented step-by-step content |
| Specs | Technical specifications, requirements |
| Architecture | Design documentation, system overviews |
| Internal | Contributor guidelines, development workflows |

### Phase 3: Structure Mapping

Build hierarchical documentation tree:

1. Map directory organization (`docs/{category}/{topic}`)
2. Identify cross-references (relative links, `../` patterns)
3. Note key entry points (root README, main API doc)
4. Document navigation patterns (table of contents, indices)

### Phase 4: Coverage Assessment

Evaluate documentation completeness:

1. Identify well-documented areas (multiple doc types present)
2. Detect undocumented components (code without corresponding docs)
3. Note missing documentation types (e.g., API docs exist but no tutorials)
4. Check for orphaned pages (no inbound links)

### Phase 5: Quality Evaluation

Assess documentation against the documentation standards (the writing module's `documentation` profile and the repository's own context conventions):

1. **SSOT compliance**: Check for information duplication across docs
2. **Clarity**: Active voice, direct statements, measured claims
3. **Structure**: Frontmatter presence, heading hierarchy, section lengths
4. **References**: Cross-references vs content duplication

## Tool Permissions

**Allowed**: Glob (file patterns), Grep (content search), Read (file examination), Bash (read-only commands), Write (reserved report path only)

**Forbidden**: Modifying source files, launching sub-agents, writing outside the reserved report path

## Report Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Organize and write the documentation discovery report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
```

Documentation discovery reports include these objective-specific sections:

- **Documentation Files**: Table with type, count, and location columns
- **Documentation Structure**: Hierarchical tree showing organization
- **Key Resources**: Entry points (root README, main API doc, architecture overview)
- **Coverage Assessment**: Well-documented areas, gaps, recommendations
- **Quality Observations**: SSOT adherence, clarity patterns, structural consistency

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Cataloging every file | Low signal, high noise | Summarize by type and purpose |
| Missing file:line references | Unverifiable claims | Include `path/to/file.md:15-22` |
| Ignoring SSOT violations | Missed quality issues | Identify information duplication |
| Assessing without principles | Subjective judgments | Reference SSOT and clarity standards |
| File counts without context | Meaningless metrics | Explain what counts mean |

## Examples

**Monorepo Documentation Discovery**: Glob `**/*.md` across 12 packages. Find 45 docs: 6 guides, 8 API docs, 12 READMEs, 3 architecture docs, 16 internal. Detect gap: no deployment documentation. Note SSOT violation: authentication flow documented in 3 places.

**Library API Documentation**: Discover 5 API guides, 2 reference docs. Coverage good for endpoints but missing error handling examples. Structure well-organized under `docs/api/`. Cross-references use relative links consistently.

## Related

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`
`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/clarity.md`
