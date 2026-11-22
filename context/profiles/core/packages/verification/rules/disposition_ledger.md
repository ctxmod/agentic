# Verdicts End With a Disposition Ledger

Every validation verdict ends with a disposition table (finding → owner/story → deferred-until); enforcement-weakening deferrals carry interim mitigations; a scheduled sweep consumes the ledger, and items without dispositions block closure.

Every validation verdict ends with a disposition table — one row per finding carrying its disposition (acted or deferred), the owning agent or story, and for deferrals the deferred-until date — because verdicts that stop at recommendations let correct findings decay: the final-audit findings F5/F7/F8/F9 got zero remediation traffic by session end, a deferred freshness acceptance criterion became a live enforcement bypass within four days, and a fleet-wide test-scan exemption sits without owner or expiry. A deferral that weakens enforcement names the interim mitigation holding the risk down until the item lands, and a scheduled sweep consumes the ledger — a story cannot close while a finding from its verdicts lacks a disposition.

- the doctrine with the sweep trigger, the closure escape, and the F5/F7/F8/F9 worked example — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/deliberate_verification.md" />
- the findings channel whose status enum (open/acted/deferred) the table consumes — <ctxmod_path path="ctx:github.com/ctxmod/agentic/context::shared::evidence/reference/findings_channel.md" />
