---
description: "Validate technical specifications, research questionable approaches, create story with issues, apply corrections"
binds_to_depth: main
---

# spec

Validate technical specifications, research questionable approaches, create story with issues, apply corrections.

Usage:

```text
/spec [spec-path] [flags]
```

| Argument | Required | Description |
| --- | --- | --- |
| `spec-path` | yes | Path to specification file to validate |
| `--epic` | no | Parent epic record id for story creation (e.g., vdwgsgzfrs) |
| `--story` | no | Existing story record id for issue append (e.g., kpqrstbcdf) |

Work-item context: optional.

Launch the `spec_phase` agent with the command arguments in a brief that grants the `spec_phase` skill:

```text
<ctxmod_activate skill="spec_phase" />
<command arguments>
```
