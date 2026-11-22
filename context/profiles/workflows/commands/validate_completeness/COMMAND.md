---
description: "Verify required element presence and coverage"
binds_to_depth: main
---

# validate_completeness

Verify required element presence and coverage.

Usage:

```text
/validate_completeness [path]
```

| Argument | Required | Description |
| --- | --- | --- |
| `path` | no | Target path or glob pattern (defaults to work item scope) |
| `--criteria` | no | Override completeness criteria source |

Work-item context: story | issue.

Launch the `validate_completeness_phase` agent with the command arguments in a brief that grants the `validate_completeness_phase` skill:

```text
<ctxmod_activate skill="validate_completeness_phase" />
<command arguments>
```
