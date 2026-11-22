# Gate-Runner Pattern

The sanctioned way to run long gates and load experiments from an
agent session: launch detached, capture the process identity, mark
progress in files, and wait in bounded chunks. It replaces every
hand-rolled `/tmp/*-gates.sh`-class settle-ping loop (background
scripts that poll a warm window and race gates into it) — the same
job, with the acceptance decision delegated to the sanctioned
wait primitive instead of a hand-rolled one.

## Launch: setsid detach with recorded identity

> On some hosts setsid-detached processes die within seconds (a host
> reaper culls unattached clients); there, run foreground and accept
> the Bash tool's move-to-background at its timeout cap — the harness
> task keeps running and delivers a completion notification. The
> identity-capture mechanics here still apply to whatever launcher
> survives.

Harness background tasks are killed at roughly the 60-minute mark;
a run that can approach that bound must outlive the session's task
table. Launch through `setsid` with output and stdin redirected, and
record the launcher PID from `$!`:

```sh
setsid env <PINNED_BINARY_VARS>=... \
    bash -c 'set -a; source <bench>/config.frozen; set +a; exec <harness>.sh <scratch>' \
    > /tmp/<run>.log 2>&1 < /dev/null &
echo "launcher=$!"
```

Verify the identity before trusting any later wait: read
`/proc/<pid>/cmdline` (empty means the process already exited — do
not mistake that for a hang). For daemon-binary evidence, verify
`/proc/<daemon_pid>/exe` against the pinned binary's sha256; the
build-dir output is not always what executes.

## Waiting: bounded tail holds, never sleep loops

A hook layer may deny consecutive `sleep` calls. The sanctioned wait
is a bounded hold on the launcher PID; each hold is capped so a
single chunk can never wedge the session (keep consecutive chunks at
or under 8 minutes):

```sh
timeout -k 30 <M> tail --pid=<pid> -f /dev/null
```

`tail --pid` exits the moment the process is gone, so completion
wakes the caller immediately; the `timeout` cap bounds the
not-completed case. Check progress between chunks by reading the
run's log tail and its artifact files — never by re-deriving state
from `ps` output.

## Marker discipline: artifacts, not process tables

Every long run records its own evidence in files the caller reads:
a warm-up record, a burst record, per-call NDJSON shards, status
samples, and a summary written last (the summary's existence is the
completion marker). Two rules keep markers honest:

- **Treat `/tmp` markers as stale by default.** A leftover output
  file from an earlier attempt looks exactly like the current run's
  output. Name markers per-run (the scratch directory is the
  namespace) and prefer reading the summary written at completion
  over any file a partial run may have touched mid-flight.
