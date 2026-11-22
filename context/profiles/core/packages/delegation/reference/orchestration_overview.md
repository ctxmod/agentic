# Orchestration Reference Overview

Entry point for the orchestration reference corpus in this module: the operating rules for multi-agent work in a repository — dispatch discipline, verification duties, and the environment traps that recur. Read the mantra files before orchestrating; consult the trap files when the corresponding situation arises.

## Topic index

| File | Covers |
| --- | --- |
| <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/main_agent_mantra.md" /> | Orchestrator duties: dispatch pattern, verification before acceptance, work-item status signals, dispatch sizing, waiting and resume discipline, workflow phase discipline |
| <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/subagent_mantra.md" /> | The subagent-side counterpart: scope, reporting, allowlist |
| `subagent_report_verification` in the ctxmod builtin `delegation` profile | Failure-mode checklist for sub-agent reports — fabricated success, false reverts, invented instructions, "pre-existing" test dismissals, guard-test inversions, spec omissions — and the verification mechanics that catch them |
| `stale_inputs_and_drift` in the ctxmod builtin `delegation` profile | Briefs, issue premises, line anchors, and cached scans go stale under concurrent execution; verify at HEAD before building on them |
| `ctx:github.com/ctxmod/agentic/context::shared::harness@shared_tree/reference/harness_traps.md` | Edit-tool virtualization and trailing-whitespace stripping, the Go command guard, pgrep self-match, cwd/shim probe traps, work-refs comment tokens |
| The host's own environment notes (private context) | Host layout, the ENOSPC root-cause checklist, check-gate recovery under disk pressure |
| The repository's own decision records | Durable product decisions (for example notification architecture, background-unit naming, log retention) |
| The repository's own commit workflow, and the `git_commit_*` skills of the agentic `workflows` profile | Commit suggestion and execution workflow |
| `pull_request_design` in the ctxmod builtin `commit_discipline` profile | Body design for pull requests and pushed commit rounds: summary visual, before/after evidence, merge danger |
| `shared_git_index_hazards` in the ctxmod builtin `commit_discipline` profile | Concurrent agents sharing one git index |
| `ctx:github.com/ctxmod/agentic/context::shared::git_mechanics@shared_tree/reference/shared_tree_git_mechanics.md` | Host git mechanics in the shared tree |
| `isolation_by_checkout` in the ctxmod builtin `no_worktrees` profile | The agent worktree ban (shell and harness isolation) and the sanctioned clean-HEAD checkout vehicles |
| <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/lesson_capture.md" /> | The delegated lesson-capture pipeline: commission, authorship, nested validation, render, local excludes |

## Reading order

New orchestrators read `main_agent_mantra` and `subagent_mantra` first, then the builtin `subagent_report_verification` and `stale_inputs_and_drift` before dispatching any execution work — those two encode the recurring ways multi-agent runs go wrong. The trap files (`harness_traps`, the host's environment notes, `shared_git_index_hazards`, `shared_tree_git_mechanics`, `worktree_baselines_and_isolation`) are consultative: load the relevant one when probes, disk pressure, commits, or isolation come up. When a friction finding must become a rendered rule, <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::delegation/reference/lesson_capture.md" /> is the route: commission the pipeline once and let the delegated phases run it.
