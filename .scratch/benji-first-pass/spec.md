# Benji First Pass

Status: ready-for-agent

## Problem Statement

An Investor needs a trustworthy, conversational way to manage a simulated Brokerage Account and receive long-term investing guidance without navigating a traditional brokerage dashboard. The current application is only a scaffold: it has no authentication, Chats, Investment Strategy, Paper Trade integration, agent tools, generated presentation, or tests.

The first pass must demonstrate a coherent wealth-management experience rather than isolated AI and brokerage features. Benji must preserve durable Chats, understand an optional Investment Strategy, use a disciplined Investment Philosophy, retrieve trustworthy live data from the existing Paper Trade service, calculate monetary results deterministically, and obtain explicit Investor Approval before changing a Brokerage Account.

## Solution

Build Benji as a Google-authenticated, ChatGPT-lite application for simulated long-term investing. An Investor can maintain multiple durable Chats with Benji, create and fund one simulated Brokerage Account, define an optional Investment Strategy conversationally, inspect current account and market information, and approve exact Brokerage Account changes.

Eve owns durable agent execution and streaming. Benji owns identity, Chat access, application records, approvals, deterministic financial calculations, and side effects. Paper Trade remains a separately deployed service accessed through a typed server-side client. Benji streams normal text and uses one atomic `present_ui` tool for bounded, read-only rich presentation built from a small trusted json-render catalog.

“First pass complete” means every decision in this specification is implemented and verified, not merely that one highlight flow works.

## User Stories

