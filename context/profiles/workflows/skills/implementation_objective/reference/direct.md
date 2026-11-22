# Direct Implementation Objective

Execute explicit instructions provided by the orchestrator. Direct objectives handle cases where specialized implementation objectives (edit, refactor, create, delete, rename) do not apply or where the orchestrator provides complete guidance.

## Base Behavior

the `implementation_objective` skill

## Scope

Direct implementation applies when:

- Instructions are explicit and complete
- No specialized objective provides better framing
- Mixed operations span multiple objective types
- Orchestrator guidance supersedes standard patterns
- Unique requirements do not fit established categories

Direct objectives serve as fallback when specialized objectives are inappropriate.

## Core Behavior

1. **Receive**: Accept explicit instructions from orchestrator
2. **Parse**: Identify discrete actions within instructions
3. **Validate**: Confirm instructions are actionable and unambiguous
4. **Execute**: Perform specified actions in order
5. **Verify**: Confirm actions achieved intended outcomes
6. **Report**: Document actions taken and results

## Relationship to Other Implementation Types

| Type | When to Use |
|------|-------------|
| Edit | Targeted content modifications |
| Refactor | Structural reorganization |
| Create | New artifact generation |
| Delete | Artifact removal |
| Rename | Identifier changes |
| **Direct** | **None of the above; explicit instructions** |

Direct objectives are used when:
- Instructions combine multiple operation types
- Standard objectives add unnecessary constraints
- Orchestrator has specific procedural requirements
- Novel requirements lack established patterns

## Principles

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/kiss.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`

KISS ensures execution focuses on specified actions without embellishment. Context efficiency keeps implementation aligned with orchestrator intent.

## Pre-Execution

Before acting:

1. Parse instructions into discrete, actionable steps
2. Identify any ambiguities requiring clarification
3. Confirm tool access for required operations
4. Verify scope boundaries are clear
5. Acknowledge parameters provided by orchestrator

## During Execution

Apply instructions with these guidelines:

- **Literal interpretation**: Follow instructions as stated
- **No assumptions**: Clarify ambiguities rather than guessing
- **Sequential ordering**: Respect instruction sequence unless parallelism is explicit
- **Scope adherence**: Act only within specified boundaries
- **No embellishment**: Avoid adding steps or enhancements not requested

## Verification

After executing:

1. Confirm each instruction was completed
2. Verify outcomes match orchestrator expectations
3. Check for unintended side effects
4. Validate artifacts remain well-formed
5. Document any deviations with rationale

## Tool Constraints

**Allowed**: As specified in instructions. When unspecified:
- Read: Examine context
- Edit: Modify existing artifacts
- Write: Create new artifacts
- Bash: Execute specified commands
- Glob/Grep: Search when needed

**Forbidden**:
- Actions outside specified scope
- Launching sub-agents (unless explicitly instructed)
- Modifying unrelated artifacts
- Adding unrequested functionality

## Response Format

After completing execution, return a brief response:

```
Completed direct implementation.

Actions taken:
- [Action 1 description]
- [Action 2 description]

Results: [Brief outcome summary]
```

Include any issues encountered or deviations from instructions.

## When to Use Specialized Objectives Instead

Prefer specialized objectives when:

| Condition | Use Instead |
|-----------|-------------|
| Single file content change | Edit |
| Structural reorganization | Refactor |
| New artifact needed | Create |
| Artifact removal | Delete |
| Identifier change | Rename |

Direct objectives are appropriate when:
- Multiple objective types combine in one task
- Instructions are procedural rather than outcome-focused
- Novel requirements lack established patterns
- Orchestrator provides complete action specification

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Interpreting vague instructions | Risk misalignment | Request clarification |
| Adding unrequested improvements | Scope creep | Execute as specified |
| Skipping verification | Unconfirmed outcomes | Validate all actions |
| Ignoring instruction order | May break dependencies | Respect sequence |
| Using when specialized applies | Less guidance | Use appropriate objective |
| Silent failures | Hidden problems | Report issues encountered |
