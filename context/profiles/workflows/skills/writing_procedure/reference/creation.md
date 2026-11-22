# Report Creation Procedure

Transform raw findings into structured reports with consistent organization and full source attribution.

## Prerequisites

- Findings collected with source references (file paths, sections, line ranges, URLs)
- Report type identified (determines required sections)
- Output path determined via organization procedure
- Structure definition available for report type
- Reader calibration for the intended audience `ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/academic_register.md`

## Steps

### 1. Initialize Report Structure

Create file at designated path with frontmatter:

```markdown
---
type: {report-type}
date: {YYYY-MM-DD}
time: {HH-mm}
source: {context-path or "general"}
objective: {objective-name}
---

# {Report Title}
```

Set title to reflect report content—avoid generic titles like "Report" or "Summary."

### 2. Write Summary Section

Open with concise overview (1-3 sentences):

- State scope examined
- Identify key findings count or categories
- Note overall assessment when applicable

Avoid detail—summary provides orientation, not exhaustive content.

### 3. Organize Findings by Category

Group related findings under descriptive headings:

```markdown
## {Category Name}

### {Finding Title}

{Finding content with supporting evidence}

- **Source**: {attribution with location}
- **Confidence**: {assessment level when applicable}
```

Use consistent heading depth: H2 for categories, H3 for individual findings.

### 4. Add Evidence Attribution

Attach source information to every claim:

| Attribution Type | Format |
|------------------|--------|
| File reference | `path/to/file.ext:15-42` |
| Section reference | `report.md § Section Name` |
| External source | `[Source Title](URL)` |
| Verbal/observation | `[observation: context]` |

Position attribution near the claim it supports—inline or immediately following.

### 5. Include Recommendations Section

When findings suggest actions, add structured recommendations with rationale and priority. Recommendations derive from findings—never introduce unsupported suggestions.

### 6. Document Metadata Section

Capture sources examined, limitations affecting completeness, and method used. Metadata enables validation and reproducibility.

### 7. Validate Structure Compliance

Before finalizing, verify:

- [ ] Frontmatter complete with required fields
- [ ] Summary present and concise
- [ ] Findings organized in categorized sections
- [ ] All claims have source attribution
- [ ] Recommendations (if present) reference supporting findings
- [ ] Metadata section documents scope and limitations
- [ ] Body word count within the artifact-type budget (see Budgets) — counted before writing; issues carrying verbatim-lifted AC text are exempt per the Budgets carve-out
- [ ] Every section carries at least one finding, decision, or recommendation — no symmetry sections
- [ ] Summary answers the objective in ≤3 sentences

### 8. Read What You Made

Re-open the finished artifact as its consumer would—no context beyond the artifact itself—and check the one thing that matters most for the type: a report's summary section alone answers the objective; a work item lets an implementer start without reading anything else. When re-reading is impossible (artifact handed off mid-turn), re-check the composed draft against the section-7 checklist as the substitute. Every statistic or percentage in the artifact carries a citation or a measurement; at this validation read, unsourced numbers are removed, never silently kept—surviving uncertainty carries the Decision Point marker conventions instead.

## Decision Points

**Findings span multiple domains**: Create separate sections per domain. Do not force unrelated findings into single category.

**Attribution unclear**: Note uncertainty explicitly: `[source uncertain: likely from X based on context]`. Do not omit attribution.

**Values unknown**: Never invent progress, status, or numbers—unknown is a valid value. Mark `[unknown: <reason>]` at the point the value would have appeared; do not omit the item. This is the `[source uncertain: …]` convention's sibling: consumers can then distinguish "absent because unknown" from "absent because unchecked."

**Evidence unavailable** (index, daemon, or lint tooling degraded): claims that could not be verified carry `[unverified: <tool or source unavailable>]`; the Metadata section names the degraded condition. This is the `[unknown: …]` convention's sibling—unknown input vs unverifiable claim. For HTML/rich artifacts, the report still states what was checked and how; silent completeness is never the fallback. Markdown/plain-text artifacts are exempt (they degrade by nature).

**Conflicting findings**: Present both with conflict flag. Do not resolve prematurely—preserve for analysis phase.

**Findings exceed reasonable length**: Split into multiple focused reports. Reference related reports in metadata.

## Budgets

All values are body-word ceilings, not targets. An over-budget artifact splits into focused reports per the "Findings exceed reasonable length" Decision Point above — never silently overflows. The per-type target bands in the structure variants remain the drafting target; this table is the whole-artifact ceiling.

| Artifact type | §R1 starter | D-5 ceiling (2× starter) |
|---|---|---|
| Discovery report | 3,000 | 6,000 |
| Analysis report | 2,500 | 5,000 |
| Validation report | 1,500 | 3,000 |
| Summary report | 500 | 1,000 |
| Epic | 500 | 1,000 |
| Story | 350 | 700 |
| Issue | 200 | 400 |
| Task | 120 | 240 |

Exemption (Issue row only): acceptance criteria lifted verbatim from their governing brief count outside this budget — a verbatim-AC issue may exceed the 400-word ceiling without splitting, because reworded compression breaks contract fidelity; all other issue text still counts, and the exemption never applies to other artifact types.

## Success Criteria

- Report file created at correct path
- Frontmatter contains all required fields
- Summary provides accurate orientation
- Findings organized in logical categories
- Every claim has traceable attribution
- Structure matches type-specific requirements

At least one criterion must be a falsifying test — a condition whose failure is observable in the artifact. Example: a reader with no context can state the report's answer from the summary section alone; if the summary leaves the objective unresolved, the report fails regardless of structure.

## Common Issues

**Attribution drift**: Source locations change after reference. Re-verify if report creation spans time.

**Category proliferation**: Too many small categories obscure patterns. Target 3-7 major sections.

**Summary too detailed**: Keep to 3 sentences maximum—detail belongs in findings.

**Unsupported recommendations**: Every recommendation must link to specific evidence.

**Ornament sections**: The three padding classes—symmetry sections, decorative tables, background-without-decision-impact—exist to balance the outline, not to carry findings. If you cannot say what a section, table, or figure does for the reader's decision, delete it. (Extends the semantic-vs-invariant existence test from facts to artifact elements.)
