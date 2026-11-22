# Orchestration Phases

Phases define distinct workflow stages that sub-agents execute. Each phase has a specific purpose, expected inputs, and outputs. Workflows compose phases into complete execution plans.

> **Concurrency gate.** Where this document describes agents running "in parallel" or "concurrently", that shape is available only to agents whose resolved `allow-parallel-agents` posture is `true` — the read-only discovery, analysis, research, review, and validation phase agents. The concurrency instruction in your own agent-depth instructions is authoritative: if it tells you to use one agent at a time, run the same steps sequentially in the listed order. The phase decomposition, report paths, and synthesis steps are unchanged either way.

## Phase vs Pattern vs Workflow

| Concept | Scope | Question Answered |
|---------|-------|-------------------|
| **Phase** | Workflow stage | What happens at this stage? |
| **Pattern** | Coordination | How do agents coordinate within/across phases? |
| **Workflow** | Activity | How do phases combine to accomplish a goal? |

## Phase Reference

| Phase | Purpose | Typical Model | Common Patterns |
|-------|---------|---------------|-----------------|
| Discovery | Gather internal context | Sonnet | Parallel, Fan-Out Fan-In |
| Research | Gather external context | Sonnet | Parallel, Fan-Out Fan-In |
| Analysis | Interpret findings into recommendations | Sonnet | Sequential, Fan-Out Fan-In |
| Planning | Structure execution approach | Sonnet | Sequential |
| Execution | Apply changes, create artifacts | Sonnet | Sequential, Parallel, Fan-Out Fan-In |
| Writing | Produce written artifacts (always Opus) | Opus | Sequential |
| Validation | Verify outcomes against criteria | Haiku (story-completion validator) | Validation Loop |
| Remediation | Correct validation findings | Sonnet | Validation Loop, Sequential |
| Project Reference Sync | Synchronize documentation with implementation | Sonnet | Sequential, Validation Loop |

Model assignment for validation is deliberate: story-completion
validation is Haiku-class — the story-completion validator owns scope
derivation, deterministic checks, in-session repair, recheck, and the
commit. Higher tiers are reserved for genuinely complex interpretation,
not routine validation. Specialized validation dimensions (structure,
references, consistency, completeness) are explicitly selected for
high-risk work or explicit requests, never dispatched by default.

## Phase Selection

| Need | Phase |
|------|-------|
| Understand current state | Discovery |
| Consult external sources | Research |
| Interpret findings | Analysis |
| Structure next steps | Planning |
| Apply modifications | Execution |
| Produce written content | Writing |
| Check quality | Validation |
| Fix issues | Remediation |
| Update project docs | Project Reference Sync |

### Common Sequences

```
Understanding: Discovery → Analysis → Planning
Creating:      Discovery → Planning → Execution → Validation
Modifying:     Discovery → Analysis → Execution → Validation
Writing:       Discovery → Analysis → Writing → Validation
Reviewing:     Discovery → Validation → Analysis → Planning
Migrating:     Discovery → Analysis → Planning → Execution → Validation
Fixing:        Validation → Remediation → Validation (loop)
```

### Dependencies

```
Discovery ──┬──> Analysis ──> Planning ──> Execution ──> Validation
            │                              │
Research ───┘                              └──> Writing ──> Validation
                                                               ↓
                                                          Remediation
                                                               ↓
                                                       (loop to Validation)
```

## Phase Details

### Discovery

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::discovery/reference/discovery_general.md" />

**Inputs**: Scope definition, project context references
**Outputs**: Discovery report with findings, patterns, gaps

### Research

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::research/reference/research_general.md" />

**Inputs**: Research questions, source type preferences
**Outputs**: Research report with sourced findings, synthesis

### Analysis

the `analysis_objective` skill

**Inputs**: Discovery report, research report (optional), target specification (for gap analysis)
**Outputs**: Analysis report with interpretations, patterns, gaps, recommendations

### Planning

the `planning_phase` skill

**Inputs**: Analysis report with findings and recommendations
**Outputs**: Planning report with steps, dependencies, success criteria, risks

### Execution

the `implementation_objective` skill

**Inputs**: Planning report, target artifact paths
**Outputs**: Implementation report, modified/created artifacts

### Writing

the `writing_phase` skill

**Inputs**: Discovery/analysis reports, schema references, output path
**Outputs**: Written artifact at designated path

Always uses Opus—no exceptions.

### Validation

the `validation_objective` skill

**Inputs**: Artifact paths, validation criteria
**Outputs**: Validation report with findings by severity, patterns, pass/fail status

Story completion routes to the story-completion validator (Haiku-class):
one session that validates against deterministic oracles, repairs scoped
failures — including proven pre-existing ones in touched or related
packages — rechecks every changed byte, and commits with commit-scope
byte identity. See
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/deliberate_verification.md" />.
Specialized validation phases are reserved for explicit high-risk needs
or user requests, not routine gates: simple tasks with low
instruction-following risk need no per-phase validation beyond the local
checks at phase boundaries.

### Remediation

the `remediation_phase` skill

**Inputs**: Validation report, artifact paths
**Outputs**: Remediation report documenting corrections

### Project Reference Sync

the `project_reference_sync_phase` skill (`github.com/ctxmod/writing_context/context`, `authoring` profile)

**Inputs**: Reference scope, project reference schema
**Outputs**: Sync report with inventory, drift detection, corrections

## Context Flow

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />

Phases connect through report paths:

1. Main agent launches phase sub-agent with parameters
2. Sub-agent reads compulsory files, executes objective
3. Sub-agent reserves its report path just-in-time (`ctxmod work new report "<title>"`) and writes there
4. Main agent receives report path (not content)
5. Main agent passes report path to next phase as INPUT_REPORT

Main agents never read intermediate reports—they pass report paths between phases.

## Phase Parameters

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />

| Parameter | Purpose |
|-----------|---------|
| Report (reserved) | Sub-agent creates the report record just-in-time with `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path |
| INPUT_REPORT | Prior phase report to consume |
| SCOPE | What to examine/modify |
| Work item context | EPIC_PATH, STORY_PATH, ISSUE_PATH |

Phase-specific parameters documented in individual phase files.

## Anti-Patterns

| Avoid | Instead |
|-------|---------|
| Skipping discovery | Gather context before subsequent phases |
| Analysis before discovery | Complete discovery first |
| Execution before planning | Plan before acting |
| Skipping validation | Validate after execution |
| Remediation without validation | Validate to provide correction guidance |
| Main agent reading reports | Pass report paths only |
| Unbounded phase scope | Define clear boundaries |

## Related

<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::patterns/reference/patterns_overview.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/workflows_overview.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/phase_information_flow.md" />
<ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::framework/reference/params.md" />
