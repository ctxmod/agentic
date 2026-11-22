---
description: "Verify cross-artifact alignment and terminology uniformity"
binds_to_depth: main
---

# validate_consistency

Verify cross-artifact alignment and terminology uniformity.

Usage:

```text
/validate_consistency [path]
```

| Argument | Required | Description |
| --- | --- | --- |
| `path` | no | Target path or glob pattern (defaults to scope from context) |
| `--baseline` | no | Reference artifact for consistency comparison |

Work-item context: optional.

Launch the `validate_consistency_phase` agent with the command arguments in a brief that grants the `validate_consistency_phase` skill:

```text
<ctxmod_activate skill="agentic_workflows_validate_consistency_phase" />
<command arguments>
```
