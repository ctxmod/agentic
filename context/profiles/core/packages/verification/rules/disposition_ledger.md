# Verdicts End With a Disposition Ledger

Every validation verdict ends with a disposition table (finding → owner/story → deferred-until); enforcement-weakening deferrals carry interim mitigations; a scheduled sweep consumes the ledger, and items without dispositions block closure.

Test: a story cannot close while a finding from its verdicts lacks a disposition.

The full rule, the incidents behind it, and the doctrine and findings-channel pointers: [disposition_ledger_detail.md](../reference/disposition_ledger_detail.md).
