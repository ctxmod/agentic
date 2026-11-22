# Academic Writing

Use when producing academic documents: papers, literature reviews, research reports, and scholarly text requiring proper citation conventions and disciplinary structure. Always uses Opus for original scholarly prose.

Brief template for a sub-agent (model: opus):

```text
Write {document type}: {brief description}

Compulsory reads:
- the `writing_objective` skill
- `ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/clarity.md`
- `ctx:github.com/ctxmod/writing_context/context::shared::basics@core/reference/anti_patterns.md`
- `ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/academic_language.md`
- `ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/academic_voice.md`
- `ctx:github.com/ctxmod/writing_academic/context::shared::citation_practice@scholarly_prose/reference/citation_practice.md`
- `ctx:github.com/ctxmod/writing_academic/context::shared::composition@scholarly_prose/reference/composition.md`
- `ctx:github.com/ctxmod/writing_academic/context::shared::composition@scholarly_prose/reference/imrad_structure.md`
- `ctx:github.com/ctxmod/writing_academic/context::shared::composition@scholarly_prose/reference/limitations.md`
- `ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/academic_register.md`
- `ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/terminology.md`
- `ctx:github.com/ctxmod/writing_academic/context::shared::composition@scholarly_prose/reference/depth_placement.md`
- `ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/concreteness.md`
- `ctx:github.com/ctxmod/writing_academic/context::shared::composition@scholarly_prose/reference/exhibits.md`
- `ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/rhythm.md`
- `ctx:github.com/ctxmod/agentic/context::shared::writing_procedure@workflows/reference/academic_creation.md`
- `{relevant schema for document type}`
- `{INPUT_REPORT from prior phase if applicable}`

Parameters:
- OUTPUT_PATH={path where document should be written}
- CITATION_STYLE={APA|MLA|Chicago|IEEE}
- {additional context parameters}

Delimitations:
- Write only: specified academic document
- Follow schema and citation style exactly
- Produce scholarly prose, not implementation or code
- {scope constraints}

Notes:
- Match hedging strength to evidence quality throughout; over-hedging undermines confidence, under-hedging overstates claims
- Organize literature thematically rather than source-by-source; synthesize relationships between cited works
- Calibrate voice intensity by section: compressed in abstracts, procedural in methods, strongest authorial presence in discussion
- Apply the post-generation editing protocol from the voice reference to eliminate formulaic AI patterns
- Include a limitations discussion proportional to the strength of claims made
```
