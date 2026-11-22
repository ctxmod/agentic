# Research Command Selection Guidance

Selection guidance for the `/research` command — the unified entry
point for comprehensive multi-source research. This document captures
variant-selection logic, source-category coverage, and resource cost
guidance. Execution lives in the research phase skills
(`research_practices_phase`, `scholarly_research_phase`,
`research_compare_phase`) and the shared
`research_procedure`/`scholarly_research_procedure` layers.

> **Concurrency gate.** Where this document describes agents running "in parallel" or "concurrently", that shape is available only to agents whose resolved `allow-parallel-agents` posture is `true` — the read-only discovery, analysis, research, review, and validation phase agents. The concurrency instruction in your own agent-depth instructions is authoritative: if it tells you to use one agent at a time, run the same steps sequentially in the listed order. The phase decomposition, report paths, and synthesis steps are unchanged either way.

## Variant Comparison

`/research` dispatches to the comprehensive workflow covering
**documentation**, **blogs**, **academic papers**, **surveys**,
**community forums**, **GitHub implementations**, and **formal
specifications**. Use specialized commands for single-source-type
research.

| Command | Scope | When to Use |
|---------|-------|-------------|
| `/research` | All sources | Significant decisions, comprehensive grounding |
| `/research_practices` | Web, community, GitHub | Current best practices, practical guidance |
| `/scholarly_research` | Papers, specs, surveys | Theoretical foundations, formal validation |
| `/research_compare` | Per-alternative analysis | Technology selection, approach evaluation |

## Work Item Context

Reports nest within work item if context provided. Comprehensive
research supports significant decisions requiring broad evidence
before commitment.

## Workflow Shape

The comprehensive research workflow executes four phases:

1. **Query Formulation** (sequential): Formulate search queries for
   all source categories (3-5 web, 3-5 academic, 2-3 community, 2-3
   GitHub, 2-3 standards)
2. **Parallel Research** (parallel): Seven source categories run
   concurrently — documentation, blogs, academic, surveys, community,
   GitHub, standards
3. **Source Aggregation** (sequential): Group findings by theme across
   all sources; identify cross-source corroboration and contradictions
4. **Synthesis** (Opus, sequential): Comprehensive synthesis with
   recommendations, confidence levels, and documented trade-offs

## Resource Cost Notes

Comprehensive research runs **7 parallel search agents** across all
source categories. This provides broad coverage but consumes more
resources than specialized commands. Use for decisions where
incomplete evidence carries high risk.

The aggregation phase (Phase 3) groups findings thematically before
synthesis, reducing the cognitive load on the synthesis agent and
improving coherence of the final report.

## Decision Guide

| Situation | Recommended Command |
|-----------|---------------------|
| Significant decision requiring broad evidence | `/research` |
| Practical guidance from current best practices | `/research_practices` |
| Theoretical foundations / formal validation | `/scholarly_research` |
| Technology selection between alternatives | `/research_compare` |
| Extremely broad topic | Decompose into sub-topics and run multiple `/research_practices` commands before a unified synthesis |

## Cross-Capability Guidance

Research output feeds naturally into analysis: run `/research` (or a
specialized variant) to gather evidence, then `/analyze` to evaluate
the evidence against decision criteria. The `/research` aggregation
report is a high-value input to `/analyze` discovery phases.

For extremely broad topics, consider decomposing into sub-topics and
running multiple `/research_practices` commands before a unified
synthesis.

## Source-Category Coverage

The comprehensive command covers seven categories; specialized
commands cover subsets:

| Source Category | Comprehensive | practices | academic | compare |
|-----------------|:-------------:|:---------:|:--------:|:-------:|
| Official documentation | yes | yes | — | — |
| Technical blogs | yes | yes | — | — |
| Academic papers | yes | — | yes | — |
| Surveys / reviews | yes | — | yes | — |
| Community forums | yes | yes | — | — |
| GitHub implementations | yes | yes | — | — |
| Formal specifications | yes | — | yes | — |
| Per-alternative deep dive | — | — | — | yes |

## Execution Pointers

- Comprehensive research → orchestrate via main agent using the
  four-phase workflow described above; each phase dispatches
  research sub-agents
- Per-source-type research → launch the matching phase skill
  (`research_practices_phase`, `scholarly_research_phase`,
  `research_compare_phase`)
- All variants share `research_procedure` and (for academic)
  `scholarly_research_procedure` layers

See research-objective (the `research_practices_phase` skill)
and `ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`.
