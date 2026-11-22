# Technical Summary Schema

Technical summaries preserve implementation context for practitioner audiences. They consolidate findings while maintaining precision necessary for decisions, reviews, and documentation. Technical summaries prioritize accuracy and completeness over accessibility.

## Base Schema

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_summary.md" />

## Frontmatter

```yaml
---
type: summary
variant: technical
date: YYYY-MM-DD
time: HH-mm
source: "{context-path or 'general'}"
objective: "{objective-name}"
scope: "{technical scope}"
domain: "{technical domain or component}"
---
```

| Field | Required | Description |
|-------|----------|-------------|
| type | Yes | Always `summary` |
| variant | Yes | Always `technical` |
| domain | Yes | Technical domain or component covered |

Additional fields from base summary schema apply.

## Audience

Technical summaries target:

- Practitioners inheriting or reviewing work
- Technical leads making decisions
- Contributors needing implementation context
- Reviewers requiring background

Readers expect precision, completeness, and actionable technical detail.

## Content Priority

| Priority | Include | Exclude |
|----------|---------|---------|
| High | Decisions, patterns, dependencies | Business justification |
| Medium | Performance, quality, verification | Stakeholder context |
| Low | Background when needed | Non-technical implications |

Emphasize "how" and "why" at the implementation level.

## Required Sections

### Technical Summary

2-4 sentences stating scope, primary findings, and implications:

```markdown
## Technical Summary

Analyzed {scope} comprising {size metrics}. Identified {key finding} following {pattern or approach}. Primary concern: {technical concern with location}.
```

### Architecture Overview

Key components, boundaries, and decisions:

```markdown
## Architecture Overview

### Components

| Component | Purpose | Location |
|-----------|---------|----------|
| {name} | {role} | {path or reference} |

### Design Decisions

- **{Decision}**: {approach chosen} because {rationale}
```

### Findings by Category

Organized findings with references and severity:

```markdown
## Findings

### {Category}

#### {Finding Title}

- **Location**: {path:lines or reference}
- **Severity**: Critical | High | Medium | Low | Info
- **Description**: {technical description}
- **Impact**: {technical consequence}
- **Resolution**: {suggested fix or approach}
```

### Dependencies

External and internal dependencies:

```markdown
## Dependencies

### External

| Dependency | Version | Purpose | Constraint |
|------------|---------|---------|------------|
| {name} | {version} | {why needed} | {version constraint} |

### Internal

| Component | Depends On | Relationship |
|-----------|------------|--------------|
| {component} | {dependency} | {how related} |
```

### Recommendations

Prioritized technical actions:

```markdown
## Recommendations

### {Recommendation Title}

- **Action**: {specific technical action}
- **Rationale**: {link to supporting findings}
- **Priority**: Critical | High | Medium | Low
- **Complexity**: High | Medium | Low
```

### References

Source reports, documentation, and locations:

```markdown
## References

### Source Reports

| Report | Type | Key Content |
|--------|------|-------------|
| {path} | {type} | {what it covers} |

### Locations

- {path:lines} - {description}
- {path:lines} - {description}
```

## Optional Sections

### Patterns Identified

Recurring structures observed:

```markdown
## Patterns

### {Pattern Name}

**Occurrences**: {locations where pattern appears}

**Description**: {what the pattern is}

**Assessment**: {whether pattern is appropriate}
```

### Quality Metrics

Measurements and assessments:

```markdown
## Quality Metrics

| Metric | Value | Threshold | Status |
|--------|-------|-----------|--------|
| {metric} | {value} | {target} | {pass/fail} |
```

### Security Considerations

Security-relevant findings:

```markdown
## Security

### {Finding Title}

- **Location**: {path:lines}
- **Risk**: {potential security impact}
- **Mitigation**: {current or recommended mitigation}
```

## Example

```markdown
---
type: summary
variant: technical
date: {date}
time: {time}
source: general
objective: summary
scope: Configuration module technical assessment
domain: config
---

# Configuration Module Technical Summary

## Technical Summary

Analyzed configuration module comprising 12 files and 850 lines. Identified YAML-based configuration following hierarchical override pattern. Primary concern: environment variable expansion logic in `config/loader.ts:45-67` lacks error handling for undefined variables.

## Architecture Overview

### Components

| Component | Purpose | Location |
|-----------|---------|----------|
| ConfigLoader | Load and merge configuration | `config/loader.ts` |
| ConfigValidator | Validate against schema | `config/validator.ts` |
| ConfigSchema | Define valid structure | `config/schema.ts` |

### Design Decisions

- **YAML over JSON**: Chosen for comment support and readability
- **Hierarchical override**: Base config extended by environment-specific files

## Findings

### Configuration Loading

#### Missing Error Handling in Variable Expansion

- **Location**: `config/loader.ts:45-67`
- **Severity**: High
- **Description**: Environment variable expansion silently returns empty string for undefined variables
- **Impact**: Configuration may contain invalid empty values causing runtime failures
- **Resolution**: Add validation step after expansion; throw on undefined required variables

#### Inconsistent Naming Convention

- **Location**: `config/*.yaml`
- **Severity**: Medium
- **Description**: Three naming patterns exist across 12 files
- **Impact**: Increased maintenance burden; confusion during modifications
- **Resolution**: Standardize on `{purpose}.{format}.{env}` pattern

## Dependencies

### External

| Dependency | Version | Purpose | Constraint |
|------------|---------|---------|------------|
| yaml | ^2.3.0 | YAML parsing | Requires 2.x for anchor support |
| ajv | ^8.12.0 | Schema validation | 8.x required for draft-2020 |

### Internal

| Component | Depends On | Relationship |
|-----------|------------|--------------|
| ConfigLoader | ConfigValidator | Validates after load |
| ConfigValidator | ConfigSchema | Uses schema for validation |

## Recommendations

### Add Variable Expansion Validation

- **Action**: Implement undefined variable detection with configurable strictness
- **Rationale**: Addresses missing error handling finding
- **Priority**: High
- **Complexity**: Low

### Standardize Naming Convention

- **Action**: Rename files to `{purpose}.{format}.{env}` pattern
- **Rationale**: Addresses inconsistent naming finding
- **Priority**: Medium
- **Complexity**: Low

## References

### Source Reports

| Report | Type | Key Content |
|--------|------|-------------|
| `{time_1}-discovery-config.md` | discovery | File inventory |
| `{time_2}-analysis-config.md` | analysis | Risk assessment |
| `{time_4}-validation-naming.md` | validation | Naming compliance |

### Locations

- `config/loader.ts:45-67` - Variable expansion logic
- `config/schema.ts:1-120` - Configuration schema definition
- `config/*.yaml` - Configuration files
```

## Length Guidelines

| Metric | Target |
|--------|--------|
| Word count | 400-800 words |
| Maximum | 1000 words |
| Format | Comprehensive for independent work |

## Validation Checklist

- [ ] Technical summary states scope with size metrics
- [ ] Architecture section documents components and decisions
- [ ] Findings include location with line ranges
- [ ] Dependencies documented with version constraints
- [ ] Recommendations link to specific findings
- [ ] References enable navigation to sources
- [ ] Terminology precise and accurate
- [ ] Total length under 1000 words
