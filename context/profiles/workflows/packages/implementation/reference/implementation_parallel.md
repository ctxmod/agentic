# Parallel Execution

**Applicability gate.** Implementation agents modify files, so they are **not** granted `allow-parallel-agents: true` — concurrent writes to overlapping scopes are the documented failure mode this default guards against. Unless the concurrency instruction in your own agent-depth instructions explicitly permits concurrent dispatch, launch these execution agents **one at a time, in the listed order**, waiting for each to complete. That instruction is authoritative and overrides this file.

This template covers the case where multiple genuinely independent scopes require execution — disjoint packages, no shared files, no cross-scope references. Launch one agent per scope to maintain scope isolation; under the default posture that launch is sequential, and the isolation, not the concurrency, is what the template buys you. Concurrent launch additionally requires isolated clean-HEAD checkouts per scope (clone or archive+init snapshot) — worktrees are banned.

Launch one execution agent per independent scope:

Brief template for a sub-agent (model: opus):

```text
Implement changes for {scope A}.

Compulsory reads:
- the `implementation_objective` skill
- `ctx:github.com/ctxmod/agentic/context::shared::reporting@workflows/reference/structure_implementation.md`
- `{planning report path}`

Parameters:
- Report: sub-agent reserves just-in-time via `ctxmod work new report "<title>" --owner <work item, if any>` immediately before writing; returns the path
- INPUT_REPORT={planning report path}
- SCOPE={scope A}

Delimitations:
- Only modify {scope A} artifacts
```

## Model Override

This variant uses Opus instead of the phase default. Parallel execution of modifications requires higher accuracy per agent to avoid cross-scope issues.
