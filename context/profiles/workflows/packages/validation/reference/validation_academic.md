# Academic Validation

Use when validating academic documents against scholarly quality criteria. Checks citation correctness, evidence quality, source credibility, bias indicators, cross-validation of claims, and structural compliance with academic conventions.

Academic validation differs from standards validation (which checks project conventions) and completeness validation (which checks element presence). Academic validation assesses whether documents meet scholarly rigor expectations across citation integrity, evidence handling, and disciplinary structure.

Brief template for a sub-agent — Validate academic document quality:

```text
Validate academic quality for: {scope or artifact description}

Compulsory reads:
- the `validation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_validation.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/severity_classification.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/quality_assurance.md`
- `ctx:github.com/ctxmod/agentic/context::shared::validation_procedure@workflows/reference/cross_validation.md`
- `ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure@academic/reference/source_evaluation_academic.md`
- `ctx:github.com/ctxmod/agentic/context::shared::scholarly_research_procedure@academic/reference/bias_detection_academic.md`
- `ctx:github.com/ctxmod/writing_academic/context::shared::citation_practice@scholarly_prose/reference/citation_practice.md`
- `ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/academic_language.md`
- `ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/academic_register.md`
- `ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/terminology.md`
- `ctx:github.com/ctxmod/writing_academic/context::shared::composition@scholarly_prose/reference/exhibits.md`
- `ctx:github.com/ctxmod/writing_academic/context::shared::language@scholarly_prose/reference/rhythm.md`
- `{discovery report if available}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- ARTIFACT_PATHS={paths to validate}
- CRITERIA_TYPE={citations|evidence|sources|bias|structure|all}

Delimitations:
- Academic quality validation only
- Do not modify artifacts
- Do not conduct new research to fill gaps

Notes:
- Run Source Reliance Checklist against each cited source
- Run Citation Readiness Checklist against each citation instance
- Run Standalone Document Checklist against the full document
- Apply CRAAP framework to evaluate source quality scores
- Check triangulation status for key claims using cross-validation strategies
- Assess replication status for empirical findings
- Apply source-level and process-level bias detection procedures
- Verify structural compliance with applicable convention (IMRaD or equivalent)
- Check academic language standards compliance
- Verify confidence levels stated for uncertain findings
- Missing or fabricated citations are Errors
- Unverified claims lacking source attribution are Errors
- Sources failing CRAAP below threshold (score under 15) are Warnings
- Untriangulated single-source findings are Warnings
- Detected bias without acknowledgment is Warning
- Minor language or formatting deviations are Info
```
