---
description: "Pull request review with change analysis, validation, and approval recommendation"
binds_to_depth: main
---

# review_pr

Pull request review with change analysis, validation, and approval recommendation.

Usage:

```text
/review_pr [pr-identifier]
```

| Argument | Required | Description |
| --- | --- | --- |
| `pr-identifier` | yes | Pull request number, URL, or source branch name |

Work-item context: optional.

Launch the `review_pr_phase` agent with the command arguments in a brief that grants the `review_pr_phase` skill:

```text
<ctxmod_activate skill="agentic_workflows_review_pr_phase" />
<command arguments>
```
