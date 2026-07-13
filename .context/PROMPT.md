We're scoping out Benji, an agentic chatbot optimized for long-term investing. Benji, along with the associated Paper Trade API that Benji will integrate with, are part of a take-home project I'm working on for a company that I'm interviewing with.

Therefore, we do not need to concern ourselves with advanced topics like rate limiting, hardened security, or things of that nature. We can focus on the happy path and avoid over-engineering for the edge cases. With that said, we still want clean, organized code.

Benji will run in the browser and offer a UX that could be described as ChatGPT-lite. **Eve as the durable agent execution engine** and **json-render as the generated presentation layer**.

It may be the case that we do not deterministically or programmatically call the Paper Trade API in all scenarios. Rather, it is likely that the agent will be performing this. Furthermore, the agent may even interact with our own database. The agent will be given access to skills and tools allowing it to carry out it's work and behave as a wealth manager and advisor.

The agent should have a table in the database where it can save semi-structured portfolio strategy notes for each user or investor. These notes will be the north star guiding operations such as portfolio rebalancing, diversification, etc.

Tech Stack:

- Next.js
- Tailwind + shadcn/ui
- Drizzle ORM + Neon Postgres
- json-render + eve
- AI SDK + AI Gateway
- Vitest + Mock Service Worker + agent-browser

## Paper Trade API

Paper Trade is a private, synchronous server-to-server API for simulated long-term investing. Benji owns users, authentication, strategy, and presentation; Paper Trade receives an opaque `investorId` and owns one USD brokerage account per investor, including cash, positions, fills, realized gain/loss, and immutable activity.

### Capabilities and API surface

**Brokerage accounts**

| Method | Endpoint                                            | Purpose                                                  |
| ------ | --------------------------------------------------- | -------------------------------------------------------- |
| `POST` | `/api/investors/{investorId}/account`               | Create an account with `startingCashCents`               |
| `GET`  | `/api/investors/{investorId}/account`               | Read cash, positions, cost basis, and realized gain/loss |
| `POST` | `/api/investors/{investorId}/account/deposits`      | Deposit `amountCents` immediately                        |
| `POST` | `/api/investors/{investorId}/account/withdrawals`   | Withdraw `amountCents` if cash is available              |
| `POST` | `/api/investors/{investorId}/account/market-orders` | Submit `{ side, ticker, quantity }`                      |
| `GET`  | `/api/investors/{investorId}/account/activities`    | Read bounded, newest-first account activity              |

Market orders support whole-share buys and sells of supported US-listed stocks and ETFs. They fill completely and synchronously at the latest usable Financial Datasets price—no pending orders, partial fills, margin, shorting, fees, or settlement delay.

**Market data**

| Method | Endpoint                                        | Purpose                                 |
| ------ | ----------------------------------------------- | --------------------------------------- |
| `GET`  | `/api/securities/{ticker}`                      | Exact, case-insensitive security lookup |
| `GET`  | `/api/securities/{ticker}/quote`                | Current price snapshot                  |
| `GET`  | `/api/securities/{ticker}/prices?start=…&end=…` | Daily historical OHLCV                  |

Market data is fetched on demand and is not persisted. Durable account reads therefore remain available if the market-data provider is unavailable, but valuation requires separate quote requests.

### Integration expectations

- Call only from the Benji backend using the shared bearer credential; secrets must never reach browsers.
- Use Benji’s opaque `investorId` as the account identifier. There is no separate Paper Trade account ID.
- Send an `Idempotency-Key` header on every mutation. Retrying the same key and payload returns the original result; changing the payload returns `409`.
- Represent all money and prices as safe integer fields ending in `Cents`.
- Expect atomic mutations and concurrency protection against overspending or overselling.
- Expect trading only from 9:30 AM–4:00 PM America/New_York, Monday–Friday, subject to a development-only always-open override.
- Handle errors as `{ "error": { "code": string, "message": string } }`, with stable codes and conventional `400`, `401`, `404`, `409`, `422`, and `503` statuses.
- Treat `503` market-data failures as transient and retry safely with the same idempotency key.
