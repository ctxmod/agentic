# Execution Phase

Apply planned changes, create artifacts, or perform primary actions defined by prior phases. Execution transforms plans into concrete outcomes.

## When to Use

Use execution phase when:

- Plans are complete and actionable
- Required inputs are available
- Changes must be applied to artifacts
- New artifacts must be created
- Actions require sub-agent execution

Execution is the primary action phase. It follows planning and precedes validation.

## Variants

- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation/reference/implementation_phase_selection.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation/reference/implementation_creation.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation/reference/implementation_edit.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation/reference/implementation_general.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation/reference/implementation_parallel.md" />
- <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::implementation/reference/implementation_work_item.md" />

## Model Selection

**Primary**: Opus

Execution requires careful reasoning to apply changes correctly. Opus provides higher accuracy for complex modifications.

**Exception**: Use Sonnet for straightforward execution with clear instructions and low ambiguity.

## Coordination Pattern

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/sequential.md" />

Use sequential when actions have dependencies, order matters, or each step builds on previous.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/parallel.md" />

Use parallel when actions are independent, artifacts don't overlap, and concurrent execution is safe.

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/fan_out_fan_in.md" />

Use fan-out fan-in when independent work requires aggregation or unified verification.

## Context Requirements

**Parameters provided by main agent**:

| Parameter | Required | Purpose |
|-----------|----------|---------|
| Report (reserved) | Yes | Sub-agent creates the report record just-in-time with `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing |
| INPUT_REPORT | Yes | Planning report to execute |
| TARGET_PATHS | For modifications | Artifacts to modify |
| OUTPUT_PATH | For creation | Where to create new artifacts |
| Work item context | If applicable | EPIC_PATH, STORY_PATH, ISSUE_PATH |

## Transition Criteria

Execution completes and transitions when:

- Planned actions are applied
- Artifacts are in intended state
- Execution report documents outcomes
- No blocking issues remain

If execution fails partially, document completed actions and escalate remaining issues.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Execution without plan | Uncoordinated changes | Complete planning first |
| Unbounded scope | Uncontrolled modifications | Execute within defined scope |
| Ignoring failures | Compounding errors | Address failures before proceeding |
| Skipping documentation | Untraceable changes | Report all actions |
| Mixing concerns | Unclear responsibility | One objective type per execution |
