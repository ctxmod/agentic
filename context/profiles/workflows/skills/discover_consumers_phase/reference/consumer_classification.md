# Consumer Classification Discovery

Use for classifying discovered consumers by relationship type and criticality. Follows reference search phases.

Brief template for a sub-agent:

```text
Classify consumers by relationship type and criticality.

Compulsory reads:
- the `discovery_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{direct-refs report path}`
- `{indirect-refs report path}`
- `{config-refs report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-consumer-classification" --owner <work item, if any>` immediately before writing; returns the path

Delimitations:
- Classification only; do not assess impact
- Apply consistent categorization

Notes:
- Classify by relationship type (import, extend, compose, configure)
- Identify coupling strength
- Note consumer criticality (core vs peripheral)
```