1. As an Investor, I want to sign in with Google, so that my Benji data is associated with a stable identity.
2. As an Investor, I want any Google account to be eligible for sign-in, so that I do not need an invitation or administrator.
3. As an Investor, I want Benji to recognize me without asking me for an Investor identifier, so that identity remains secure and unobtrusive.
4. As an Investor, I want a discreet persistent simulation indicator, so that I always understand that no real money is involved.
5. As a new Investor, I want a welcoming first-use state, so that I understand what Benji can do.
6. As a new Investor, I want clickable starter affordances for creating a Brokerage Account, defining an Investment Strategy, or asking an investing question, so that I can begin quickly.
7. As an Investor, I want a clicked starter affordance to populate rather than immediately submit the composer, so that I can edit it first.
8. As an Investor, I want starter affordances to disappear after my first sent message, so that the active Chat stays focused.
9. As an Investor, I want to create multiple Chats, so that separate investing topics do not become one unwieldy transcript.
10. As an Investor, I want to list and switch between Chats, so that I can resume prior work.
11. As an Investor, I want each Chat title derived from my first message, so that I can recognize it without naming it manually.
12. As an Investor, I want to delete a Chat, so that it disappears permanently from Benji and can no longer be accessed through the application.
13. As an Investor, I want my Brokerage Account and Investment Strategy shared across all my Chats, so that Benji has one consistent understanding of my situation.
14. As an Investor, I want completed Chat content restored after refresh, so that I do not lose prior guidance.
15. As an Investor, I want an in-progress response to reconnect after refresh, so that durable work is not abandoned.
16. As an Investor, I want one active turn at a time in a Chat, so that messages and actions remain ordered.
17. As an Investor, I want the composer disabled while Benji is responding or awaiting my decision, so that I cannot create ambiguous concurrent input.
18. As an Investor, I want concise status messages while Benji uses tools, so that I understand what it is doing without seeing internal reasoning.
19. As an Investor, I want clear inline failures, so that API or agent problems are understandable without exposing internal payloads.
20. As an Investor, I want Benji to answer ordinary investing questions with streamed text, so that simple answers remain conversational.
21. As an Investor, I want rich cards, tables, and charts when they materially clarify an answer, so that structured financial information is easy to understand.
22. As an Investor, I want generated presentation to remain read-only, so that generated controls cannot authorize or execute actions.
23. As an Investor, I want fixed, consistent controls for sensitive decisions, so that Brokerage Account and Investment Strategy changes are never hidden inside generated UI.
24. As an Investor, I want to choose Starting Cash before my Brokerage Account is created, so that Benji does not invent an opening amount.
25. As an Investor, I want to approve the exact account creation before it occurs, so that Benji cannot silently create a Brokerage Account.
26. As an Investor, I want to retrieve my Brokerage Account and recent Account Activity, so that I can understand current cash, Positions, gains or losses, and recent events.
27. As an Investor, I want to deposit a specific USD amount after approving it, so that I control simulated funding.
28. As an Investor, I want to withdraw a specific USD amount after approving it, so that I control simulated cash removal.
29. As an Investor, I want Benji to look up a Tradable Security, so that I can verify a Ticker and listing before acting.
30. As an Investor, I want Benji to retrieve a current quote with its timestamp, so that prices are presented as time-sensitive information.
31. As an Investor, I want Benji to retrieve daily historical prices for an inclusive date range, so that I can review a security’s price history.
32. As an Investor, I want a simple closing-price chart, so that historical movement is legible without technical-analysis clutter.
33. As an Investor, I want to place an Investor-Directed Market Order without first defining an Investment Strategy, so that strategy setup does not block an explicit instruction.
34. As an Investor without an Investment Strategy, I want Benji to lightly suggest defining one, so that I understand the benefit without being obstructed.
35. As an Investor without an Investment Strategy, I want Benji to avoid independently choosing a security or quantity for me, so that it does not make unsupported personalized recommendations.
36. As an Investor, I want every Market Order approval to show side, Ticker, whole-share quantity, latest quote, quote timestamp, and estimated total, so that I understand the exact instruction.
37. As an Investor, I want Market Order approval to authorize only side, Ticker, and quantity rather than guarantee a fill price, so that current-quote execution is represented honestly.
38. As an Investor, I want a changed Market Order to require a fresh approval, so that Benji cannot alter an approved instruction.
39. As an Investor, I want each Market Order approved separately, so that a non-atomic rebalance is not disguised as one atomic action.
40. As an Investor, I want ambiguous mutation retries to reuse the same idempotency key, so that a network interruption cannot duplicate an action.
41. As an Investor, I want one optional current Investment Strategy, so that Benji can tailor recommendations without forcing setup.
42. As an Investor, I want my Investment Strategy to record investment goals, time horizon, risk tolerance, liquidity needs, and preferences or restrictions, so that advice has a durable north star.
43. As an Investor, I want unknown Investment Strategy dimensions to remain explicitly unknown, so that Benji does not invent personal facts.
44. As an Investor, I want an incomplete Investment Strategy to remain useful, so that I can develop it gradually.
45. As an Investor, I want Benji to draft an initial Investment Strategy for my confirmation, so that I can review its interpretation before it becomes current.
46. As an Investor, I want Material Strategy Changes presented as before-and-after fields plus the resulting complete Investment Strategy, so that I understand their consequences.
47. As an Investor, I want a Material Strategy Change to apply only after I confirm the exact proposal, so that assumptions or advice-altering changes do not silently become authoritative.
48. As an Investor, I want an outdated Material Strategy Change rejected if another Chat changed the strategy first, so that newer decisions cannot be overwritten.
49. As an Investor, I want clear Strategy Clarifications applied immediately and then disclosed, so that harmless wording corrections do not create unnecessary friction.
50. As an Investor, I want Benji’s default guidance grounded in broad diversification, broad-market ETFs, low-cost indexing, disciplined rebalancing, and resistance to market timing, so that advice supports long-term investing.
51. As an Investor, I want my Investment Strategy to override generic defaults where appropriate, so that Benji respects my confirmed circumstances and restrictions.
52. As an Investor, I want Benji to prefer diversified broad-market ETFs by default, so that concentrated stock picking is not its baseline behavior.
53. As an Investor, I want individual-stock concentration risk made explicit when I request individual stocks, so that I understand the trade-off.
54. As an Investor, I want supported ETFs to provide exposure to asset classes Paper Trade cannot hold directly, so that the Tradable Security universe can still support diversification.
55. As an Investor, I want Benji to distinguish ETF exposure from direct ownership of an unsupported asset, so that its language remains accurate.
56. As an Investor, I want a current Portfolio Valuation calculated from my account and current quotes, so that I can see cash, Position values, cost basis, gains or losses, and allocation.
57. As an Investor, I want financial arithmetic calculated deterministically rather than by the model, so that displayed values are trustworthy.
58. As an Investor, I want realized and unrealized gain or loss distinguished, so that I understand completed and current outcomes.
59. As an Investor, I want Benji to avoid claiming historical portfolio performance it cannot calculate, so that a chart is not fabricated from incomplete Account Activity.
60. As an Investor, I want Benji to identify obvious Position concentration or drift, so that I can discuss diversification and rebalancing.
61. As an Investor, I want rebalancing recommendations without a hidden optimizer or automatic target-weight engine, so that Benji’s reasoning remains understandable and within available data.
62. As an Investor, I want Benji to avoid precise sector, geographic, or ETF look-through claims without live supporting data, so that diversification analysis does not overstate its evidence.
63. As an Investor, I want Paper Trade to be the only initial live market-data source, so that current claims come from one trusted contract.
64. As an Investor, I want Benji to validate Tickers and prices through Paper Trade, so that stale model knowledge does not become current market data.
65. As an Investor, I want Benji to avoid current fundamentals or news claims that Paper Trade cannot support, so that unavailable research is not implied.
66. As an Investor, I want general educational guidance even when no Brokerage Account or Investment Strategy exists, so that setup does not block learning.
67. As an Investor, I want Benji to manage only my simulated Brokerage Account, so that it does not imply it maintains a complete financial plan.
68. As an Investor, I want outside assets, debts, taxes, income, or life circumstances to inform advice only when I volunteer them, so that they are not treated as canonical managed records.
69. As an Investor, I want Benji to act only during an Investor-initiated Chat, so that it does not monitor or change anything while I am away.
70. As an Investor, I want a calm, concise, plainspoken experience, so that financial guidance is understandable rather than promotional or overloaded with disclaimers.
71. As an Investor, I want Benji to state uncertainty and ask only necessary clarifying questions, so that it does not pretend to know missing facts.
72. As an Investor, I want monetary values shown consistently in USD, so that balances and estimates are unambiguous.
73. As an Investor, I want quote and Account Activity timestamps shown in my browser timezone, so that event times are locally meaningful.
74. As an Investor, I want daily price dates preserved as market calendar dates, so that timezone conversion does not shift them.
75. As an Investor on a narrow screen, I want the Chat sidebar to collapse into a sheet, so that the transcript and composer remain usable.
76. As an Investor, I want Benji to follow my system light or dark preference, so that it fits my environment without another setting.
77. As an Investor, I want my Chats, Investment Strategy, and Brokerage Account operations isolated from other Investors, so that another signed-in person cannot access them.
78. As an Investor, I want deleted Chat access revoked even if Eve retains internal workflow data, so that Benji does not falsely promise immediate third-party erasure.
79. As an Investor, I want Benji to work in both development and production, so that the take-home can be built locally and demonstrated on Vercel.

