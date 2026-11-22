# Work Item Planning

Use when planning story and issue organization from analysis findings. Groups related findings, determines parallelization strategy, and identifies auto-fixable corrections.

Brief template for a sub-agent:

```text
Plan story and issue organization from validation findings.

Compulsory reads:
- the `planning_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::planning_objective@workflows/reference/general.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/approach_selection.md`
- `ctx:github.com/ctxmod/agentic/context::shared::planning_procedure@workflows/reference/dependency_analysis.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/prioritization.md`
- `ctx:github.com/ctxmod/agentic/context::shared::analysis_procedure@workflows/reference/impact_scoring.md`
- `ctx:github.com/ctxmod/agentic/context::shared::items@work/reference/work_structure_story.md`
- `ctx:github.com/ctxmod/agentic/context::shared::items@work/reference/work_structure_issue.md`
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_plan.md`
- `{analysis report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- STORY_PATH={story_path}
- EPIC_ID={epic_id_if_provided_else_null}
- STORY_ID={story_id}

Delimitations:
- Planning only; do not create issues or modify artifacts

Notes:
- Follow general planning objective for approach selection
- Use dependency-analysis procedure for identifying independent issues
- Group related findings into single issues
- Identify independent issues for parallel creation
- Separate auto-fixable issues from complex issues
- Apply impact/effort prioritization
- If creating new story, include story summary and scope
```
