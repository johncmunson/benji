# 03 — Reconnect an active Chat

**What to build:** An Investor can refresh while Benji is still working and reconnect to the same active turn without reordering, duplicating, or silently abandoning work.

**Blocked by:** 02 — Hold a durable Chat with Benji.

**Status:** ready-for-agent

- [ ] Stream cursor progress is saved during execution rather than only when a turn finishes.
- [ ] Refreshing an active Chat resumes Eve streaming from the saved index.
- [ ] Replayed or duplicated Eve events produce one ordered transcript entry each.
- [ ] The interface distinguishes an interrupted browser stream from a failed Eve run.
- [ ] Only one turn may be active within a Chat.
- [ ] The composer is disabled while Benji is running or awaiting an Investor decision, and no message queue is offered.
- [ ] No “Stop generating” control is shown because disconnecting cannot guarantee backend cancellation.
- [ ] Concise running and failure states remain visible before and after reconnect without exposing internal reasoning.
- [ ] Unit tests cover active cursor persistence, reconnect start position, idempotent event projection, active-state composer behavior, and failure/disconnect distinctions.