## Implementation Decisions

1. **Domain identity:** The authenticated person is the Investor. “User” remains an authentication implementation term and is not used as domain language. Better Auth’s stable user ID is used directly as Paper Trade’s opaque `investorId`; there is no separate Investor table or mapping.
2. **Authentication:** Better Auth supports Google OAuth only. Any Google account may sign in. Email/password, other OAuth providers, allowlists, invitations, roles, organizations, and administration are excluded.
3. **Runtime environments:** Benji must operate in development and production. Production runs on Vercel with Neon Postgres. There is no staging environment or alternate hosting target.
4. **Model:** Eve uses `openai/gpt-5.6-sol` through AI Gateway with medium reasoning effort. The first pass has one model and no fallback or routing policy.
5. **Agent voice:** Benji is calm, concise, plainspoken, transparent about uncertainty, and sparing with clarifying questions and disclaimers.
6. **Investment Philosophy:** Distill the relevant principles of _Money, Simplified_ into Benji’s standing system prompt. Do not inject the full book, create a book skill, routinely cite it, or quote it. Mention the source only when asked about Benji’s philosophy.
7. **Investment Philosophy scope:** Benji favors long-term broad diversification, broad-market ETFs, low-cost indexing, disciplined rebalancing, and resistance to market timing. Individual stocks are considered when requested with concentration risk disclosed. The philosophy guides but does not override an Investment Strategy.
8. **Managed scope:** Benji manages only the simulated Brokerage Account. Volunteered outside circumstances can inform advice but are not canonical records and do not become a complete financial plan.
9. **Activity model:** Benji acts only within Investor-initiated Chats. No schedules, background monitoring, proactive notifications, or unattended rebalancing run in the first pass.
10. **Eve boundary:** Eve owns durable multi-turn execution, model and tool loops, streaming, pauses, and replay. The application does not recreate an agent loop or hold process-local Chat state.
11. **Application authority:** Better Auth identity, Chat ownership, the Investment Strategy, transcript projection, approvals, idempotency, and side effects remain application-owned.
12. **Eve access:** The browser reaches Eve through a thin same-origin channel. Every create, continue, stream, and approval operation authenticates with Better Auth and checks Chat ownership while preserving Eve JSON/NDJSON unchanged.
13. **Continuation security:** Eve continuation tokens remain server-side, are encrypted before persistence, and never appear in browser-visible records or logs.
14. **Chat model:** An Investor may own multiple Chats. Each Chat maps to one durable Eve execution context; all Chats share the Investor’s Brokerage Account and Investment Strategy.
15. **Chat lifecycle:** Support create, list, switch, and delete. Omit manual rename, search, folders, sharing, and archival.
16. **Chat titles:** Derive a short title deterministically from the trimmed first Investor message. Do not make a separate model call.
17. **Chat persistence:** Persist a lossless ordered projection of Eve events plus the complete Eve cursor needed for resume. Save stream progress while running, restore completed content after refresh, and reconnect active work from the saved stream index.
18. **Chat deletion:** Deleting a Chat removes Benji-owned transcript and Chat records and permanently revokes application access to the Eve execution context. The product does not promise immediate erasure of Eve workflow history or sandbox snapshots.
19. **Turn ordering:** Permit one active turn per Chat. Disable the composer while Benji is running or waiting for an Investor decision. Do not provide a message queue.
20. **Cancellation:** Omit “Stop generating” because Eve cannot guarantee immediate backend cancellation; a disconnect must not be presented as cancellation.
21. **Workspace and subagents:** Do not depend on an Eve filesystem workspace and do not add subagents. Canonical state belongs in Postgres or Paper Trade. A research subagent can be reconsidered when broader market research is added.
22. **Attachments:** Support text input only. File uploads, statement imports, images, and multimodal analysis are excluded.
23. **Application tables:** In addition to Better Auth’s schema, add `investmentStrategies`, `chats`, `chatEvents`, and `accountMutations`. There is no separate Investor table.
24. **Investment Strategy storage:** Store one row per Investor containing one JSON object with exactly five concise free-text fields: investment goals, time horizon, risk tolerance, liquidity needs, and preferences or restrictions. Avoid enums, normalized goal/allocation tables, and agent-defined keys.
25. **Incomplete strategy:** Each Investment Strategy field may be explicitly unknown. Benji may advise from confirmed information while disclosing when missing information materially limits advice.
26. **Strategy authority:** The Investment Strategy is optional. An initial draft becomes current only after Investor confirmation.
27. **Material changes:** A proposed change is material when it could alter Benji’s recommendations or depends on an assumption. Present changed fields, before-and-after values, and the resulting complete Investment Strategy. Apply only the exact confirmed proposal.
28. **Strategy Clarifications:** An explicit change that preserves strategy meaning may be persisted immediately, followed by a notification describing what changed.
29. **Strategy concurrency:** Version the Investment Strategy. A Material Strategy Change records its base version and fails closed if the strategy changed before confirmation; Benji must produce a fresh proposal.
30. **No-strategy behavior:** An Investor-Directed Market Order may proceed without an Investment Strategy after normal approval. Benji lightly suggests defining a strategy. Benji must not independently choose the security or quantity in this state.
31. **Paper Trade boundary:** Paper Trade is an existing external service in another repository. Benji does not implement its API. The supplied OpenAPI specification is authoritative.
32. **Paper Trade configuration:** Require a distinct Paper Trade base URL in development and production. Keep the shared service credential server-only and never infer the service URL from the incoming Benji request.
33. **Paper Trade client:** Use a small handwritten `fetch` client. Validate consumed response shapes with Zod, normalize documented errors, send the bearer credential, and avoid OpenAPI client-generation dependencies.
34. **Paper Trade operations:** Expose typed tools for account creation and retrieval, deposits, withdrawals, recent Account Activity, Market Orders, Tradable Security lookup, quotes, and daily historical prices. `HEAD` and `OPTIONS` are protocol details and are not agent tools.
35. **Tool shape:** Give Benji narrow typed tools rather than raw HTTP or a generic request tool. Tool inputs never accept `investorId`; server-side authenticated context derives the target Investor.
36. **Database access:** Benji receives no SQL or generic database tool. It uses narrow Investment Strategy capabilities to read the current strategy, apply a Strategy Clarification, or propose a Material Strategy Change.
37. **Account creation:** Do not create a Brokerage Account silently. When one is needed, ask for Starting Cash and use a fixed account-creation approval before calling Paper Trade. Investment Strategy setup remains optional.
38. **Brokerage Account authority:** Every account creation, deposit, withdrawal, and Market Order requires a fixed application-owned Investor Approval. Generated UI cannot provide approval.
39. **Market Order approval:** Display side, Ticker, whole-share quantity, latest quote and timestamp, and estimated USD total. Approval authorizes only the exact side, Ticker, and quantity, not a guaranteed fill price. Any parameter change requires a new approval.
40. **Sequential Market Orders:** One Investor Approval authorizes one Market Order. Benji may explain a complete rebalance, but each resulting instruction is proposed and approved separately because Paper Trade has no atomic batch operation.
41. **Mutation records:** Persist proposed business parameters, approval state, generated idempotency key, and terminal result for each Brokerage Account mutation.
42. **Idempotency:** The application generates the idempotency key; the model and browser cannot supply or alter it. Replay of the same Eve action reuses the persisted key.
43. **Retry behavior:** Do not retry ordinary Paper Trade errors. If a mutation has an ambiguous transport interruption, retry once with the same idempotency key to recover Paper Trade’s retained result; otherwise surface the failure without guessing.
44. **Market data:** Paper Trade is the sole first-pass live market-data source. Validate every Ticker and current price through Paper Trade. Do not claim current fundamentals, news, ETF holdings, sectors, or geographic exposures without supporting live data.
45. **Future research:** Broader market research is a likely follow-up immediately after the first pass, but it is not required here and must not be scaffolded speculatively.
46. **Tradable Security universe:** Benji can manage active US-listed stocks and ETFs accepted by Paper Trade. It cannot manage individual bonds, direct real estate, private investments, or other unsupported assets. A supported ETF may provide exposure to an otherwise unsupported asset class, with the distinction stated clearly.
47. **Portfolio Valuation:** Add one deterministic read operation that combines the Brokerage Account with current quotes and computes cash, Position market values, total cost basis, realized and unrealized gain or loss, total value, and allocation. The model does not perform financial arithmetic.
48. **Performance claims:** Do not implement or imply historical portfolio performance, time-weighted returns, or account return charts. Paper Trade lacks historical account valuations and exposes only the 100 most recent Account Activity entries.
49. **Rebalancing:** Benji may identify Position concentration or drift and recommend changes consistent with the Investment Strategy. Do not build an allocation optimizer, automatic target-weight engine, or unattended rebalancer.
50. **Diversification claims:** Initially assess diversification from Position weights and clearly established security characteristics only. Avoid precise sector, geographic, or ETF look-through claims.
51. **Currency and quantities:** Preserve monetary values as safe integer cents internally and format them as USD for display. Market Orders use positive whole shares only.
52. **Time display:** Show quote and Account Activity timestamps in the browser timezone. Preserve daily historical-price dates as market calendar dates without timezone conversion.
53. **Generated presentation seam:** The sole Eve/json-render integration is `present_ui`, whose typed input contains one complete JSON-serializable spec. The tool has no domain side effect and returns only a small acknowledgement.
54. **Atomic rendering:** Render generated UI only after a successful `present_ui` result. Validate the spec again at the browser presentation boundary and fail closed with a fixed fallback for malformed or unknown content.
55. **Text first:** Ordinary streamed text is valid and preferred for simple responses. Benji calls `present_ui` only when structured presentation materially helps.
56. **Catalog:** Select only json-render/shadcn `Card`, `Stack`, `Grid`, `Heading`, `Text`, `Badge`, and `Table`, plus one custom `PriceChart`. Keep server-safe catalog definitions separate from React implementations.
57. **Generated UI interaction:** Exclude generated forms, buttons, links, external URLs, server actions, authorization controls, and approvals. Generated UI is read-only in the first pass.
58. **Presentation bounds:** Cap generated tables at 100 rows and chart specs at 500 points. Deterministically downsample displayed chart data while preserving the first and last points; do not place larger raw datasets in generated specs.
59. **PriceChart:** Plot daily closing price only with clear USD and market-date axes for the requested range. Omit candlesticks, volume, technical indicators, and client-side range controls.
60. **Fixed controls:** Account mutation approvals, Material Strategy Change confirmations, authorization, retry/status controls, and errors use application-owned shadcn/ui components.
61. **Primary layout:** Build a chat-centric interface with a Chat sidebar, transcript, fixed approvals, and composer. There is no separate portfolio dashboard or Investment Strategy editor; Benji presents those views inside a Chat.
62. **Welcome state:** Before the first message, show editable click-to-populate starter affordances for account creation, strategy definition, and a general investing question. Remove them after the first message is sent.
63. **Simulation disclosure:** Keep a persistent but discreet “Simulated investing” indicator. Do not add lengthy legal disclaimers, suitability attestations, or compliance workflows.
64. **Responsive behavior:** Design desktop-first. Collapse the Chat sidebar into a sheet on narrow screens while preserving transcript, approval, and composer usability. Do not build a separately optimized mobile application.
65. **Theme:** Follow the operating system light/dark preference with no manual override.
66. **Tool visibility:** Show concise operation statuses and understandable failures. Do not expose chain-of-thought, raw tool inputs or outputs, credentials, continuation tokens, or unrestricted Eve events.
67. **Observability:** Emit structured server logs containing sanitized Chat, Eve run, tool, duration, status, and error metadata. Use Eve and AI Gateway dashboards for traces and usage. Do not build a custom analytics pipeline or admin UI.
68. **Data sensitivity:** Never log shared service credentials, Google credentials, continuation tokens, unrestricted generated specs, or sensitive tool output.
69. **Chat UI libraries:** Use installed shadcn/ui components as the UI foundation without modifying foundational components.
70. **First-pass acceptance:** Completion means all behavior and constraints in this specification work in development and production and are covered by the agreed verification posture, not only the highlighted sign-in/account/strategy/order flow.

