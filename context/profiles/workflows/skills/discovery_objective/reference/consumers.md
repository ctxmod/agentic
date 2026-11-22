# Consumer Discovery Objective

Consumer discovery identifies files that reference, import, or depend on a target. This variant maps dependency relationships to assess change impact, identify coupling patterns, and ensure modifications propagate correctly.

## Base Behavior

the `discovery_objective` skill

## Consumer Categories

### Direct References

Files that explicitly reference the target through imports, links, or path literals. These create hard dependencies where target changes require consumer updates.

### Type References

Files that reference types, interfaces, or schemas defined in the target. Type consumers may compile against the target without runtime imports.

### Documentation Links

Files that link to the target through cross-references, inline tags, or markdown links. Documentation consumers need updates when target paths or content structure changes.

### Semantic Dependencies

Files that depend on concepts, patterns, or conventions established by the target without explicit references. Semantic consumers are harder to detect but equally affected by changes.

## Discovery Methodology

### Consumer Search Process

The core search workflow follows the standard consumer search procedure:

Brief template for a procedure-depth agent (launched from objective depth):

```text
Run the consumer search workflow to find all files that reference, import, or depend on the target.
`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/consumer_search.md`
```

**Application notes for consumer discovery**:

- Extract all identifier types from target (path, symbols, frontmatter, unique strings)
- Execute multi-pattern search across target directories using identifier-specific patterns
- Expand scope to include related references (e.g., `discovery.md` → `discovery-source.md`)
- Classify confirmed consumers by dependency strength (hard, soft, implicit)

### Consumer Classification

Categorize each consumer by relationship strength:

| Strength | Description | Impact |
|----------|-------------|--------|
| **Hard** | Import, require, inline | Breaking change propagates |
| **Soft** | Link, reference | Update recommended |
| **Implicit** | Follows patterns from target | Review recommended |

### Phase 4: Impact Assessment

For each consumer, assess:

- **Update necessity**: Must change, should change, or review only
- **Cascading potential**: Whether consumer changes trigger further propagation
- **Isolation opportunity**: Whether consumer can abstract away the dependency

## Tool Permissions

**Allowed**: Glob (directory scan), Grep (reference search), Read (context verification), Write (reserved report path only)

**Forbidden**: Modifying source files, launching sub-agents, writing outside the reserved report path

## Search Patterns

Search patterns vary by file type and language. The consumer search procedure provides comprehensive templates for:

- **Import patterns**: Language-specific import statements
- **Frontmatter references**: YAML field values and metadata
- **Markdown links**: Cross-references and inline tags

Brief template for a procedure-depth agent (launched from objective depth):

```text
Apply the consumer search patterns to locate import, frontmatter, and markdown-link references to the target.
`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/consumer_search.md`
```

## Report Structure

Brief template for a procedure-depth agent (launched from objective depth):

```text
Create and organize the consumer discovery report at a report path reserved just-in-time via `ctxmod work new report "<title>"`.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
```

Consumer discovery reports include these objective-specific sections:

| Section | Content |
|---------|---------|
| Target | File path, key identifiers, search terms used |
| Consumers by Category | Direct references, type references, documentation links, semantic dependencies |
| Impact Assessment | High/Medium/Low impact consumers with rationale |
| Cascading Dependencies | Consumers that themselves have consumers |
| Related Files | Files examined during discovery |

Each consumer entry includes file:line with reference context.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Searching only exact paths | Miss name-based references | Search all identifier patterns |
| Ignoring semantic dependencies | Incomplete impact picture | Check pattern followers |
| Flat consumer listing | No prioritization | Categorize by impact strength |
| Missing cascading analysis | Underestimate change scope | Identify consumers with consumers |
| Stopping at first match | Miss multiple references | Scan all reference types |

## Examples

**Reference File Consumers**: Target `reference/principles/ssot.md`. Search for path references, `name: ssot` inline tags, and "single source of truth" mentions. Find 12 consumers: 4 rules (hard), 5 skills (soft), 3 agents (soft).

**Agent File Consumers**: Target `agents/discovery/discovery-source.md`. Search for `agent:discovery-source` invocations, skill references, command orchestrations. Find 8 consumers across commands and skills with 3 cascading dependencies.

**Type Definition Consumers**: Target `types/report.ts`. Search for import patterns, type references, interface extensions. Find 15 direct imports and 4 type-only references.

## Related

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
