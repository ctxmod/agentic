# Alternative Research

Use for researching individual alternatives against evaluation criteria. Runs in parallel with one agent per alternative.

Brief template for a sub-agent:

```text
Research {alternative} against defined evaluation criteria.

Compulsory reads:
- `ctx:github.com/ctxmod/agentic/context::shared::discovery_objective@workflows/reference/external.md`
- the `research_compare_phase` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_discovery.md`
- `{criteria-definition report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- EVALUATION_CRITERIA={criteria from Phase 1}

Delimitations:
- Research {alternative} only; do not compare to others
- Do not modify any files
- Cite all sources with URLs

Notes:
- Use perplexity_search for comprehensive coverage
- Address each evaluation criterion explicitly
- Include both strengths and limitations
- Note real-world adoption and case studies
- Document version and recency of information
```
