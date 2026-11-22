# Handoff Summary Schema

Handoff summaries enable seamless continuation by successors. They capture current state, pending items, relevant background, and implicit knowledge that might otherwise be lost during transitions. Handoff summaries minimize ramp-up time for anyone continuing the work.

When the requester states what the next session will focus on, tailor the handoff to that stated focus rather than writing a general-purpose summary.

## Base Schema

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_summary.md" />

## Frontmatter

```yaml
---
type: summary
variant: handoff
date: YYYY-MM-DD
time: HH-mm
source: "{context-path or 'general'}"
objective: "{objective-name}"
scope: "{handoff scope}"
stopping_point: "{brief description of where work stopped}"
prior_handoff: "{path to previous handoff if applicable}"
---
```

| Field | Required | Description |
|-------|----------|-------------|
| type | Yes | Always `summary` |
| variant | Yes | Always `handoff` |
| stopping_point | Yes | Brief description of current state |
| prior_handoff | Conditional | Previous handoff for continuity |

Additional fields from base summary schema apply.

## Audience

Handoff summaries target:

- Successors continuing work after session boundary
- Team members inheriting responsibilities
- Future agents resuming long-running tasks
- Reviewers needing historical context

Readers need sufficient context to continue without the original participant.

## Content Priority

| Priority | Include | Exclude |
|----------|---------|---------|
| High | Current state, pending items, blockers | Completed work (unless relevant) |
| Medium | Decisions made, assumptions, rationale | Exhaustive history |
| Low | Background context when needed | Activity logs |

Emphasize what successors need to continue effectively.

Do not duplicate content already captured in other artifacts — plans, reports, decision records, commits, diffs. Reference Materials is pointer-tables by structure; keep it that way: reference other artifacts by path, never copy their content into the handoff body.

## Redaction

No secrets, credentials, or PII in handoff bodies. Reference secret-store keys by path instead of embedding their values.

## Required Sections

### Handoff Summary

2-3 sentences stating scope, current state, and primary pending item:

```markdown
## Handoff Summary

{Brief description of work scope} after completing {what was accomplished}. {Current state description}. Primary pending: {most important next item}.
```

### Current State

Precise description of where work stopped:

```markdown
## Current State

**Working On**: {current task or item}

**Location**: {path, reference, or context}

**Status**: {completion state}

**Stopping Point**: {specific point where work paused}

**Last Action**: {what was just completed}

**Next Action**: {immediate next step}
```

### Pending Items

Prioritized list of items requiring attention:

```markdown
## Pending Items

### 1. {Item Title} (immediate)

- **Description**: {what needs to be done}
- **Location**: {relevant path or reference}
- **Context**: {background needed}
- **Estimated Effort**: {rough sizing}

### 2. {Item Title} (after #1)

- **Description**: {what needs to be done}
- **Dependencies**: {what must complete first}
```

Number items by priority. Note dependencies between items.

### Decisions Made

Key decisions with rationale for context:

```markdown
## Decisions Made

### {Decision Title}

- **Choice**: {what was decided}
- **Alternatives Considered**: {other options}
- **Rationale**: {why this choice}
- **Trade-off**: {what was sacrificed}
- **Reference**: {supporting documentation}
```

### Open Questions

Unresolved items needing investigation or input:

```markdown
## Open Questions

### {Question}

- **Context**: {why this question arose}
- **Impact**: {what depends on the answer}
- **Suggested Approach**: {how to investigate}
```

### Reference Materials

Pointers to reports, documentation, and locations:

```markdown
## Reference Materials

### Reports

| Report | Relevance |
|--------|-----------|
| {path} | {what it covers} |

### Key Locations

- {path:lines} - {what is there}
- {path} - {what is there}

### External Resources

- [{title}]({url}) - {relevance}
```

### Suggested Skills

Skill keys and slash-commands the successor should invoke, one line each:

```markdown
## Suggested Skills

- `review_pr_phase` — validate the changeset before handoff closes
- `/review_audit` — full-quality gate before the successor starts
```

## Optional Sections

### Assumptions

Implicit assumptions made during work:

```markdown
## Assumptions

- **{Assumption}**: {what was assumed and why}
```

### Workarounds

Temporary solutions in place:

```markdown
## Workarounds

### {Workaround Title}

- **Situation**: {what necessitated the workaround}
- **Solution**: {what was done}
- **Trigger**: {when proper fix should be applied}
- **Location**: {where workaround exists}
```

### Known Issues

Problems encountered that remain unresolved:

