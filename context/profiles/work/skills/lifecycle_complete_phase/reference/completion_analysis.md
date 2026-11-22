# Completion Analysis

Use as fan-in synthesis after parallel criteria and children verification. Consumes both verification reports and determines overall completion eligibility.

If verification reports are ambiguous, `ctxmod work show <id>` provides supplemental metadata. Primary input remains the verification reports. See `work_store_first` in the ctxmod builtin `work_discipline` profile.

Brief template for a sub-agent (model: sonnet):

```text
Synthesize completion verification results for {WORK_ITEM_ID}.

Compulsory reads:
- `{CRITERIA_REPORT}`
- `{CHILDREN_REPORT}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "completion-analysis" --owner <work item, if any>` immediately before writing; returns the path
- CRITERIA_REPORT={criteria verification report path}
- CHILDREN_REPORT={children verification report path}
- WORK_ITEM_PATH={work item directory}
- WORK_ITEM_TYPE={epic|story|issue|task}

Delimitations:
- Analysis only; do not modify work item
- Produce clear completion eligibility determination

Notes:
- Read both verification reports
- Synthesize criteria and children verification
- Determine if completion requirements are met
- List any blocking issues
```

## Model Override

Uses Sonnet instead of the lifecycle default. Synthesis across multiple verification reports requires higher reasoning capability than individual field-level tasks.
