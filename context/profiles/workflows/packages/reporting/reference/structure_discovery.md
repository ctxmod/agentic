# Discovery Report Schema

Discovery reports document findings from systematic exploration. They catalog what exists, where it resides, and how elements relate. Discovery answers "what?" without interpreting significance—interpretation belongs to analysis reports.

## Frontmatter

```yaml
---
type: discovery
date: YYYY-MM-DD
time: HH-mm
source: "{context-path or 'general'}"
objective: "{objective-name}"
scope: "{brief scope description}"
---
```

| Field | Required | Description |
|-------|----------|-------------|
| type | Yes | Always `discovery` |
| date | Yes | Report creation date |
| time | Yes | Report creation time |
| source | Yes | Work item context or "general" |
| objective | Yes | Objective that produced this report |
| scope | Yes | Brief description of exploration scope |

## Required Sections

### Summary

1-3 sentences providing orientation. State:

- Domain or area explored
- Number of finding categories
- Notable observations

Avoid detail—detail belongs in findings sections.

### Findings

Primary content organized by category. Each category as H2, individual findings as H3.

**Finding structure**:

```markdown
### {Finding Title}

{Description of what was found}

- **Location**: {path, reference, or identifier}
- **Related**: {connections to other findings}
- **Metrics**: {counts, measurements when applicable}
```

Group related findings under category headings. Target 3-7 major categories.

### Gaps

Areas requiring further investigation. Document:

- What could not be determined
- Why the gap exists
- Suggested approach to address

### Metadata

Sources examined and method used:

- **Sources examined**: List with locations
- **Method**: Approach taken for exploration
- **Limitations**: Constraints affecting completeness

## Optional Sections

### Notable Items

Specific findings warranting immediate attention. Use when particular findings require escalation or urgency.

### Patterns Detected

Recurring structures observed across findings. Include:

- Pattern description
- Occurrence locations
- Significance for downstream work

### Recommendations

Suggested next steps when findings imply actions. Each recommendation links to supporting findings.

## Attribution Format

Every claim requires source attribution:

| Source Type | Format |
|-------------|--------|
| File with location | `path/to/file.ext:15-42` |
| File without location | `path/to/file.ext` |
| Section reference | `document.md § Section Name` |
| External source | `[Title](URL)` |
| Observation | `[observation: context]` |

Position attribution inline or immediately following the claim.

## Example

```markdown
---
type: discovery
date: {date}
time: {time}
source: general
objective: discovery
scope: Configuration file inventory
---

# Configuration Discovery Report

## Summary

Explored configuration across three directories. Identified 12 configuration files in 4 format categories. Notable finding: environment-specific overrides lack consistent naming.

## Findings

### Format Categories

#### YAML Configuration

Found 5 YAML files providing structured settings.

- **Location**: `config/*.yaml`
- **Related**: Referenced by initialization routines
- **Metrics**: 5 files, 342 total lines

#### Environment Variables

Found 2 environment template files.

- **Location**: `.env.example`, `.env.template`
- **Related**: Deployment documentation
- **Metrics**: 45 unique variables defined

### Naming Patterns

Configuration files follow `{purpose}.{env}.{format}` pattern inconsistently. 3 of 12 files deviate from convention.

- **Location**: `config/` directory
- **Related**: Gaps section—naming standardization

## Gaps

Environment-specific override behavior undocumented. Requires review of initialization logic to determine precedence rules.

## Metadata

- **Sources examined**: `config/`, `deployment/`, project root
- **Method**: Recursive file pattern matching, content analysis
- **Limitations**: Runtime-generated configuration not captured
```

## Length Guidelines

| Scope | Target Length |
|-------|---------------|
| Narrow (single domain) | 300-500 words |
| Standard (category) | 500-1000 words |
| Broad (cross-domain) | 1000-1500 words |

## Validation Checklist

- [ ] Frontmatter includes all required fields
- [ ] Summary provides orientation in 1-3 sentences
- [ ] Findings organized in 3-7 categories
- [ ] Every claim has source attribution
- [ ] Gaps section documents unknowns
- [ ] Metadata section records scope and method
- [ ] Word count within guidelines
