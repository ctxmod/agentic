# Summary Production Procedure

Distill multiple reports into concise summaries preserving essential information.

## Prerequisites

- Source reports identified with paths
- Summary purpose defined (handoff, decision support, status update)
- Target length constraint established
- Audience identified

## Steps

### 1. Inventory Source Reports

List reports with metadata: path, type, date, key topic. Record total scope to calibrate compression.

### 2. Extract Key Findings

From each report, identify findings meeting inclusion criteria:

- Affects decisions or next steps
- Represents significant discovery
- Indicates risk or concern
- Contradicts prior understanding

Exclude routine confirmations, strategically irrelevant detail, and superseded findings. Mark each with source attribution.

### 3. Categorize and Prioritize

Group findings by priority:

| Priority | Category |
|----------|----------|
| 1 | Actions required |
| 2 | Key insights |
| 3 | Risks identified |
| 4 | Context updates |
| 5 | Confirmations |

### 4. Apply Compression

Reduce detail while preserving meaning:

- Remove redundant context
- Consolidate related findings
- Use abbreviated attribution
- Eliminate hedging language

**Example**: "Auth module uses 24-hour JWT tokens (`src/auth/`, `config/auth.json:15-28`)." compresses 42 words to 15.

### 5. Synthesize Cross-Report Patterns

Identify themes spanning multiple reports. Note which reports share the pattern and its significance. Synthesis adds value beyond concatenation.

### 6. Construct Summary Document

Structure varies by purpose:

**Handoff**: Status, key findings (3-5 items), outstanding items, source reports.

**Decision support**: Recommendation with confidence, supporting evidence, risks and alternatives, source reports.

### 7. Validate Compression Quality

- [ ] Length within target: source-word count ÷ purpose ratio (Compression Ratios table below), rounded to a whole number — hard ceiling 1,000 words
- [ ] Priority 1 findings present
- [ ] No unsourced claims
- [ ] Actionable items identified

## Decision Points

**Conflicting findings**: Present conflict explicitly with confidence levels. Do not resolve arbitrarily.

**Exceeds length target**: Increase compression or split into focused summaries.

**Audience unknown**: Default to decision-maker audience—emphasize actions and risks.

## Compression Ratios

| Purpose | Ratio |
|---------|-------|
| Quick status | 20:1 |
| Handoff | 10:1 |
| Decision support | 5:1 |

## Success Criteria

- Purpose reflected in structure
- Key findings preserved with attribution
- Cross-report patterns surfaced
- Reader can act without source reports

## Common Issues

**Over-compression**: Critical detail lost. Verify reader can take action from summary alone.

**Attribution lost**: Maintain source links even in compressed form.

**Pattern blindness**: Explicitly compare findings across reports during extraction.