```markdown
## Known Issues

### {Issue Title}

- **Symptom**: {what happens}
- **Cause**: {if known}
- **Workaround**: {how to work around}
- **Suggested Fix**: {how to properly resolve}
```

### Context for Successor

Background information helpful for continuation:

```markdown
## Context for Successor

{Narrative context that does not fit structured sections}
```

## Example

```markdown
---
type: summary
variant: handoff
date: {date}
time: 18-00
source: general
objective: summary
scope: Configuration standardization implementation
stopping_point: Naming remediation complete, documentation in progress
prior_handoff: reports/{date}/18-00-summary-handoff.md
---

# Configuration Standardization Handoff

## Handoff Summary

Handed off configuration standardization after completing discovery, analysis, validation, and remediation phases. Documentation update in progress at 60%. Primary pending: Complete environment precedence documentation section.

## Current State

**Working On**: Documentation update

**Location**: `docs/configuration.md`

**Status**: 60% complete

**Stopping Point**: Completed sections on file structure and naming conventions. Environment precedence section started but incomplete.

**Last Action**: Added naming convention examples with before/after comparisons

**Next Action**: Document environment variable expansion order and override rules

## Pending Items

### 1. Complete Environment Precedence Section (immediate)

- **Description**: Document how environment-specific configuration overrides base configuration
- **Location**: `docs/configuration.md:145-200`
- **Context**: Loader implementation shows hierarchical merge; document expected behavior
- **Estimated Effort**: 1-2 hours

### 2. Add Validation to Pipeline (after documentation)

- **Description**: Configure automated naming validation
- **Location**: CI/CD configuration
- **Dependencies**: Infrastructure access (currently pending)
- **Estimated Effort**: 2-3 hours once access granted

### 3. Update Team Onboarding Materials (low priority)

- **Description**: Reference new configuration documentation in onboarding guide
- **Location**: `docs/onboarding.md`
- **Dependencies**: Documentation complete
- **Estimated Effort**: 30 minutes

## Decisions Made

### Naming Pattern Choice

- **Choice**: `{purpose}.{format}.{env}` pattern
- **Alternatives Considered**: `{env}/{purpose}.{format}`, `{purpose}-{env}.{format}`
- **Rationale**: Single directory simplifies tooling; format before env enables sorting
- **Trade-off**: Longer filenames
- **Reference**: `reports/{date}/{time_2}-analysis-config.md § Recommendations`

### Strict Variable Expansion

- **Choice**: Fail on undefined required variables
- **Alternatives Considered**: Silent empty string, warning only
- **Rationale**: Fail-fast prevents runtime surprises
- **Reference**: Design discussion in analysis report

## Open Questions

### Should Optional Variables Have Defaults?

- **Context**: Current implementation allows undefined optional variables
- **Impact**: Determines whether defaults file is needed
- **Suggested Approach**: Survey existing usage patterns; propose convention

## Reference Materials

### Reports

| Report | Relevance |
|--------|-----------|
| `reports/{date}/{time_1}-discovery-config.md` | File inventory and patterns |
| `reports/{date}/{time_2}-analysis-config.md` | Risk assessment and recommendations |
| `reports/{date}/{time_3}-remediation-naming.md` | Naming fixes applied |

### Key Locations

- `config/loader.ts:45-67` - Variable expansion logic
- `docs/configuration.md:145-200` - Section in progress
- `config/*.yaml` - Standardized configuration files

## Workarounds

### Manual Validation Until Pipeline Integration

- **Situation**: Automated validation blocked on infrastructure access
- **Solution**: Running validation script manually before commits
- **Trigger**: Remove once pipeline integration complete
- **Location**: `scripts/validate-config.sh`
```

## Length Guidelines

| Metric | Target |
|--------|--------|
| Word count | 500-900 words |
| Maximum | 1100 words |
| Format | Comprehensive for cold-start continuation |

## Validation Checklist

- [ ] Handoff summary states scope and primary pending item
- [ ] Current state includes specific stopping point and next action
- [ ] Pending items prioritized with dependencies noted
- [ ] Decisions include rationale and trade-offs
- [ ] Open questions include suggested approach
- [ ] References enable navigation to key materials
- [ ] Workarounds documented with removal triggers
- [ ] No secrets, credentials, or PII in the body; secret-store keys referenced by path only
- [ ] Suggested Skills lists the skill keys and slash-commands the successor should invoke
- [ ] No duplicated content — other artifacts referenced by path, not copied
- [ ] Total length under 1100 words
