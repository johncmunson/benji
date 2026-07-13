# 10 — Approve an Investor-Directed Market Order

**What to build:** An Investor can direct Benji to buy or sell an exact whole-share quantity, inspect the current estimate, approve one Market Order, and receive the Paper Trade fill.

**Blocked by:** 05 — Create a confirmed Investment Strategy; 07 — Research a Tradable Security; 08 — Create and retrieve a Brokerage Account.

**Status:** ready-for-agent

- [ ] The Market Order tool accepts only side, Ticker, and a positive whole-share quantity; it does not accept Investor identity or an idempotency key.
- [ ] Benji validates the Tradable Security and fetches the latest quote before asking for Investor Approval.
- [ ] The fixed approval displays side, Ticker, quantity, quote, quote timestamp, and estimated USD total.
- [ ] Approval authorizes only the exact side, Ticker, and quantity and clearly does not guarantee the displayed fill price.
- [ ] Changing any authorized parameter requires a fresh proposal and approval.
- [ ] One approval authorizes one Market Order; batch approval and atomic multi-order execution are unavailable.
- [ ] A persisted mutation record supplies one stable idempotency key across Eve replay and ambiguous transport recovery.
- [ ] Paper Trade errors such as market closure, unsupported Ticker, insufficient cash, insufficient shares, or limits are presented accurately.
- [ ] Without an Investment Strategy, Benji may execute an explicit Investor-Directed Market Order after approval, lightly suggests defining a strategy, and does not independently choose a security or quantity.
- [ ] With an Investment Strategy, the approved instruction still remains the only authority for execution.
- [ ] Generated UI cannot approve or execute the Market Order.
- [ ] Unit tests cover exact authorization, changed parameters, no-strategy behavior, whole-share validation, quote display, fill-price variation, one-order scope, idempotent recovery, and documented failures.
