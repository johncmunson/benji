# 12 — Receive strategy-guided investing guidance

**What to build:** An Investor can receive disciplined, strategy-aware long-term investing guidance, including bounded diversification and rebalancing recommendations that remain within Paper Trade’s supported evidence and approval model.

**Blocked by:** 06 — Safely evolve the Investment Strategy; 10 — Approve an Investor-Directed Market Order; 11 — Review a deterministic Portfolio Valuation.

**Status:** ready-for-agent

- [ ] Benji’s standing instructions distill the agreed _Money, Simplified_ principles without injecting the book, creating a dedicated skill, routinely citing it, or quoting it.
- [ ] Benji defaults to broad diversification, broad-market ETFs, low-cost indexing, disciplined rebalancing, and resistance to market timing.
- [ ] The current Investment Strategy guides and may override generic defaults; unknown dimensions are acknowledged rather than inferred.
- [ ] Benji prefers diversified broad-market ETFs and discusses individual stocks when requested while making concentration risk explicit.
- [ ] Benji limits managed assets to active US-listed stocks and ETFs supported by Paper Trade.
- [ ] A supported ETF may be recommended for exposure to an otherwise unsupported asset class, but Benji clearly distinguishes fund exposure from direct ownership.
- [ ] Volunteered outside assets, debts, taxes, income, or life circumstances may inform advice but are not stored as canonical managed records or presented as a complete financial plan.
- [ ] Benji can identify obvious Position concentration or drift and recommend a rebalance consistent with known strategy information.
- [ ] Rebalancing uses no hidden optimizer, automatic target-weight engine, unattended execution, or batch approval; resulting Market Orders follow the sequential exact-approval flow.
- [ ] Diversification analysis is limited to Position weights and clearly established security characteristics.
- [ ] Benji avoids precise sector, geographic, ETF look-through, current fundamentals, and current news claims without live supporting data.
- [ ] Paper Trade remains the only live market-data source; broader market research is not scaffolded in this ticket.
- [ ] Benji remains useful for general education without a Brokerage Account or Investment Strategy.
- [ ] Benji does not schedule work, monitor while the Investor is away, create files, use subagents, or accept attachments.
- [ ] Representative conversations are manually smoke-tested; model wording is not unit-tested and no automated eval framework is added.
