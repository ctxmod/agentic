# Consistency Validation Procedure

Verify that content maintains alignment within an artifact and across related artifacts. This procedure detects contradictions, terminology drift, and conflicting guidance.

## Prerequisites

- Target artifact(s) identified and accessible
- Scope defined (internal consistency, cross-file consistency, or both)
- Related artifacts identified when checking cross-file consistency

## Steps

1. **Identify consistency dimensions** relevant to the artifacts:
   - **Terminology**: Same concepts use same terms throughout
   - **Facts**: Stated information does not contradict itself
   - **Guidance**: Instructions and recommendations align
   - **Formatting**: Style conventions applied uniformly
   - **References**: Cross-references describe targets accurately

2. **Extract assertions** from target artifacts. Assertions are statements of fact, definition, or guidance that can be compared:
   - Definitions: "X means Y" or "X is defined as Y"
   - Facts: "X contains Y" or "X does Y"
   - Guidance: "Do X" or "Avoid Y" or "Prefer X over Y"
   - Quantities: "X is N" or "X takes N units"

3. **Group related assertions** by topic or subject. Assertions about the same concept form comparison sets.

4. **Compare assertions within groups** for alignment:
   - **Identical**: Same assertion stated multiple times (redundant but consistent)
   - **Compatible**: Different aspects of same topic, no contradiction
   - **Conflicting**: Mutually exclusive statements about same subject
   - **Ambiguous**: Statements that could be read as conflicting depending on interpretation

5. **Identify terminology drift** by comparing term usage across artifacts:
   - Same term used with different meanings
   - Same concept referred to by different terms without explicit aliasing
   - Abbreviations used inconsistently

6. **Check reference accuracy** by comparing reference descriptions with target content. A reference claiming "X describes Y" should link to content that actually describes Y.

7. **Document findings** with specific assertion pairs, locations, and conflict type.

## Decision Points

**Intentional variation**: Some differences are deliberate (context-specific guidance). Verify intent before flagging as inconsistency.

**Version differences**: Artifacts from different versions may legitimately differ. Establish version scope before validation.

**Scope conflicts**: Guidance applicable in narrow scope may conflict with broader guidance. Check scope qualifiers.

**Ambiguity resolution**: When statements could be interpreted as consistent or conflicting, favor consistent interpretation unless evidence suggests otherwise.

## Success Criteria

- All relevant assertions extracted and grouped
- Comparisons completed within and across groups
- Conflicts documented with specific locations and conflict type
- Terminology drift identified
- Reference accuracy verified

## Common Issues

**Semantic equivalence missed**: Different phrasing for same assertion flagged as inconsistent. Normalize meaning before comparison.

**Context collapse**: Comparing assertions from different contexts as if equivalent. Preserve context in comparison.

**Stale references**: References to removed or renamed content appear as inaccuracies. Distinguish missing targets from inaccurate descriptions.

**Granularity mismatch**: General statements and specific statements about same topic may appear to conflict. Recognize specialization relationships.

**Cascade effects**: One inconsistency causes apparent inconsistencies in dependent content. Identify root causes.
