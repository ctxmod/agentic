# Criteria Definition Research

Use for defining evaluation criteria for comparing alternatives. First phase in comparative research workflows.

Brief template for a sub-agent — Define evaluation criteria for comparative research:

```text
Define evaluation criteria for comparing: {alternatives}
User-specified criteria: {criteria or "derive appropriate criteria"}

Compulsory reads:
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/external.md`
- the `research_compare_phase` skill
- `ctx:github.com/ctxmod/engineering/context::shared::practice@principles/reference/research_first.md`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "{nn}-criteria-definition" --owner <work item, if any>` immediately before writing; returns the path

Delimitations:
- Criteria definition only; do not research alternatives yet
- Generate 5-10 evaluation criteria with clear definitions
- Include criteria weight suggestions based on common priorities

Notes:
- Balance technical criteria (performance, scalability) with practical criteria (learning curve, community support)
- Include both quantitative and qualitative criteria
- Define how each criterion will be measured or assessed
```
