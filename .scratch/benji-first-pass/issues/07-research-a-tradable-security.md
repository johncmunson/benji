# 07 — Research a Tradable Security

**What to build:** An Investor can ask Benji to validate a Tradable Security, retrieve its current quote and daily historical prices from Paper Trade, and receive bounded text or rich presentation.

**Blocked by:** 05 — Create a confirmed Investment Strategy.

**Status:** ready-for-agent

- [ ] Paper Trade is treated as an existing external service whose supplied OpenAPI contract is authoritative; no Paper Trade endpoint is implemented in Benji.
- [ ] Development and production require a Paper Trade base URL and a server-only shared credential; the URL is not inferred from the Benji request origin.
- [ ] A small handwritten client uses `fetch`, bearer authentication, and Zod validation rather than generated-client dependencies.
- [ ] Narrow typed tools cover Tradable Security lookup, current quote, and inclusive-range daily historical prices.
- [ ] Benji validates Tickers and current prices through Paper Trade and handles documented and malformed responses without exposing provider details.
- [ ] Ordinary Paper Trade errors are not retried automatically and are presented clearly in the Chat.
- [ ] Quote timestamps display in the Investor’s browser timezone; daily prices remain unshifted market calendar dates.
- [ ] Benji may answer with text alone or call `present_ui` when structured presentation materially helps.
- [ ] The custom `PriceChart` plots daily closing price only with clear USD and market-date axes.
- [ ] Generated charts are capped at 500 points and deterministic display downsampling preserves the first and last points.
- [ ] Generated tables are capped at 100 rows, and larger raw responses never enter a generated spec.
- [ ] Candlesticks, volume overlays, technical indicators, client-side range controls, generated links, and generated actions are absent.
- [ ] Tool status is concise, and no claim is made about unavailable current fundamentals, news, sectors, geography, or ETF holdings.
- [ ] MSW-backed unit tests cover request construction, bearer authentication, normalization, success validation, documented errors, malformed responses, date handling, chart bounds, and endpoint-preserving downsampling.