## Testing Decisions

1. Tests assert externally observable behavior at stable boundaries rather than private helpers, React implementation details, framework internals, model wording, or database query shape.
2. The primary unit-test seam is the authenticated Benji operation layer invoked by Eve tools. With Paper Trade and database boundaries mocked, tests cover derived Investor identity, Investment Strategy lifecycle rules, version conflicts, approvals, mutation records, idempotency reuse, and Portfolio Valuation output.
3. The Paper Trade client is a separate contract seam tested with Mock Service Worker. Tests cover URL and method construction, bearer authentication, request normalization, success-response validation, documented errors, malformed responses, server-only identity derivation, and one same-key retry after ambiguous mutation transport failure.
4. The Eve access channel is tested as an authorization seam with Better Auth and Chat persistence mocked. Tests prove that create, continue, stream, reconnect, delete, and approval operations allow the owner and reject another authenticated Investor.
5. The presentation adapter is tested as the browser rendering seam. Tests prove that only a successful `present_ui` result renders, malformed or unknown specs fail closed, read-only catalog constraints hold, tables are bounded to 100 rows, charts are bounded to 500 points, and downsampling preserves endpoints.
6. Deterministic Portfolio Valuation tests cover cash-only accounts, multiple Positions, current market values, average cost basis, realized and unrealized signed gains or losses, allocation, integer-cent arithmetic, and unavailable quote failures.
7. Investment Strategy tests cover initial confirmation, incomplete fields, explicit unknowns, immediate Strategy Clarifications with notification, Material Strategy Changes, inferred changes requiring confirmation, exact proposal application, and stale base-version rejection.
8. Mutation tests cover account creation, deposits, withdrawals, and Market Orders as separate approved operations; changed parameters cannot reuse approval; retries reuse the original idempotency key; terminal results are persisted.
9. Chat tests cover deterministic first-message titles, multiple Chats sharing one Investment Strategy, one active turn per Chat, ordered idempotent event projection, cursor updates during streaming, refresh hydration, active reconnect, and access revocation on deletion.
10. UI unit coverage focuses on fixed approval contents and behavior, welcome affordances populating the composer and disappearing after first send, composer disabling, simulation disclosure, tool status/error rendering, system theme behavior, and narrow-screen sidebar accessibility where these can be tested without duplicating framework behavior.
11. No current repository test prior art exists; the application is a scaffold. New tests establish the above boundaries without introducing broad fixtures or an elaborate testing framework.
12. Model calls remain out of unit tests. No automated agent-evaluation framework is added.
13. Manual agent-browser smoke testing uses the authenticated profile and covers Google sign-in, Chat creation, approved Brokerage Account creation, confirmed Investment Strategy creation, a quote and `PriceChart`, an approved Market Order, refreshed Portfolio Valuation, Chat refresh/reconnect, Chat switching, and Chat deletion.
14. Local checks include unit tests, linting, type checking, and a production build. The same core smoke story is verified against the Vercel production deployment.

