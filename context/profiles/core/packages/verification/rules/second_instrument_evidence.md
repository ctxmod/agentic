# Second-Instrument Evidence

Output used as verification evidence is re-derived through a second pinned instrument — HEAD blobs via `git show` plus a pinned `diff`, raw output by bypassing the wrapper, cleanup that bypasses interactive aliases, unpiped exit codes — never through a `PATH` wrapper alone.

Test: every output that decides an action or lands in evidence has been re-derived by an instrument that bypasses the wrapper that produced it, and every gate's literal exit code is recorded.

The four canonical forms in full, the incidents, and where host-specific spellings belong: [second_instrument_evidence_detail.md](../reference/second_instrument_evidence_detail.md).
