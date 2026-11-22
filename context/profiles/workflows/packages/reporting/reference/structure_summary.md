# Summary Report Schema

Summary reports synthesize multiple reports into consolidated overviews. They aggregate findings across reports, extract cross-cutting themes, and provide unified guidance. Summary answers "what's the overall picture?"—synthesizing individual reports into coherent wholes.

## Variants

Specialized schemas exist for different audiences and purposes:

| Variant | Audience | Schema |
|---------|----------|--------|
| Executive | Non-technical stakeholders | `summary/executive.md` |
| Progress | Project tracking | `summary/progress.md` |
| Technical | Developer audiences | `summary/technical.md` |
| Decision | Decision-makers | `summary/decision.md` |
| Handoff | Successors continuing work | `summary/handoff.md` |

Use this base schema for general-purpose summaries. Use variants when audience or purpose is specific.

## Frontmatter

```yaml
---
type: summary
date: YYYY-MM-DD
time: HH-mm
source: "{context-path or 'general'}"
objective: "{objective-name}"
scope: "{summary scope description}"
period: "{time period covered if applicable}"
---
```

| Field | Required | Description |
|-------|----------|-------------|
| type | Yes | Always `summary` |
| date | Yes | Report creation date |
| time | Yes | Report creation time |
| source | Yes | Work item context or "general" |
| objective | Yes | Objective that produced this report |
| scope | Yes | What this summary covers |
| period | Conditional | Time period when summarizing temporal work |

## Required Sections

### Summary

2-4 sentences providing orientation. State:

- Scope covered
- Number of source reports synthesized
- Primary conclusion or status

### Source Reports

Reports included in this summary:

```markdown
## Source Reports

| Report | Type | Date | Key Topic |
|--------|------|------|-----------|
| discovery-config.md | discovery | {date_1} | Configuration inventory |
| analysis-config.md | analysis | {date_2} | Configuration assessment |
| validation-naming.md | validation | {date_2} | Naming compliance |
```

### Key Themes

Cross-cutting patterns observed across reports:

```markdown
### {Theme Title}

{Pattern description spanning multiple sources}

- **Sources**: {list of reports where pattern appears}
- **Significance**: {why this matters}
```

### Action Items

Consolidated recommendations from source reports:

```markdown
### {Action Item}

- **Source**: {originating report}
- **Priority**: Critical | High | Medium | Low
- **Status**: Pending | In Progress | Blocked | Completed
```

## Optional Sections

### Status Overview

High-level metrics when tracking progress:

| Metric | Value |
|--------|-------|
| Completion | 75% |
| Open issues | 3 |
| Blockers | 1 |

### Timeline

Chronological view when temporal progression matters:

```markdown
## Timeline

| Date | Event | Report |
|------|-------|--------|
| {date_1} | Discovery completed | discovery-config.md |
| {date_2} | Analysis completed | analysis-config.md |
| {date_3} | Validation completed | validation-naming.md |
```

### Risk Summary

Aggregated risks requiring attention:

```markdown
### {Risk Title}

- **Sources**: {reports identifying this risk}
- **Severity**: High | Medium | Low
- **Mitigation**: {current mitigation status}
```

### Blockers

Items preventing progress:

```markdown
### {Blocker Title}

- **Affects**: {what is blocked}
- **Owner**: {responsible party}
- **Escalation**: {escalation status}
```

### Next Period Focus

Priorities for upcoming work based on synthesis.

## Synthesis Guidelines

| Activity | Description |
|----------|-------------|
| Deduplication | Merge identical findings across reports |
| Generalization | Abstract specific findings into patterns |
| Prioritization | Elevate critical items, suppress noise |
| Trend identification | Note patterns across time or domain |
| Contradiction handling | Document conflicting findings with context |

Avoid listing findings from each report sequentially. Transform inputs into integrated insights.

## Compression Ratios

| Purpose | Target Ratio |
|---------|--------------|
| Quick status | 20:1 |
| Standard handoff | 10:1 |
| Decision support | 5:1 |

Summaries should be shorter than the sum of their inputs.

## Example

```markdown
---
type: summary
date: {date}
time: {time}
source: general
objective: summary
scope: Configuration management assessment
period: {date_1} to {date_2}
---

# Configuration Assessment Summary

## Summary

Synthesized 3 reports covering configuration discovery, analysis, and validation. Primary finding: naming inconsistency creates maintenance risk. Recommend standardization before new configuration additions.

## Source Reports

| Report | Type | Date | Key Topic |
|--------|------|------|-----------|
| {time_1}-discovery-config.md | discovery | {date} | 12 config files inventoried |
| {time_2}-analysis-config.md | analysis | {date} | Risk assessment completed |
| {time_4}-validation-naming.md | validation | {date} | 75% compliance achieved |

## Key Themes

### Naming Convention Drift

Three naming patterns exist across configuration files, creating confusion and error risk.

- **Sources**: discovery-config.md, validation-naming.md, analysis-config.md
- **Significance**: Blocks safe modification; increases onboarding time

### Documentation Gaps

Environment handling and precedence rules undocumented.

- **Sources**: discovery-config.md, analysis-config.md
- **Significance**: Deployment behavior unpredictable

## Action Items

### Standardize Naming Convention

- **Source**: analysis-config.md
- **Priority**: High
- **Status**: Pending

### Document Environment Precedence

- **Source**: analysis-config.md
- **Priority**: Medium
- **Status**: Pending

### Remediate 3 Naming Violations

- **Source**: validation-naming.md
- **Priority**: High
- **Status**: Pending

## Status Overview

| Metric | Value |
|--------|-------|
| Discovery | Complete |
| Analysis | Complete |
| Validation | Complete |
| Remediation | Pending |
```

## Length Guidelines

| Source Count | Target Length |
|--------------|---------------|
| 2-3 reports | 300-500 words |
| 4-6 reports | 500-800 words |
| 7+ reports | 800-1200 words |

## Validation Checklist

- [ ] Frontmatter includes all required fields
- [ ] Summary states scope and source count
- [ ] Source reports listed with paths, types, and dates
- [ ] Key themes identify cross-report patterns
- [ ] Themes include source attribution
- [ ] Action items consolidated with priority and status
- [ ] Compression ratio appropriate to purpose
- [ ] Word count within guidelines
