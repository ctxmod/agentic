# Compaction-Resistant Validation Dispatch — Detail

The full compaction-resistant dispatch rule, for the [validation_dispatch](../rules/validation_dispatch.md) rule.

A validation dispatch survives context continuation by construction — a brief's coordination token is checked before any compulsory read so a continuation self-parks as a cheap duplicate, evidence lands on disk incrementally per check, scope stays narrow, changes queue to check boundaries, hashes are full-form with mandatory post-continuation re-verification — and when report writes are prohibited, findings persist through the sanctioned findings channel (work-item append or schema-stable evidence JSON), never /tmp files or return-message prose.

- the dispatch doctrine and the incremental-evidence pattern — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/deliberate_verification.md" />
- the findings channel and its stable evidence-JSON schema — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/findings_channel.md" />
