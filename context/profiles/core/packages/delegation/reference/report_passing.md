# Report Passing Between Phases

The rationale and worked patterns behind the report-passing rule: why
main agents pass report paths instead of report content, how the
pattern applies to work-item execution, and what the single
aggregation carve-out actually permits.

Main agents pass report paths between phases; they do not read
intermediate sub-agent reports.

## Rationale

Main agents need report paths for orchestration, not report content.
Reading intermediate reports consumes context budget that should be
reserved for orchestration logic. In multi-phase orchestrations this
scales poorly, as accumulated report content displaces reasoning
space.

The cost compounds rather than adds: every report the orchestrator
reads stays resident for the remainder of the orchestration, so a
main agent that reads each phase's output arrives at the final phase
with its context dominated by material it did not need and cannot
evict.

## The correct pattern

Each phase agent receives the paths it needs and reads them itself.
The orchestrator handles paths only.

```
Phase 1: Discovery agent writes report, returns summary + path
Phase 2: Analysis agent receives path, reads discovery report
Phase 3: Validation agent receives paths, reads previous reports
```

The sub-agent, not the orchestrator, pays the context cost of reading
— and it pays that cost inside a context that is discarded when the
sub-agent returns.

## The incorrect pattern

```
Phase 1: Discovery agent writes report, returns summary + path
Phase 2: Main agent reads discovery report, summarizes for analysis agent
Phase 3: Main agent reads more reports, context bloat accumulates
```

Re-summarizing a report for a downstream agent is the specific
anti-pattern: it costs the orchestrator the full read, costs the
sub-agent fidelity, and produces a lossy paraphrase where the
authoritative document was already reachable by path.

## Work-item execution

The pattern applies especially to work items. Issues within stories,
and tasks within issues, execute **sequentially**, with each
receiving all previous work-item reports. **Parallel work-item
execution violates this rule** — parallel siblings cannot receive
each other's reports, so the sequencing guarantee that makes
path-passing sufficient no longer holds.

Use `ctxmod work list --kind report --owner <id>` to discover the
available reports for a work item rather than globbing the reports
directory by hand.

## Report-record creation (just-in-time, by the sub-agent)

Dispatchers never pre-assign report paths. The report-writing sub-agent
creates its own report record just-in-time — immediately before writing —
with `ctxmod work new report "<title>" --owner <owning work item, if
any>`, writes to the `file` it prints (an entry named
`<DD-HHMM>-<id>-<slug>.md`), and returns that path and record id in its
final message; the parent passes the returned path onward. This removes
the depth-3 dispatch defect (agents mandated to write reports but given
no path) and name-targeting collisions by construction, and the
timestamped entry name never trips a sub-agent report-file Write guard
keyed on `REPORT.md`-style basenames. Report/summary/findings/analysis
`.md` files MAY be written into the work store — a harness
findings-as-text note is countermanded for that scope only.

### Transitional tolerance (legacy dispatches)

A legacy dispatch that still pre-mints or hand-assembles a report path
for a child is honored as given — but it is advisory, never
authoritative. A child that honors reserve-before-create and finds the
provided path unwritable (a harness report-file Write guard blocks
REPORT/SUMMARY/FINDINGS/ANALYSIS-prefixed basenames even inside the
work store) creates its own record, which lands at a different
date/time entry name (same slug suffix). The store's record ids — each
minted with an exclusive publish — are the SSOT for collision-free
paths; a path no record owns is absent to any rule-following writer.
This tolerance is marked for later removal once no dispatch template
mints report paths.

Consequences, observed across a ~21h-delayed child:

- Expect a child's report by listing the reports tree, not by trusting
  the announced path — and a "missing at announced path" observation
  does not mean the child failed.
- A child completing after its section closed should SendMessage its
  actual path to the parent and any downstream consumers the parent
  named; the parent may already have recorded the work as
  abandoned/unavailable.
- A parent receiving the late report reconciles its own stale
  provenance claims (edit the section and process notes to cite the
  real path) and re-notifies consumers who received the unavailable
  framing.

<!-- anchor: subreport-path-divergence-reserve-before-create -->

## Exceptions

**None.** Main agents pass report paths; they never read intermediate
reports.

Final summary aggregation may read reports — but a main agent doing
that aggregation still returns a summary plus the report path to its
own invoker, rather than returning report content. The carve-out
covers reading at the aggregation step; it never licenses propagating
report content upward.

## Related

Report *passing* is about context economy and is distinct from report
*verification*: a sub-agent's state-changing claims must still be
verified in the orchestrator's own working tree, using the
orchestrator's own tools rather than by reading the report body.
See `subagent_report_verification` in the ctxmod builtin `delegation` profile.