- **Stop by recorded PID only.** `kill -TERM <recorded pid>`, then a
  bounded wait, then SIGKILL — never `pkill`/`pgrep` by name (the
  pattern matches the caller's own compound command; see the pgrep
  self-match trap in <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::harness/reference/harness_traps.md" />).

## The wait primitive replaces settle-ping loops

When the gate is waiting on a daemon rather than on a local
process, do not hand-roll a ping loop. Use a wait primitive that
delegates acceptance to the client's own settle machinery and answers
in scripting exit codes — for example 0 settled, 3 timeout (healthy
daemon, deadline expired), 4 unreachable. Its deadline is a fixed
constant in code; a script that needs its own bound wraps the binary
in its own `timeout`. Stdout carries a JSON payload (verdict, waited
time, scopes); stderr carries diagnostics.

```sh
timeout -k 30 600 <wait-primitive> /path/to/repo   # exit 0 | 3 | 4
```

## Live-host discipline (shared daemon, shared machine)

Peer sessions share the host and its daemons. Serialize heavy runs;
stamp every run with the calibration load band (nproc plus sampled
loadavg); filter evidence windows by timestamp; never attribute
foreign commits or foreign daemons' behavior to your run; keep
experiments inside redirected scratch universes (HOME/XDG) so the
live daemon and live state are untouched; one bounded attempt per
commit round — a censored observation is recorded, never retried
silently.

## Turn management: waits, survival, cadence

Organized by actor: the coordinator in-turn, the dispatched worker,
and the phase agent's assumptions about its own background tasks. A
`timeout N command sleep M` fails because `command` is a shell builtin,
not an executable — the working shape is
`timeout N bash -c 'command sleep M'`; and a sleep guard's "end turn,
you will be notified" advice holds only for nested agents — a detached
external process fires no notification, which is what the bounded
primitives below exist for.
<!-- anchor: timeout-builtin-trap -->

### In-turn bounded waits (coordinator collecting an external run)

- **Marker fire:** `timeout 2160 inotifywait -q -e create -t 2100
  <dir>` as a background task — no sleep token, wakes on marker-file
  creation.
- **Chained holds:** `timeout -k 30 585 tail --pid=$WPID -f /dev/null`
  foreground — each Bash call caps at 600 s, so every wait is at most
  585 s, iterating with a marker check between calls. Resolve the
  watcher PID with `pgrep -x` on the exact binary; never `pgrep -f`
  (self-match; see <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::harness/reference/harness_traps.md" />).
- **Hold the turn alive with real work between checks**:
  a passive wait strands a gate for hours. Pin the exact PID
  (`/usr/bin/ps -p <pid> -o pid,etime,rss --no-headers`); RSS
  oscillation plus fresh go-compiler children means working, static
  RSS means wedged. Interleave legitimate work — report skeleton with
  splice anchor, static verification, SendMessage acknowledgements.
  One gate-scale run at a time: scan the field for live gate
  processes before launching. Harvest verdicts from the
  redirect files only after the pinned PID is gone; verdict
  semantics live in
  <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/verification_evidence.md" />.
<!-- anchor: inotifywait-tail-585 -->
<!-- anchor: rss-oscillation-alive -->

### Waiting on a harness-backgrounded gate

Never call TaskStop on a healthy in-flight gate — waiting is ending
the turn with zero tool calls (three reflex kills in one session each
forced a full re-run). TaskStop is for deliberate
abandonment, with a reason the task should not finish. Gate output is
buffered until exit, so a 0-byte log mid-run is not a wedge signal —
and not a reason to restart either. An older observation that
harness-backgrounded tasks survived a turn end and delivered
notifications is historical; the current truth for phase agents:
<!-- anchor: taskstop-quiet-end -->
<!-- anchor: bg-survival-historical -->

### Phase agents: background gates die at turn end

The main session reaps backgrounded gate tasks belonging to
a phase agent's session when the agent's turn ends ("stopped by main
session" notifications; three reaped gate tracks observed in one
session). Run long gates foreground, one per tool call, inside the
10-minute Bash cap — about 2 minutes on a healthy substrate.
Foreground shape: `timeout -k 10 540 <gate> ...; echo EXIT=$?` — the
external timeout wraps the gate's own bound (double-bound), so the call
never outruns the tool window. Parent-package path targets can drag
whole module and workspace roots into the plan and queue behind the
gate's store lock while subpackage paths drain. On a contended
substrate (lock waits of hours, whole-run caps, settle failures
repeating after one retry): stop attempting and record a sanctioned
substitution — for Go, `GOWORK=off staticcheck` type-check evidence
over the edited prefixes — recorded never-silent.
<!-- anchor: foreground-double-bound -->
<!-- anchor: reaped-at-turn-end -->

### Nested-agent gate resume

A dispatched agent that ends its turn citing a pending background gate
is not reliably woken by its own task notification — the gate can exit
while the agent sits stopped, report unwritten. The parent: (1) locate
the gate PID with a self-match-excluded scan; (2) bounded-wait in-turn
`timeout -k 30 <bound+margin> tail --pid=<PID> -f /dev/null`; (3)
resume the SAME agent via SendMessage with finish-now instructions
(re-read the output file; re-run bounded if it is missing or
truncated). A transient "Connection refused" from a stopped agent is
the same resume shape: SendMessage the same agent, never re-dispatch
fresh.
<!-- anchor: parent-tail-sendmessage -->

### Aliveness is not handler readiness (test helpers)

Neither a fixed sleep margin nor a signal-0 aliveness poll proves a
helper process has installed its signal handlers — the Go runtime
installs them during init, after the process is already signalable
(dropping a "redundant" 100 ms sleep turned a passes-race into a real
failure). Make readiness deterministic: the helper prints a readiness
line after installing handlers; the parent synchronizes on it via
StdoutPipe with a timeout. When a package-var cache forces
helper-binary reuse across tests, PID-suffix the fixed temp names
(t.TempDir would be removed at test end).
<!-- anchor: readiness-line-sync -->
