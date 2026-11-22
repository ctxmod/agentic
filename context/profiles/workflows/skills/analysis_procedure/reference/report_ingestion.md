# Report Ingestion Procedure

Consume multiple discovery or research reports and extract findings into structured representations with full source traceability.

## Prerequisites

- Report file paths identified (discovery reports, research summaries, analysis outputs)
- Extraction schema defined (what finding types to capture)
- Output format specified for extracted findings

## Steps

### 1. Inventory Reports

List all report files with their metadata:

```
report_path: context.local/work/reports/2026-02/03-1056-qwrtzpsdfg-codebase-scan.md
report_type: discovery
sections: 4
line_count: 156
```

Record total report count and aggregate line counts to estimate processing scope.

### 2. Chunk Reports for Processing

Apply hybrid chunking based on report structure:

- **Section-based**: Split at H2 headers when sections exceed 200 lines
- **Semantic**: Keep related findings together within chunks
- **Size-limited**: Cap chunks at 500 lines to maintain context window efficiency

Preserve section headers in each chunk for attribution.

### 3. Extract Findings with Attribution

Process each chunk and extract findings. Attach source metadata to every finding:

```yaml
finding:
  content: "Auth module uses JWT with 24-hour expiration"
  source:
    report: "context.local/work/reports/2026-02/03-1056-hgfdsqwrtz-auth-analysis.md"
    section: "## Token Configuration"
    lines: "45-52"
  category: "security-config"
  confidence: "high"
```

Tag findings by type: factual observation, pattern identification, risk indicator, recommendation.

### 4. Cross-Reference Findings

Compare findings across reports. Identify:

- **Corroborated findings**: Same observation in multiple reports
- **Contradictions**: Conflicting information requiring resolution
- **Gaps**: Expected findings absent from reports

Link related findings using shared identifiers.

### 5. Validate Extraction Quality

Spot-check extracted findings against source reports. Verify:

- Attribution points to correct location
- Finding content accurately represents source
- No findings orphaned from sources

Sample 10% of findings minimum for validation.

## Decision Points

**Large report set (>10 reports)**: Prioritize by relevance to current objective. Process high-priority reports first.

**Overlapping content across reports**: Prefer findings from more recent or more authoritative sources. Note duplicates but avoid redundant extraction.

**Unclear finding boundaries**: When content spans sections, attribute to the section containing the core claim. Use line ranges to capture full context.

**Conflicting findings**: Extract both with conflict flag. Do not resolve during extraction—preserve for analysis phase.

## Success Criteria

- All reports processed without truncation
- Every finding has complete source attribution (report, section, lines)
- Findings categorized by type
- Cross-references established between related findings
- Validation sample passes accuracy check (>95% correct attribution)

## Common Issues

**Missing section headers**: Fall back to line-number-only attribution. Note structural limitation in extraction metadata.

**Report format inconsistency**: Normalize during extraction. Convert varied formats to standard finding structure.

**Duplicate findings**: Use content hashing to detect near-duplicates. Keep earliest occurrence, link duplicates to primary.

**Attribution drift**: Re-verify line numbers if reports may have changed since initial read. Timestamp extractions for staleness detection.
