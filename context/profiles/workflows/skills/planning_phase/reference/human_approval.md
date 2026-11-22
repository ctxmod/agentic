# Human Approval Phase

Present recommendations or options to the developer for review and approval before proceeding with execution.

## When to Use

Use human approval phase when:

- Multiple valid approaches exist and choice affects outcomes
- Changes have significant impact requiring human judgment
- Automation benefits from human oversight
- Developer preferences should inform execution

Human approval adds a quality gate where human judgment improves outcomes.

## Main Agent Behavior

This phase executes directly within the main agent (no sub-agent launch).

**Process**:

1. **Read input report**: Consume recommendations from prior phase
2. **Present summary**: Display key options to developer
3. **Await input**: Pause for developer response
4. **Process decision**: Map response to approved action
5. **Generate approval**: Create approval record for subsequent phases

**Presentation format**:

```markdown
## Recommendations

### Recommended Approach

{Primary recommendation from report}

### Alternative Approaches

{Additional options if present}

### Associated Context

{Work items, files, or other relevant context}

Approve recommended approach? (y/n/number)
```

**Response handling**:

| Input | Action |
|-------|--------|
| "y" or "yes" | Approve recommended approach |
| "n" or "no" | Reject recommendations, await alternatives |
| Number (1, 2, 3) | Select specified alternative |
| Manual text | Capture custom specification |

**Exit conditions**:

- Developer approves or specifies approach: proceed to execution
- Developer rejects all options: abort workflow
- Developer requests alternatives: loop to presentation with new options

## Output Structure

Generate approval record:

```
APPROVED_APPROACH:
- Strategy: [recommended | alternative-N | manual]
- Details: [specific parameters or specifications]
- Rationale: [developer's reasoning if manual]
```

Pass this record to subsequent execution phases.

## Context Requirements

**Inputs required**:

| Input | Source | Purpose |
|-------|--------|---------|
| Recommendations | Prior phase report | Options to present |
| Work item context | If applicable | EPIC_PATH, STORY_PATH, ISSUE_PATH |

**Outputs produced**:

| Output | Destination | Purpose |
|--------|-------------|---------|
| APPROVED_APPROACH | Execution phase | Approved action to execute |

## Decision Types

| Type | Presentation | Examples |
|------|--------------|----------|
| Binary choice | Yes/no approval | Proceed with single recommendation? |
| Multiple choice | Numbered alternatives | Select strategy 1, 2, or 3? |
| Open specification | Free-form input | Specify custom commit message |
| Confirmation with changes | Modified recommendation | Auto-fixes applied. Re-confirm? |

## Approval Workflows

### Binary Approval

```
Present: Single recommendation
Await: y/n
Proceed: On approval, abort on rejection
```

### Multi-Option Approval

```
Present: 1-3 alternatives with trade-offs
Await: Number or y for recommended
Proceed: On selection, reject on "n"
```

### Iterative Approval

```
Present: Initial recommendation
Await: Feedback
Refine: Incorporate feedback
Present: Updated recommendation
Loop: Until approval or abort
```

## Transition Criteria

Human approval completes and transitions when:

- Developer provides clear response
- Response maps to executable action
- Approval record is generated
- Workflow direction is unambiguous

If developer aborts, terminate workflow cleanly with status report.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Presenting without context | Uninformed decisions | Include rationale and trade-offs |
| Forcing single option | False choice | Present genuine alternatives or skip approval |
| Ignoring feedback | Frustration, poor outcomes | Incorporate developer input |
| Over-approval for trivial changes | Workflow friction | Reserve for meaningful choices |
| Skipping documentation | Lost context | Record approval decision and rationale |

## When to Skip

Human approval is not required when:

- Only one valid approach exists
- Automation produces consistently correct results
- Developer explicitly requested automation without confirmation
- Speed is critical and choices are equivalent

Skip approval phase entirely rather than presenting a false choice.
