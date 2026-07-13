# 09 — Move cash and review Account Activity

**What to build:** An Investor can approve exact simulated deposits and withdrawals and ask Benji to review the Brokerage Account’s recent Account Activity.

**Blocked by:** 08 — Create and retrieve a Brokerage Account.

**Status:** ready-for-agent

- [ ] Narrow Paper Trade tools cover deposits, withdrawals, and recent Account Activity.
- [ ] Every deposit and withdrawal uses a fixed Investor Approval that displays the exact USD amount.
- [ ] A parameter change after approval requires a new proposed mutation and approval.
- [ ] Each cash movement persists its own approval state, request parameters, idempotency key, and terminal result.
- [ ] Eve replay and one ambiguous-transport retry reuse the original idempotency key and cannot move cash twice.
- [ ] Insufficient cash, unsupported limits, missing accounts, malformed responses, and service failures are surfaced clearly without guessing.
- [ ] Account Activity is described as recent and never as a complete history because Paper Trade returns at most 100 entries.
- [ ] Account Activity timestamps display in the Investor’s browser timezone.
- [ ] Benji shows concise deposit, withdrawal, and activity-retrieval status without raw request or response payloads.
- [ ] Unit tests cover exact approvals, changed parameters, successful and rejected cash movements, idempotent recovery, recent activity ordering/limits, timezone-ready timestamps, and cross-Investor isolation.
