# Plan Report Schema

Plan reports document an intended approach for achieving an objective. They record the selected strategy, execution steps, dependencies, risk mitigations, and success criteria. Plan reports answer "how will we proceed?" and "what could go wrong?"--building on analysis reports that answer "what does it mean?"

## Frontmatter

```yaml
---
type: plan
date: YYYY-MM-DD
time: HH-mm
source: "{context-path or 'general'}"
objective: "{objective-name}"
approach: "{selected approach description}"
confidence: "{planning confidence level}"
---
```

| Field | Required | Description |
|-------|----------|-------------|
| type | Yes | Always `plan` |
| date | Yes | Report creation date |
| time | Yes | Report creation time |
| source | Yes | Work item context or "general" |
| objective | Yes | Objective that produced this report |
| approach | Yes | Brief description of selected approach |
| confidence | Yes | Overall confidence: High, Moderate, or Low |

## Required Sections

### Summary

2-4 sentences providing orientation. State:

- Selected approach and rationale
- Number of execution steps or phases
- Primary risks or constraints

Stakeholders read summaries first--ensure standalone value.

### Input Sources

Reports and materials informing the plan:

```markdown
## Input Sources

| Source | Type | Date | Key Topic |
|--------|------|------|-----------|
| {time_1}-discovery-config.md | discovery | {date} | Configuration inventory |
| {time_2}-analysis-config.md | analysis | {date} | Configuration improvement recommendations |
```

Include paths, types, and dates for all consumed materials.

### Approach

Selected strategy with rationale:

```markdown
## Approach

{Statement of the chosen approach}

**Rationale**: {Why this approach was selected over alternatives}

**Source**: {Link to analysis recommendation or other justification}
```

Connect the approach to analysis findings or other evidence. When multiple alternatives existed, briefly state why this one was selected.

### Execution Plan

Ordered steps with dependencies and verification points:

```markdown
## Execution Plan

### Step 1: {Step Title}

- **Action**: {What to do}
- **Dependencies**: {Prerequisites or prior steps}
- **Output**: {Expected result}
- **Verification**: {How to confirm completion}

### Step 2: {Step Title}

- **Action**: {What to do}
- **Dependencies**: Step 1
- **Output**: {Expected result}
- **Verification**: {How to confirm completion}
```

Each step includes enough detail for execution without ambiguity. Group related activities into single steps when they share dependencies and verification criteria.

### Success Criteria

Measurable outcomes that confirm plan completion:

```markdown
## Success Criteria

- [ ] {Specific, verifiable condition}
- [ ] {Specific, verifiable condition}
- [ ] {Specific, verifiable condition}
```

Criteria must be binary--either met or not. Avoid subjective assessments like "improved quality" without measurable thresholds.

### Risk Mitigation

Identified risks with probability, impact, and response strategy:

```markdown
## Risk Mitigation

### {Risk Description}

- **Probability**: High | Medium | Low
- **Impact**: High | Medium | Low
- **Mitigation**: {Preventive action}
- **Contingency**: {Response if risk materializes}
```

Every identified risk requires both mitigation (preventive) and contingency (reactive) strategies.

## Optional Sections

### Alternatives Considered

When the approach was selected from multiple options:

| Alternative | Advantage | Disadvantage | Reason Rejected |
|-------------|-----------|--------------|-----------------|
| Option A | Speed | Limited flexibility | Constraint X prevents adoption |
| Option B | Thoroughness | Time cost | Schedule does not permit |

### Dependencies

External requirements or enabling conditions:

```markdown
## Dependencies

**Requires**: {Items that must exist before execution begins}

**Enables**: {Items that become possible after plan completion}
```

### Checkpoints

Intermediate verification points within the execution plan:

```markdown
### Checkpoint: {After Step N}

- **Verification**: {What to check}
- **Rollback trigger**: {Condition that halts progress}
- **Recovery action**: {How to proceed if checkpoint fails}
```

