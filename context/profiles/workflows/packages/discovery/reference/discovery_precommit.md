# Precommit Discovery

Use when discovering issues through precommit hook validation. Detects configuration, runs hooks, classifies failures, applies auto-fixes, and produces categorized issue list.

Brief template for a sub-agent:

```text
Run precommit validation against all repository files.

Compulsory reads:
- `precommit` in the ctxmod builtin `commit_discipline` profile
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path

Delimitations:
- Run precommit hooks only; do not create work items
- Follow procedure exactly (detect config, run hooks, classify failures, apply auto-fixes)

Notes:
- Detect precommit configuration (.pre-commit-config.yaml, .husky, custom hooks)
- Skip if no configuration found
- Classify all failures (format, lint, type, test, security)
- Apply auto-fixes when available
- Re-stage modified files appropriately
- Output: issue list with file:line references and categorization
```
