# Standards Validation Procedure

Verify that written content adheres to defined style, clarity, and expression guidelines. This procedure applies to any prose content governed by writing standards.

## Prerequisites

- Target content identified and accessible
- Applicable standards documented (style guide, anti-patterns list, clarity guidelines)
- Validation scope defined (full document or specific sections)

## Steps

1. **Identify applicable standards** from governing guidelines. Standards typically cover:
   - **Vocabulary constraints**: Preferred terms, prohibited jargon, domain-specific terminology
   - **Structural patterns**: Sentence variety, paragraph length, heading conventions
   - **Anti-patterns**: Formulaic expressions, filler phrases, AI-generated tells
   - **Tone requirements**: Active voice preference, hedging limits, claim proportionality
   - **Formatting conventions**: Lists vs prose, table usage, code block style

2. **Extract prose elements** by parsing the target. Separate narrative content from structural elements (headings, lists, tables, code).

3. **Scan for prohibited patterns** using pattern matching against anti-pattern lists. Flag each match with:
   - Pattern matched
   - Location in document
   - Suggested alternative (if defined)

4. **Assess vocabulary compliance** by identifying terms against constraint lists. Check both prohibited terms and required domain terminology.

5. **Measure structural metrics** for prose elements:
   - Sentence length distribution
   - Paragraph length distribution
   - Active vs passive voice ratio
   - Transition variety

6. **Compare metrics to targets** defined in standards. Flag deviations outside acceptable ranges.

7. **Document findings** organized by standard category. Include specific violations with locations and severity.

## Decision Points

**Conflicting standards**: When guidelines contradict, favor more specific guidance over general principles.

**Context-dependent standards**: Some standards apply only to certain content types. Verify applicability before flagging violations.

**Standards not quantified**: When standards use qualitative language ("use sparingly"), establish reasonable thresholds for validation.

**Embedded content**: Technical content (code, data, quotes) may exempt from prose standards. Identify boundaries before validation.

## Success Criteria

- No prohibited patterns detected
- Vocabulary complies with constraints
- Structural metrics within acceptable ranges
- Tone aligns with guidelines
- Findings documented with locations and categories

## Common Issues

**Over-flagging technical content**: Terms appropriate in technical context flagged as jargon. Respect domain boundaries.

**Metric gaming**: Content that satisfies metrics but lacks quality. Use metrics as indicators, not absolutes.

**Style guide gaps**: Unaddressed scenarios require judgment. Document decisions for consistency.

**Regional variations**: Spelling, punctuation, and style conventions vary by locale. Apply standards consistently within scope.

**Legacy content exemptions**: Existing content may predate current standards. Clarify whether validation applies retroactively.
