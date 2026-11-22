---
description: "Run precommit validation, create or append to story with issues, execute to completion"
binds_to_depth: main
---

# precommit

Run precommit validation, create or append to story with issues, execute to completion.

Usage:

```text
/precommit [flags]
```

| Argument | Required | Description |
| --- | --- | --- |
| `--epic` | no | Parent epic record id for story creation (e.g., vdwgsgzfrs) |
| `--story` | no | Existing story record id for issue append (e.g., kpqrstbcdf) |
| `--staged-only` | no | Restrict validation to staged files only |

Work-item context: dynamic.

Launch the `precommit_phase` agent with the command arguments in a brief that grants the `precommit_phase` skill:

```text
<ctxmod_activate skill="precommit_phase" />
<command arguments>
```