Include checkpoints when execution spans multiple phases or when early failure detection reduces wasted effort.

### Constraints

Boundaries limiting the solution space:

- Resource constraints (time, capacity, availability)
- Technical constraints (compatibility, performance, dependencies)
- Process constraints (approvals, sequencing, coordination)

### Next Steps

Immediate actions to begin execution. Distinct from the execution plan itself--these are preparatory activities.

## Confidence Levels

| Level | Definition | Application |
|-------|------------|-------------|
| High | Proven approach, clear dependencies, low risk | Similar work completed before, stable requirements |
| Moderate | Sound approach, some unknowns | New approach with understood components, moderate risk |
| Low | Experimental approach, significant unknowns | Novel territory, high uncertainty, many assumptions |

State confidence overall and per execution step when confidence varies. Document factors affecting certainty.

## Priority Levels

| Priority | Criteria |
|----------|----------|
| Critical | Blocks all progress, immediate action required |
| High | Significant impact, clear value, address soon |
| Medium | Moderate impact, worthwhile, can schedule |
| Low | Minor impact, address opportunistically |

Apply priority levels to execution steps, risk mitigations, and dependencies.

## Example

```markdown
---
type: plan
date: {date}
time: {time}
source: general
objective: planning
approach: Phased naming standardization with validation gates
confidence: High
---

# Configuration Standardization Plan

## Summary

Plan phased standardization of configuration naming across 12 files using the `.{purpose}.{format}` convention. Three execution phases with validation checkpoints between each. Primary risk: reference breakage during renames.

## Input Sources

| Source | Type | Date | Key Topic |
|--------|------|------|-----------|
| {time_1}-discovery-config.md | discovery | {date} | Configuration file inventory |
| {time_2}-analysis-config.md | analysis | {date} | Naming convention recommendations |

## Approach

Adopt phased rename strategy with validation gates between phases.

**Rationale**: Phased approach isolates failures to individual batches, enabling rollback without affecting completed phases.

**Source**: `{time_2}-analysis-config.md § Recommendations`

## Execution Plan

### Step 1: Prepare Reference Map

- **Action**: Document all references to each configuration file across the project
- **Dependencies**: None
- **Output**: Reference map listing each file and its dependents
- **Verification**: Every configuration file has a complete dependent list

### Step 2: Rename Core Configuration

- **Action**: Rename 4 core files to `{purpose}.{format}` pattern, update references
- **Dependencies**: Step 1 (reference map)
- **Output**: 4 renamed files with updated references
- **Verification**: All references resolve, no broken paths detected

### Step 3: Rename Environment Configuration

- **Action**: Rename 5 environment-specific files, update references
- **Dependencies**: Step 2 verified
- **Output**: 5 renamed files with updated references
- **Verification**: All references resolve, environment loading confirmed

### Step 4: Rename Auxiliary Configuration

- **Action**: Rename 3 auxiliary files, update references
- **Dependencies**: Step 3 verified
- **Output**: 3 renamed files with updated references
- **Verification**: Full validation pass across all 12 files

## Success Criteria

- [ ] All 12 configuration files follow `{purpose}.{format}` convention
- [ ] Zero broken references across the project
- [ ] Environment-specific loading verified in all contexts
- [ ] Naming validation passes with zero violations

## Risk Mitigation

### Reference Breakage During Rename

- **Probability**: Medium
- **Impact**: High
- **Mitigation**: Build complete reference map before any renames; validate after each phase
- **Contingency**: Revert phase using version control; fix reference map gaps before retry

### Environment Loading Failure

- **Probability**: Low
- **Impact**: High
- **Mitigation**: Verify loading behavior before and after each environment file rename
- **Contingency**: Revert environment file renames; investigate loading mechanism before retry

## Checkpoints

### Checkpoint: After Step 2

- **Verification**: All 4 core files renamed, references updated, validation passing
- **Rollback trigger**: Any broken reference detected
- **Recovery action**: Revert Step 2 renames, update reference map, retry

### Checkpoint: After Step 3

- **Verification**: All 9 files renamed so far, environment loading confirmed
- **Rollback trigger**: Environment loading failure in any context
- **Recovery action**: Revert Step 3 renames, investigate loading mechanism

## Dependencies

**Requires**: Configuration discovery report, analysis recommendation

**Enables**: Configuration module standardization, automated validation
```

