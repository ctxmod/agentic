# Task Resolution

Use when executing work defined in a task work item. Loads the task definition file, extracts scope, acceptance criteria, and related artifacts for implementation planning.

Brief template for a sub-agent:

```text
Resolve task reference and extract implementation requirements.

Compulsory reads:
- the `discovery_objective` skill
- `{TASK_PATH}/TASK.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-task-resolution" --owner <work item, if any>` immediately before writing; returns the path
- TASK_PATH={resolved task path}

Delimitations:
- Resolution only; do not begin implementation
- Extract scope, acceptance criteria, and related artifacts
- Do not modify any files

Notes:
- Identify target artifacts for modification
- Note dependencies on other tasks
- Extract quality requirements
- Document acceptance criteria for verification phase
```