## Out of Scope

- Implementing or modifying the external Paper Trade API.
- Real-money accounts, transfers, custody, or brokerage connectivity.
- Individual bonds, direct real estate, private equity, private lending, commodities, or other assets unsupported by Paper Trade; supported ETFs that provide exposure remain in scope.
- Complete net-worth, tax, estate, insurance, retirement-income, liability, or holistic financial-plan records.
- Broader market research, fundamentals, current news, security screening, ETF holdings analysis, sector analysis, geographic look-through, or a research provider.
- Background monitoring, schedules, alerts, notifications, unattended rebalancing, or automatic Brokerage Account changes.
- Allocation optimization, automatic target weights, tax-loss harvesting automation, or multi-order atomic execution.
- Historical Portfolio Valuation, portfolio performance charts, time-weighted returns, or reconstructed full Account Activity beyond Paper Trade’s recent 100 entries.
- Subagents, persistent filesystem workflows, uploaded files, statements, images, or multimodal input.
- Progressive json-render patches, arbitrary generated HTML or code, generated forms, generated approvals, generated server actions, or generated external links.
- A separate portfolio dashboard, separate Investment Strategy editor, or non-chat product surfaces beyond authentication and fixed Chat controls.
- Email/password authentication, non-Google OAuth providers, account invitations, roles, organizations, administration, allowlists, or tenant management.
- Manual Chat titles, search, folders, sharing, archival, or transcript export.
- A reliable backend cancellation control, queued turns, or concurrent turns within one Chat.
- A manual theme override or a separately optimized mobile application.
- Staging, alternate cloud hosting, self-hosted Eve workflow infrastructure, or non-Neon production storage.
- Rate limiting, hardened compliance workflows, extensive legal disclosures, custom analytics infrastructure, or an admin observability UI.
- OpenAPI client generation, direct model access to HTTP, raw SQL tools, or generic database tools.
- Multi-model routing, provider fallback, automated model evaluations, or live-model unit tests.

## Further Notes

- The current codebase has Eve, Next.js, React, Drizzle, Neon/Postgres support, Zod, Vitest, MSW, Tailwind, and shadcn/ui foundations, but the application page is still a placeholder and the domain schema and test suite are empty.
- Better Auth and the required json-render/AI packages are not currently declared and will need to be added at the minimum versions compatible with the installed stack.
- Required server configuration includes Google OAuth credentials, Better Auth secrets, AI Gateway authentication, the Paper Trade base URL and shared credential, database URLs, and a continuation-token encryption secret. Environment validation must distinguish development, test, and production without introducing staging behavior.
- Database schema changes must be expressed through Drizzle schema definitions and generated migrations rather than handwritten migrations.
- The Paper Trade OpenAPI specification is the source of truth for endpoint paths, normalized requests, error codes, safe-integer constraints, idempotency semantics, whole-share quantities, and timestamp/date formats.
- The existing architecture decision establishing Eve as execution engine and json-render as generated presentation remains authoritative.
- Advanced research is the expected next capability after this first pass, but the first-pass interfaces should not be generalized or scaffolded for it prematurely.