### Research Literature Review Plan

```markdown
---
type: plan
date: {date}
time: {time}
source: general
objective: planning
approach: Systematic review with staged screening and thematic synthesis
confidence: Moderate
---

# Literature Review Plan

## Summary

Plan systematic review of 85 candidate sources across 4 databases, using three screening stages and thematic synthesis. Four execution steps with verification between screening rounds. Primary risk: inclusion criteria ambiguity causing inconsistent screening decisions.

## Input Sources

| Source | Type | Date | Key Topic |
|--------|------|------|-----------|
| {time_1}-discovery-sources.md | discovery | {date} | Source inventory across databases |
| {time_2}-analysis-sources.md | analysis | {date} | Relevance assessment and screening criteria |

## Approach

Apply staged screening (title, abstract, full-text) followed by thematic synthesis of included sources.

**Rationale**: Staged screening reduces effort by eliminating irrelevant sources early. Thematic synthesis accommodates heterogeneous source types better than statistical aggregation.

**Source**: `{time_2}-analysis-sources.md § Methodology Recommendations`

## Execution Plan

### Step 1: Finalize Inclusion Criteria

- **Action**: Define precise inclusion and exclusion criteria with worked examples
- **Dependencies**: None
- **Output**: Criteria document with boundary examples for ambiguous cases
- **Verification**: Two independent reviewers achieve >90% agreement on 10 test sources

### Step 2: Title and Abstract Screening

- **Action**: Screen 85 sources by title and abstract against inclusion criteria
- **Dependencies**: Step 1 (criteria document)
- **Output**: Shortlist of ~30 sources passing initial screen, with exclusion rationale for removed sources
- **Verification**: Exclusion rationale documented for every removed source

### Step 3: Full-Text Review

- **Action**: Review full text of shortlisted sources, extract key findings
- **Dependencies**: Step 2 verified
- **Output**: Extraction table with findings, methods, and quality assessment per source
- **Verification**: All shortlisted sources reviewed, extraction table complete

### Step 4: Thematic Synthesis

- **Action**: Group findings into themes, identify patterns and gaps
- **Dependencies**: Step 3 verified
- **Output**: Synthesis document with themes, supporting evidence, and identified gaps
- **Verification**: Every included source represented in at least one theme

## Success Criteria

- [ ] All 85 candidate sources screened with documented decisions
- [ ] Inclusion criteria applied consistently (>90% inter-reviewer agreement)
- [ ] Extraction table complete for all included sources
- [ ] Thematic synthesis covers all included sources with no orphaned findings

## Risk Mitigation

### Inclusion Criteria Ambiguity

- **Probability**: Medium
- **Impact**: High
- **Mitigation**: Test criteria on sample sources before full screening; document boundary cases
- **Contingency**: Re-screen disputed sources with revised criteria if agreement drops below threshold
```

## Length Guidelines

| Scope | Target Length |
|-------|---------------|
| Focused (single approach) | 500-800 words |
| Standard (domain planning) | 800-1500 words |
| Comprehensive (multi-domain) | 1500-2500 words |

## Validation Checklist

- [ ] Frontmatter includes all required fields with approach and confidence
- [ ] Summary provides standalone value in 2-4 sentences
- [ ] Input sources listed with paths, types, and dates
- [ ] Approach states selection rationale with evidence link
- [ ] Execution steps include action, dependencies, output, and verification
- [ ] Success criteria are specific and binary
- [ ] Risk mitigations include both preventive and contingency strategies
- [ ] Confidence level documented with supporting factors
- [ ] Word count within guidelines
