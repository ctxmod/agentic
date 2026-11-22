---
description: "Orchestrate academic tasks - write papers, correct documents, analyze sources, integrate findings"
binds_to_depth: main
---

# scholarly_task

Orchestrate academic tasks - write papers, correct documents, analyze sources, integrate findings.

Usage:

```text
/scholarly_task [task-description] [flags]
```

| Argument | Required | Description |
| --- | --- | --- |
| `task-description` | yes | Description of the academic task to perform |
| `--type` | no | Task type override (writing, correction, analysis, integration) |
| `--citation-style` | no | Citation format (APA, MLA, Chicago, IEEE). Defaults to APA. |
| `--document` | no | Path to existing document (for correction/revision) |
| `--sources` | no | Paths to source documents (for analysis/integration) |
| `--output` | no | Target output path |
| `--epic` | no | Parent epic record id for story creation (e.g., vdwgsgzfrs) |
| `--story` | no | Existing story record id for issue append (e.g., kpqrstbcdf) |

Work-item context: optional.

Launch the `scholarly_task_phase` agent with the command arguments in a brief that grants the `scholarly_task_phase` skill:

```text
<ctxmod_activate skill="scholarly_task_phase" />
<command arguments>
```
