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

Paper Trade maintains one simulated Brokerage Account per Wealth Manager
Investor and exposes on-demand Tradable Security data.

## Authentication

All business operations require the opaque shared service credential in an
`Authorization: Bearer <credential>` header. The credential is not a JWT.
`OPTIONS` and unsupported-method responses do not authenticate.

## Money and quantities

Monetary fields ending in `Cents` are integer cents. Share quantities are
positive whole shares. Request values are restricted to JavaScript safe
integers where stated.

## Idempotency

Every account mutation requires `Idempotency-Key`. Keys are scoped to the
exact Investor and retained with terminal results. Repeating the same key
and normalized request replays the original status and body; reusing it for
a different request returns `409 idempotency_conflict`.

## Method handling

Documented `HEAD` operations are provided by Next.js by invoking `GET` and
suppressing the body. Unsupported exported methods return `405` with
`{"error":{"code":"method_not_allowed","message":"Method not allowed."}}`
and the same `Allow` value shown by the path's `OPTIONS` operation. These
rejecting methods are intentionally omitted as OpenAPI operations.

`OPTIONS` responses expose `Allow` only; they are not complete CORS
preflight responses and emit no `Access-Control-Allow-*` headers.

## Endpoints

- `POST /api/investors/{investorId}/account` — Create a Brokerage Account.
- `GET /api/investors/{investorId}/account` — Retrieve an account and its Positions.
- `POST /api/investors/{investorId}/account/deposits` — Deposit cash into an account.
- `POST /api/investors/{investorId}/account/withdrawals` — Withdraw available cash.
- `GET /api/investors/{investorId}/account/activities` — List recent Account Activity.
- `POST /api/investors/{investorId}/account/market-orders` — Execute a simulated Buy or Sell.
- `GET /api/securities/{ticker}` — Look up a Tradable Security.
- `GET /api/securities/{ticker}/quote` — Fetch the current quote.
- `GET /api/securities/{ticker}/prices` — Fetch daily prices for an inclusive date range.

## OpenAPI Specification

[OpenAPI Specification](paper-trade.openapi.yaml)
