# Issue Writing Objective

Create an issue that defines a specific work unit. Issues represent atomic pieces of work that can be assigned, tracked, and completed independently. Each issue describes the problem or change, establishes completion criteria, and provides implementation context. Produce the issue document at the designated ISSUE_PATH.

## Base Behavior

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::items/reference/work_items.md" />

## Schema

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::items/reference/work_structure_issue.md" />

The schema defines record fields, required sections, optional sections, issue type classification, status lifecycle, path conventions, and validation checklist. This objective defines the behavioral process for creating issues.

## Scope

Issues represent trackable work units:

- Bugs requiring correction
- Enhancements adding capability
- Refactoring improving structure
- Documentation additions or corrections
- Configuration changes
- Research questions requiring investigation
- Review or audit activities

Issues may decompose into tasks for granular tracking.

## Principles

`ctx:github.com/ctxmod/engineering/context::shared::design@principles/reference/ssot.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`
`ctx:github.com/ctxmod/writing_context/context::shared::density@core/rules/point_cite_never_paste.md`

SSOT ensures issues are the authoritative source for work unit scope—tasks reference issues rather than duplicating context. Context efficiency keeps issue content focused on what enables work to begin. Reference over duplication encourages linking to parent stories rather than restating acceptance criteria.

## Core Behavior

1. **Understand**: Clarify what work this issue represents
2. **Classify**: Determine issue type (bug, enhancement, research, etc.)
3. **Define**: Establish completion criteria that verify resolution
4. **Contextualize**: Link to parent story and related issues
5. **Provide**: Include information needed to begin work
6. **Write**: Produce issue document at ISSUE_PATH

## Type-Specific Guidance

### Bug Issues

Focus on observable behavior:

- Describe current versus expected behavior clearly
- Include reproduction steps when applicable
- State impact and severity
- Avoid prescribing solutions in the issue

### Enhancement Issues

Focus on capability gaps:

- Describe what exists now versus what should exist
- Articulate why the enhancement matters
- Define boundaries of the change
- Leave implementation approach open

### Research Issues

Focus on the question:

- State what needs investigation
- Define expected deliverable format
- Set boundaries on investigation scope
- Specify decision criteria if applicable

## Pre-Writing

Before drafting:

1. Review parent story for acceptance criteria this issue supports
2. Identify related issues to understand dependencies
3. Gather reproduction steps or requirements
4. Confirm ISSUE_PATH parameter is available

`ctx:github.com/ctxmod/agentic/context::shared::discovery_procedure@workflows/reference/broad_scan.md`

## During Writing

`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/clarity.md`
`ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/anti_patterns.md`

While authoring:

- Verify completion criteria are testable
- Include paths to relevant artifacts
- Check for unstated assumptions
- Use domain-appropriate terminology
- Avoid prescribing implementation approach

## Document Creation

Brief template for a procedure-depth agent (launched from objective depth):

```text
Create and organize the issue document following the report creation and organization procedures.
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/creation.md`
`ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/organization.md`
```

Issue documents follow report creation patterns. Issues reference their parent story. Issue completion requires all completion criteria met and all tasks (if any) resolved.

## Tool Constraints

**Allowed**:
- Read: Story, related issues, relevant artifacts, schema
- Glob: Find related files
- Grep: Search for context
- Write: Issue document at ISSUE_PATH only

**Forbidden**:
- Implementing the issue (separate concern)
- Modifying existing artifacts
- Writing outside ISSUE_PATH

## Response Format

After writing the issue, return a brief response:

```
Wrote issue to {ISSUE_PATH}.

Summary: [2-3 sentence summary of issue type and scope]
```

Do not return the full issue content. The document is the deliverable.

## Anti-Patterns

| Avoid | Why | Instead |
|-------|-----|---------|
| Vague titles ("Fix bug") | No context | Descriptive titles ("Validation accepts invalid input") |
| Missing reproduction steps | Bugs unverifiable | Include steps to observe problem |
| Implementation prescriptions | Premature decisions | Describe outcomes, not methods |
| Unbounded scope | Never complete | Define explicit boundaries |
| Missing context | Work cannot begin | Provide artifact references |
| Combining unrelated work | Tracking difficulty | One issue per concern |
