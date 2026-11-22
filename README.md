# Agentic

Doctrine and workflow skills for agentic work, for ctxmod. Work state lives in the ctxmod work store (`ctxmod work …`).

Profiles:

- `core` (default): delegation (main-agent and sub-agent mantras, report passing, research briefs, phase information flow, lesson capture), coordination patterns, verification rules, and verification-evidence doctrine.
- `workflows`: depth-layered skills — phase, objective, and procedure — for discovery, analysis, planning, implementation, validation, remediation, research, review, reporting, git commits, and cleanup; the workflow framework (parameters, phases, workflow definitions); command-selection guidance; and slash commands. Extends `core`.
- `work`: work-item orchestration on the ctxmod work store — the work router, intent capture and writing, lifecycle phases, work-item schemas and templates, and store upkeep. Extends `workflows`.
- `academic`: academic research and academic task phases. Extends `workflows`.
- `teaching`: a stateful teaching workspace (the `teach` skill). Extends `core`.
- `shared_tree`: hazards and mechanics of several agents sharing one working tree — git mechanics, harness traps, the gate-runner pattern, and edit verification. Extends `core`.

Select a profile by its selector, e.g. `github.com/ctxmod/agentic/context/profiles/workflows`.

Layout notes:

- Skills live at `context/profiles/<profile>/skills/<name>/SKILL.md`, with their variant files in the skill's own `reference/`. Skills are available, not active: an orchestrator grants one to a sub-agent with `<ctxmod_activate skill="agentic_<profile>_<name>" />` (the canonical skill name) in the spawn brief (the brief templates in each skill show the shape).
- Capability packages (`context/profiles/<profile>/packages/<capability>/package.yaml`) declare the agents that run each skill and carry the shared variant references.
- Slash commands live at `context/profiles/<profile>/commands/<name>/COMMAND.md`.
- Engineering principles (DRY, KISS, SSOT) come from `github.com/ctxmod/engineering/context`; context-writing doctrine from `github.com/ctxmod/writing_context/context`. Pointers into other modules are written as `ctx:` references in code spans.
