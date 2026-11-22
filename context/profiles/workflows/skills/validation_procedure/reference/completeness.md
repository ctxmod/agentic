# Completeness Validation Procedure

Verify that an artifact covers all required topics, cases, or elements within its defined scope. This procedure detects gaps, missing coverage, and incomplete treatment.

## Prerequisites

- Target artifact identified and accessible
- Scope definition available (topic list, coverage requirements, completeness criteria)
- Access to reference materials defining expected coverage

## Steps

1. **Define completeness criteria** from scope documentation. Criteria types include:
   - **Topic coverage**: Subjects that must be addressed
   - **Case coverage**: Scenarios, conditions, or variations to include
   - **Element coverage**: Components, sections, or fields required
   - **Depth requirements**: Level of detail expected per topic

2. **Extract covered content** from the target artifact. Build inventory of:
   - Topics addressed (from headings, sections, explicit statements)
   - Cases handled (from examples, conditions, variations discussed)
   - Elements present (from structural analysis)
   - Depth achieved (from word count, detail level, subsection presence)

3. **Map coverage to criteria** by comparing extracted content against requirements:
   - Explicitly covered: Topic addressed directly with required depth
   - Implicitly covered: Topic addressed through related content
   - Partially covered: Topic mentioned but lacking required depth
   - Not covered: Topic absent from artifact

4. **Identify gaps** from uncovered and partially covered items. For each gap:
   - Specify missing topic, case, or element
   - Note relationship to covered content (if implicit coverage exists)
   - Assess impact of the gap

5. **Assess distribution** of coverage across criteria. Uneven coverage (some topics exhaustive, others thin) indicates prioritization issues even when all topics appear.

6. **Document findings** with coverage map, gap list, and distribution assessment.

## Decision Points

**Implicit vs explicit coverage**: Some topics may be covered through inference. Determine whether implicit coverage satisfies criteria.

**Scope boundaries**: Content beyond defined scope is not a gap. Verify scope definition before flagging missing items.

**Depth ambiguity**: When depth requirements lack quantification, use peer artifacts as benchmarks.

**Dynamic scope**: Scope may expand based on discovered content. Document scope changes during validation.

## Success Criteria

- All criteria items assessed for coverage
- Coverage level documented for each item (explicit, implicit, partial, none)
- Gaps identified with impact assessment
- Distribution evaluated across criteria
- Findings include specific missing topics with suggested additions

## Common Issues

**Scope creep in validation**: Flagging content beyond original scope as gaps. Anchor validation to defined criteria.

**Synonymous coverage**: Same topic addressed using different terminology. Recognize semantic equivalence.

**Assumed knowledge**: Implicit prerequisites treated as gaps. Distinguish introductory content from reference content.

**Over-counting implicit coverage**: Stretching interpretation to mark gaps as covered. Require clear relationship for implicit coverage.

**Single-dimension assessment**: Checking topic presence without depth verification. Combine presence and depth for accurate coverage assessment.
