# 05 — Create a confirmed Investment Strategy

**What to build:** An Investor can conversationally draft, review, confirm, and later retrieve one optional Investment Strategy, including explicitly unknown dimensions.

**Blocked by:** 02 — Hold a durable Chat with Benji.

**Status:** ready-for-agent

- [ ] Each Investor may have at most one current Investment Strategy shared by all Chats.
- [ ] The strategy stores exactly five concise free-text dimensions: investment goals, time horizon, risk tolerance, liquidity needs, and preferences or restrictions.
- [ ] Any dimension may remain explicitly unknown; Benji does not invent missing values.
- [ ] Benji can provide general education before a strategy exists and can create a partially complete strategy.
- [ ] Benji accesses strategy data through narrow typed capabilities and receives no generic SQL or database tool.
- [ ] An initial strategy remains a draft until the Investor confirms it through a fixed application-owned control.
- [ ] After confirmation, the strategy is current and can be retrieved from any Chat.
- [ ] The first atomic `present_ui` seam accepts one complete spec, performs no domain mutation, and returns only a small acknowledgement.
- [ ] Read-only strategy summaries use only the approved json-render/shadcn catalog primitives.
- [ ] Generated presentation renders only after successful tool completion, is validated again at the presentation boundary, and fails closed through a fixed fallback.
- [ ] Generated forms, links, approvals, arbitrary URLs, server actions, and external code are unavailable.
- [ ] There is no separate Investment Strategy editor; creation and review happen inside a Chat.
- [ ] Unit tests cover optional and incomplete strategies, explicit unknowns, initial fixed confirmation, cross-Chat retrieval, successful atomic presentation, and malformed or unknown spec rejection.
