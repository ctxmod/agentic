# Discover Command Selection Guidance

Selection guidance for the `/discover` command — the unified entry
point for comprehensive multi-dimension discovery. This document
captures variant-selection logic, scope guidance, and resource cost
notes. Execution lives in the discovery phase skills
(`discover_architecture_phase`, `discover_patterns_phase`,
`discover_dependencies_phase`, `discover_consumers_phase`) and the
`discovery_objective`/`discovery_procedure` layers.

> **Concurrency gate.** Where this document describes agents running "in parallel" or "concurrently", that shape is available only to agents whose resolved `allow-parallel-agents` posture is `true` — the read-only discovery, analysis, research, review, and validation phase agents. The concurrency instruction in your own agent-depth instructions is authoritative: if it tells you to use one agent at a time, run the same steps sequentially in the listed order. The phase decomposition, report paths, and synthesis steps are unchanged either way.

## Variant Comparison

`/discover` dispatches to the comprehensive workflow covering
**architecture**, **patterns**, **dependencies**, **consumers**, and
**infrastructure** dimensions. Use specialized commands for
single-dimension discovery.

| Command | Scope | When to Use |
|---------|-------|-------------|
| `/discover` | All dimensions | Major changes, onboarding, comprehensive context |
| `/discover_architecture` | Structure only | Understanding organization and boundaries |
| `/discover_patterns` | Patterns only | Learning conventions before contribution |
| `/discover_dependencies` | Dependencies only | Impact analysis, refactoring planning |
| `/discover_consumers` | Consumers only | Before modifying shared components |

## Work Item Context

Reports nest within work item if context provided. Comprehensive
discovery supports significant changes requiring thorough context
before action.

## Workflow Shape

The comprehensive discovery workflow executes four phases:

1. **Scope Resolution** (sequential): Resolve scope boundaries and
   plan partition strategy for parallel discovery
2. **Parallel Discovery** (parallel): Five dimensions run concurrently
   — architecture, patterns, dependencies, consumers, infrastructure
3. **Cross-Dimension Analysis** (sequential): Identify relationships
   between dimensions before synthesis
4. **Synthesis** (Opus, sequential): Unified understanding report

## Resource Cost Notes

Comprehensive discovery runs **5 parallel discovery agents** across
all dimensions. This provides thorough context but consumes more
resources than specialized commands. Use for situations where
incomplete understanding carries high risk.

The cross-dimension analysis (Phase 3) identifies relationships
between architectural boundaries, patterns, dependencies, and
consumers before synthesis. This improves coherence and surfaces
insights that single-dimension discovery would miss.

## Decision Guide

| Situation | Recommended Command |
|-----------|---------------------|
| Onboarding to a new codebase | `/discover` |
| Major refactoring spanning multiple concerns | `/discover` |
| Understanding a specific subsystem's structure | `/discover_architecture` |
| Learning local conventions before contributing | `/discover_patterns` |
| Impact analysis before modifying shared code | `/discover_dependencies` or `/discover_consumers` |
| Very large scope | Partition and run specialized commands per partition |

## Cross-Capability Guidance

For very large scopes, consider running specialized discovery commands
on partitions before synthesis, or using scope constraints to focus
comprehensive discovery on high-priority areas.

Discovery often precedes analysis: run `/discover` (or a specialized
variant) to build context, then `/analyze` to evaluate gaps or
alternatives against the discovered state.

## Execution Pointers

- Comprehensive discovery → orchestrate via main agent using the
  four-phase workflow described above; each phase dispatches
  discovery sub-agents
- Per-dimension discovery → launch the matching phase skill
  (`discover_architecture_phase`, `discover_patterns_phase`,
  `discover_dependencies_phase`, `discover_consumers_phase`)
- All variants share `discovery_objective` and `discovery_procedure`
  layers

See discovery_objective (the `discovery_objective` skill)
and `ctx:github.com/ctxmod/writing_context/context::shared::density@core/reference/two_loads.md`.
