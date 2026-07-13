# 13 — Operate and verify the complete first pass

**What to build:** The complete agreed Benji first pass runs reliably in development and on Vercel production, with sanitized observability and verification of every preceding slice.

**Blocked by:** 03 — Reconnect an active Chat; 04 — Manage multiple Chats; 06 — Safely evolve the Investment Strategy; 09 — Move cash and review Account Activity; 12 — Receive strategy-guided investing guidance.

**Status:** ready-for-agent

- [ ] Development and production configuration validates Google OAuth, Better Auth, AI Gateway, Paper Trade base URL and credential, Neon/Postgres, and continuation-token encryption secrets.
- [ ] Production runs on Vercel with Neon and same-origin authenticated Eve routes; staging, alternate hosting, and self-hosted workflow infrastructure are not introduced.
- [ ] Structured server logs include sanitized Chat, Eve run, tool, duration, status, and error metadata.
- [ ] Logs never contain Google or Paper Trade credentials, continuation tokens, unrestricted generated specs, chain-of-thought, or sensitive tool output.
- [ ] Eve and AI Gateway dashboards provide run and usage visibility without a custom analytics pipeline or admin UI.
- [ ] All Paper Trade business operations in the authoritative contract are reachable through the agreed Benji tools; protocol-only `HEAD` and `OPTIONS` operations are not agent capabilities.
- [ ] The complete UI remains chat-centric, desktop-first, system-themed, accessible, and usable with the narrow-screen sidebar sheet; no separate portfolio dashboard or strategy editor appears.
- [ ] Unit tests cover the authenticated Benji operation seam, MSW-backed Paper Trade client, Eve access boundary, and presentation adapter boundary without live model calls.
- [ ] Local unit tests, linting, type checking, formatting, and production build all pass.
- [ ] Database changes are represented in Drizzle schema definitions and generated migrations rather than handwritten migrations.
- [ ] Manual agent-browser smoke testing covers Google sign-in, starter affordances, durable Chat creation, approved Brokerage Account creation, confirmed Investment Strategy creation, Tradable Security quote and `PriceChart`, approved Market Order execution, Portfolio Valuation, refresh/reconnect, Chat switching, and deletion.
- [ ] The authenticated smoke flow succeeds against the Vercel production deployment as well as development.
- [ ] No staging, rate limiting, hardened compliance workflow, custom administration, model fallback, live-model unit tests, or automated eval framework is added.
- [ ] “First pass complete” is declared only when every acceptance criterion across tickets 01–13 and the parent specification has been implemented and verified.
