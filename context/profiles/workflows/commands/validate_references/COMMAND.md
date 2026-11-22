---
description: "Verify cross-reference integrity and link resolution"
binds_to_depth: main
---

# validate_references

Verify cross-reference integrity and link resolution.

Usage:

```text
/validate_references [path]
```

| Argument | Required | Description |
| --- | --- | --- |
| `path` | no | Target path or glob pattern (defaults to scope from context) |
| `--depth` | no | Reference resolution depth (default: 2) |

Work-item context: optional.

Launch the `validate_references_phase` agent with the command arguments in a brief that grants the `validate_references_phase` skill:

```text
<ctxmod_activate skill="validate_references_phase" />
<command arguments>
```
