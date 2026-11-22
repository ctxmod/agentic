---
description: "Research and compare alternatives for informed decision-making"
binds_to_depth: main
---

# research_compare

Research and compare alternatives for informed decision-making.

Usage:

```text
/research_compare [alternatives] [criteria]
```

| Argument | Required | Description |
| --- | --- | --- |
| `alternatives` | yes | Comma-separated list of alternatives to compare |
| `criteria` | no | Evaluation criteria (defaults to common criteria for the domain) |

Work-item context: optional.

Launch the `research_compare_phase` agent with the command arguments in a brief that grants the `research_compare_phase` skill:

```text
<ctxmod_activate skill="agentic_workflows_research_compare_phase" />
<command arguments>
```
