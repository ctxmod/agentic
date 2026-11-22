# References Validation Procedure

Verify that all cross-references in an artifact resolve to valid targets. This procedure applies to any content containing links, citations, or pointers to other resources.

## Prerequisites

- Target artifact with references identified
- Access to reference targets (local files, remote resources, or metadata)
- Reference syntax patterns defined (links, citations, includes, imports)

## Steps

1. **Extract references** from the target artifact using syntax patterns. Common reference types:
   - **File paths**: Relative or absolute paths to local files
   - **URLs**: Links to external resources
   - **Named references**: Identifiers resolved through lookup (citations, aliases)
   - **Line ranges**: File path with line number specifications
   - **Anchors**: Links to sections within documents

2. **Classify references** by type and resolution method:
   - Local file references: Verify path existence
   - URL references: Verify resource accessibility
   - Named references: Verify lookup resolution
   - Anchors: Verify target section exists

3. **Resolve each reference** using appropriate method:
   - File paths: Check filesystem existence, respect relative path base
   - URLs: Verify accessibility (HEAD request or cached status)
   - Named references: Query index or registry for resolution
   - Anchors: Parse target document for matching anchor

4. **Verify target validity** beyond mere existence:
   - File references: Target is correct type (not directory when file expected)
   - Line ranges: Target file has sufficient lines
   - Anchors: Target section has expected content
   - URLs: Response indicates success (not redirect to error page)

5. **Identify orphaned references** by cross-checking bidirectional links when applicable. A reference expected to be reciprocal should have corresponding back-reference.

6. **Document findings** with reference location, target, resolution status, and failure reason when applicable.

## Decision Points

**Optional vs required references**: Some references may be marked optional. Validate existence but classify missing optional references as warnings, not errors.

**External URL validation**: Network access may be restricted or slow. Use cached validation results when available, flag for manual verification when inaccessible.

**Circular references**: Detect cycles in reference chains. Circular references may be valid (bidirectional links) or errors (infinite loops).

**Relative path ambiguity**: Determine base path for relative references. Document assumption when base is unclear.

## Success Criteria

- All references extracted and classified
- Resolution attempted for each reference
- Invalid references documented with failure reasons
- Orphaned references identified when bidirectional checking applies
- Findings categorized by severity

## Common Issues

**Path case sensitivity**: Filesystem case sensitivity varies by platform. Validate using target platform rules.

**Moved or renamed targets**: References become stale when targets change. Detect patterns suggesting recent moves.

**Transitive resolution**: Reference points to redirect or alias. Decide whether to validate final target or immediate target.

**Fragment-only references**: Anchors without file path reference current document. Validate within same-document scope.

**Encoding issues**: Special characters in paths or URLs require proper encoding. Normalize before resolution.
