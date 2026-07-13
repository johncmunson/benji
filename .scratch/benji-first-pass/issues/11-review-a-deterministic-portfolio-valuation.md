# 11 — Review a deterministic Portfolio Valuation

**What to build:** An Investor can ask Benji for a current Portfolio Valuation whose monetary values and allocations are calculated deterministically from the Brokerage Account and current Paper Trade quotes.

**Blocked by:** 07 — Research a Tradable Security; 08 — Create and retrieve a Brokerage Account; 10 — Approve an Investor-Directed Market Order.

**Status:** ready-for-agent

- [ ] One narrow read operation combines the Brokerage Account with current quotes for every Position.
- [ ] The operation deterministically calculates Available Cash, each Position’s market value and cost basis, realized and unrealized signed gain or loss, total account value, and allocation.
- [ ] All financial arithmetic preserves integer cents and safe quantities; the model does not calculate or correct money values.
- [ ] Cash-only and multi-Position Brokerage Accounts produce valid Portfolio Valuations.
- [ ] A missing or unusable quote fails clearly rather than silently valuing a Position with stale or invented data.
- [ ] Benji can present the valuation using bounded `Card`, `Grid`, `Badge`, and `Table` compositions or concise text.
- [ ] Generated tables remain capped at 100 rows and contain no generated action or approval control.
- [ ] Current realized and unrealized results are distinguished clearly.
- [ ] Benji does not display or imply historical portfolio performance, time-weighted returns, or a portfolio return chart.
- [ ] Unit tests cover cash-only accounts, multiple Positions, signed gains and losses, allocation, rounding boundaries, unavailable quotes, malformed market data, and exact rich-presentation values.
