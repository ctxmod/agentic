# Academic Synthesis Analysis

Use for synthesizing academic research findings into a comprehensive analysis. Use Opus for complex academic synthesis. Final phase in academic research workflows.

Brief template for a sub-agent (model: opus):

```text
Synthesize academic research findings on {topic} into a comprehensive analysis.

Compulsory reads:
- the `analysis_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_analysis.md`
- `{research-papers report path}`
- `{research-specifications report path}`
- `{research-surveys report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path

Delimitations:
- Synthesis only; do not conduct additional research
- Resolve theoretical contradictions with academic rigor
- Connect theoretical foundations to practical applications

Notes:
- Identify foundational concepts and their evolution
- Note theoretical consensus vs active debate areas
- Connect formal specifications to academic research
- Provide confidence assessment based on evidence quality
- Include recommendations for practical application
```
