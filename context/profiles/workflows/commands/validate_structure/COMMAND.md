---
description: "Verify schema and convention compliance for artifacts"
binds_to_depth: main
---

# validate_structure

Verify schema and convention compliance for artifacts.

Usage:

```text
/validate_structure [path]
```

| Argument | Required | Description |
| --- | --- | --- |
| `path` | no | Target path or glob pattern (defaults to scope from context) |
| `--criteria` | no | Override default validation criteria |

Work-item context: optional.

Launch the `validate_structure_phase` agent with the command arguments in a brief that grants the `validate_structure_phase` skill:

```text
<ctxmod_activate skill="agentic_workflows_validate_structure_phase" />
<command arguments>
```
