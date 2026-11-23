# Findings Channel

The sanctioned persistence path for validation findings when report
writes are prohibited, and for any finding that must survive a
continuation boundary. The trigger from the field: dispatches carried
"DO NOT write markdown reports (developer constraint)" and "NOT a
report .md (developer prohibits reports, work items allowed)", and
findings then lived in return messages or /tmp evidence files — one
stale-hook finding survived only because it happened to be converted to
a work item, and in the adjacent policy-rollout window a prohibited
writer wrote its markdown report anyway, recorded as a violation with
the evidence kept in JSON. The channel's contract: findings land in a
durable, reserved, schema-stable location, and the return message
carries the path plus a one-line verdict — so report-passing survives
the prohibition instead of degrading to prose.

## The two forms

- **Evidence JSON (default).** Reserve an artifact path under the owning
  work item (`ctxmod work new <artifact kind> "<title>"`, slug
  `findings-evidence`, a `.json` extension) and write the schema below.
  A continuation rewrites the same reserved file and updates
  `continuations` and the re-verification fields; the reservation, not
  the first write, owns the file's identity.
- **Work-item append.** When the finding changes the owning work item's
  own state (it blocks an acceptance criterion or adds scope), or the
  writer cannot create a record, append a
  `## Findings — <writer> (<date>)` section to the work item carrying
  the same JSON in a fenced block. Appending to work items in place is
  already sanctioned practice; acceptance-criteria checkboxes are ticked
  the same way.

Either form is the findings channel, and the return message carries its
path either way. /tmp files, transcript prose, and findings that exist
only in the return message are not the channel — they are the failure
the channel exists to prevent.

## The schema

`findings-evidence/1` — one file (or one fenced block) per
writer-session. Unknown fields are forbidden; a new optional field
bumps the `schema` value. Angle-bracket placeholders mark values a live
capture fills in:

```json
{
  "schema": "findings-evidence/1",
  "written_at": "<RFC3339 UTC>",
  "writer": "<identity string from the dispatch brief>",
  "work_item": "<owning work item id, e.g. vdwgsgzfrs>",
  "gate": "<gate or dispatch name; omit when none>",
  "coordination_token": "<the brief's token; omit when none>",
  "report_write_prohibited": true,
  "reserved_report_path": "<reserved report path; omit when none was reserved>",
  "reserved_report_written": false,
  "continuations": 0,
  "reverified_at": "<RFC3339 UTC; REQUIRED when continuations > 0>",
  "reverified_how": "<what was re-checked against live state, and how>",
  "findings": [
    {
      "id": "F1",
      "severity": "fail",
      "title": "<one-line finding statement>",
      "detail": "<factual sentence carrying the observed values>",
      "evidence": [
        {
          "kind": "command-output",
          "path": "<captured artifact path>",
          "sha256": "<full 64-hex sha256 of the artifact; omit rather than truncate>",
          "quote": "<the load-bearing line, verbatim>"
        }
      ],
      "suggested_owner": "<coordinator | writer | validator | operator>",
      "status": "open"
    }
  ]
}
```

Field rules:

- `writer` is the identity the dispatch brief assigned; `gate` and
  `coordination_token` echo the brief when it carried them.
- `reserved_report_path` and `reserved_report_written` — when a report
  path was reserved before the prohibition was known, name it and
  record `false`. A reserved-but-unwritten report is never evidence;
  consumers treat it as absent.
- `continuations` counts compaction continuations experienced. When it
  is nonzero, `reverified_at` and `reverified_how` are REQUIRED — the
  post-continuation re-verification against live state is mandatory,
  not optional.
- `findings[].severity` is `fail`, `warn`, or `note`, aligned with the
  validator's own scale when one exists.
- `findings[].evidence[].sha256` is the full 64-hex form of the
  captured artifact or the field is omitted — a truncated hash is
  evidence corruption by summarization and is forbidden outright;
  `quote` carries the verbatim load-bearing line. `kind` is
  `command-output`, `file-content`, or `observation`.
- `findings[].status` is `open`, `acted`, or `deferred`, so a consumer
  can drive disposition without re-deriving it.

## Worked example (the W4 round, 2026-10-06)

The case: a W4-round validator reserved its markdown report and then
hit the session's explicit report-write prohibition — its own verdict
states the shape this channel formalizes: "Requested markdown report
reserved but must remain unwritten under explicit developer prohibition;
factual reserved JSON evidence + direct handoff will satisfy persistence
without violating." In the same window the round's stale-hook finding
survived only through ad hoc conversion to a work item, and a compaction
continuation produced a duplicate validator that parked correctly but
only after four compulsory reads. Under this channel, the round's
persistence is one reserved file (the JSON below is a marked sketch
reconstructed from the incident record — a live capture carries the
real hash and paths, in full form):

```json
{
  "schema": "findings-evidence/1",
  "written_at": "2026-10-06T05:41:00Z",
  "writer": "w4-acceptance-validator",
  "work_item": "demo-release/W4",
  "gate": "W4 independent acceptance",
  "coordination_token": "w4-acceptance/<expected evidence artifact>",
  "report_write_prohibited": true,
  "reserved_report_path": "<the reserved-but-unwritable report path>",
  "reserved_report_written": false,
  "continuations": 1,
  "reverified_at": "2026-10-06T05:39:30Z",
  "reverified_how": "re-read both captured artifacts and re-ran the failing hook check against the live tree before recording",
  "findings": [
    {
      "id": "F1",
      "severity": "fail",
      "title": "Stale commit-hook binary executed during the policy rollout",
      "detail": "A hook binary older than the current source executed for the rollout; the freshness check caught it and forced a rebuild.",
      "evidence": [
        {
          "kind": "command-output",
          "path": "<captured rollout log>",
          "sha256": "<full 64-hex sha256 of the captured log>",
          "quote": "a stale hook binary was caught by the freshness check itself and forced a rebuild"
        }
      ],
      "suggested_owner": "writer",
      "status": "open"
    }
  ]
}
```

The return message then carries one line — the findings-evidence path,
the finding count, and the verdict — which is the report-passing
contract with the channel file standing in the report's place. When the
prohibition lifts before the reserved report is needed, the report is
written from the channel file, not from memory.
