# Decision Summary Schema

Decision summaries present options with trade-offs for informed decision-making. They structure information to enable stakeholders to evaluate alternatives, understand implications, and make informed choices. Decision summaries enable choice without advocating prematurely.

## Base Schema

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::reporting/reference/structure_summary.md" />

## Frontmatter

```yaml
---
type: summary
variant: decision
date: YYYY-MM-DD
time: HH-mm
source: "{context-path or 'general'}"
objective: "{objective-name}"
scope: "{decision scope}"
decision: "{brief statement of decision needed}"
deadline: "{when decision is required}"
---
```

| Field | Required | Description |
|-------|----------|-------------|
| type | Yes | Always `summary` |
| variant | Yes | Always `decision` |
| decision | Yes | Brief statement of what must be decided |
| deadline | Conditional | When decision is needed (if time-sensitive) |

Additional fields from base summary schema apply.

## Audience

Decision summaries target:

- Decision-makers evaluating alternatives
- Technical leads choosing approaches
- Stakeholders providing input on direction
- Teams aligning on strategy

Readers expect clear options, objective trade-offs, and sufficient context to decide.

## Content Priority

| Priority | Include | Exclude |
|----------|---------|---------|
| High | Options, trade-offs, recommendations | Implementation details |
| Medium | Constraints, risks, reversibility | Historical context |
| Low | Background when needed | Advocacy without evidence |

Emphasize enabling informed choice over advocating specific outcomes.

## Required Sections

### Decision Context

2-3 sentences stating what must be decided:

```markdown
## Decision Context

**Decision Needed**: {statement of what must be decided}

**Background**: {why this decision is necessary}

**Timeline**: {when decision is needed and why}

**Constraints**: {factors limiting options}
```

### Options

Each option with description, advantages, and disadvantages:

```markdown
## Options

### Option A: {Name}

{Description of what this option entails}

**Advantages**:
- {Benefit with evidence}
- {Benefit with evidence}

**Disadvantages**:
- {Cost or risk with evidence}
- {Cost or risk with evidence}

**Reversibility**: High | Medium | Low
```

Minimum two options. Include "do nothing" or "defer" when viable.

### Comparison Matrix

Table comparing options across key dimensions:

```markdown
## Comparison

| Dimension | Option A | Option B | Option C |
|-----------|----------|----------|----------|
| Cost | {assessment} | {assessment} | {assessment} |
| Timeline | {assessment} | {assessment} | {assessment} |
| Risk | {assessment} | {assessment} | {assessment} |
| Quality | {assessment} | {assessment} | {assessment} |
| Flexibility | {assessment} | {assessment} | {assessment} |
```

Use consistent assessment scales: High/Medium/Low, numeric values, or relative rankings.

### Risks

Risk assessment for each option:

```markdown
## Risks

### Option A Risks

- **{Risk}**: {likelihood and impact}

### Option B Risks

- **{Risk}**: {likelihood and impact}
```

### Recommendation

Suggested option with rationale (when appropriate):

```markdown
## Recommendation

**Recommended**: Option {X}

**Rationale**: {evidence-based reasoning}

**Confidence**: High | Moderate | Low

**Dissenting View**: {alternative perspective if applicable}
```

Include recommendation only when evidence supports clear preference. Omit or state "No clear recommendation" when options are equivalent.

## Optional Sections

### Stakeholder Input

Perspectives gathered from stakeholders:

```markdown
## Stakeholder Input

| Stakeholder | Preference | Rationale |
|-------------|------------|-----------|
| {role} | Option {X} | {their reasoning} |
```

### Sensitivity Analysis

How recommendation changes under different assumptions:

```markdown
## Sensitivity

| If | Then |
|----|------|
| {assumption changes} | {Option Y becomes preferable because...} |
```

### Decision Criteria

Explicit criteria for evaluation:

```markdown
## Decision Criteria

| Criterion | Weight | Description |
|-----------|--------|-------------|
| {criterion} | {priority} | {what makes an option better on this dimension} |
```

## Trade-off Dimensions

Common dimensions for comparison:

| Dimension | Measures |
|-----------|----------|
| Cost | Resource expenditure (time, budget, effort) |
| Quality | Reliability, maintainability, correctness |
| Risk | Likelihood and impact of negative outcomes |
| Timeline | Speed of delivery or implementation |
| Flexibility | Adaptability to future changes |
| Complexity | Effort to implement and maintain |
| Reversibility | Difficulty of changing course later |

Select dimensions relevant to the specific decision.

## Example

```markdown
---
type: summary
variant: decision
date: {date}
time: 16-00
source: general
objective: summary
scope: Configuration storage approach
decision: Select storage mechanism for configuration data
deadline: 2026-05-20
---

# Configuration Storage Decision

## Decision Context

**Decision Needed**: Select storage mechanism for application configuration.

**Background**: Current file-based approach lacks versioning and audit capabilities. Growth requires more robust solution.

**Timeline**: Decision needed by 2026-05-20 to meet sprint commitment.

**Constraints**: Must support rollback within 5 minutes; budget limited to existing infrastructure.

## Options

### Option A: File-Based with Version Control

Continue file-based storage with version control integration.

**Advantages**:
- No new infrastructure required
- Team already familiar with approach
- Built-in diff and history

**Disadvantages**:
- Manual deployment process
- No real-time updates
- Limited access control granularity

**Reversibility**: High

### Option B: Dedicated Configuration Service

Deploy specialized configuration management service.

**Advantages**:
- Real-time updates without deployment
- Fine-grained access control
- Built-in audit logging

**Disadvantages**:
- Additional infrastructure to maintain
- Learning curve for team
- Potential single point of failure

**Reversibility**: Medium

### Option C: Database Storage

Store configuration in existing database.

**Advantages**:
- Leverages existing infrastructure
- Transactional updates
- Familiar query patterns

**Disadvantages**:
- Schema changes for configuration changes
- Performance overhead for frequent reads
- Configuration mixed with application data

**Reversibility**: Medium

## Comparison

| Dimension | Option A | Option B | Option C |
|-----------|----------|----------|----------|
| Cost | Low | Medium | Low |
| Timeline | Immediate | 2 weeks | 1 week |
| Risk | Low | Medium | Low |
| Flexibility | Medium | High | Medium |
| Complexity | Low | High | Medium |

## Risks

### Option A Risks

- **Deployment coupling**: Configuration changes require full deployment cycle

### Option B Risks

- **Service availability**: New service becomes critical dependency
- **Operational overhead**: Team must maintain additional component

### Option C Risks

- **Schema rigidity**: Configuration structure changes require migrations

## Recommendation

**Recommended**: Option A (File-Based with Version Control)

**Rationale**: Meets rollback requirement, uses existing infrastructure, lowest risk. Enhanced automation can address deployment process concern.

**Confidence**: Moderate

**Dissenting View**: Option B provides superior flexibility if real-time updates become necessary.
```

## Length Guidelines

| Metric | Target |
|--------|--------|
| Word count | 400-700 words |
| Maximum | 900 words |
| Format | Structured for quick option comparison |

## Validation Checklist

- [ ] Decision context clearly states what must be decided
- [ ] At least two viable options presented
- [ ] Each option includes advantages, disadvantages, and reversibility
- [ ] Comparison matrix uses consistent dimensions
- [ ] Risks documented per option
- [ ] Recommendation includes rationale and confidence (if provided)
- [ ] No hidden advocacy in option descriptions
- [ ] Total length under 900 words
