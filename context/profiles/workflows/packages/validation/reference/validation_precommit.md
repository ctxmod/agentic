# Precommit Validation

Use when verifying precommit hooks pass after work completion. Runs full precommit validation to confirm all issues resolved.

Brief template for a sub-agent:

```text
Verify precommit validation passes after all work items complete.

Compulsory reads:
- `precommit` in the ctxmod builtin `commit_discipline` profile
- the `validation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_validation.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path

Delimitations:
- Validation only; do not modify files

Notes:
- Run precommit hooks against all files
- Verify all findings resolved
- Report any remaining issues
```
