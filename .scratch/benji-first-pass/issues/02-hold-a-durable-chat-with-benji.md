# 02 — Hold a durable Chat with Benji

**What to build:** An authenticated Investor can start one Chat, send text, receive a streamed Benji response through Eve, and return to the completed transcript after refresh.

**Blocked by:** 01 — Sign in and enter Benji.

**Status:** ready-for-agent

- [ ] A first message creates an application-owned Chat bound to one durable Eve execution context and the authenticated Investor.
- [ ] The browser reaches Eve through a same-origin channel that authenticates every create, continue, and stream operation and rejects another Investor.
- [ ] Eve uses `openai/gpt-5.6-sol` through AI Gateway with medium reasoning effort.
- [ ] Benji’s standing instructions establish its calm, concise, plainspoken voice and prohibit invented certainty.
- [ ] Normal assistant text streams into the transcript without an AI SDK/json-render stream adapter.
- [ ] Ordered Eve events and the complete resume cursor are persisted while the application remains authoritative for Chat ownership.
- [ ] Eve continuation tokens are encrypted before persistence and never exposed to the browser or logs.
- [ ] A completed transcript restores after page refresh without duplicate content.
- [ ] A short Chat title is derived deterministically from the trimmed first Investor message without a second model call.
- [ ] The empty Chat shows three clickable starter affordances for account creation, Investment Strategy definition, and a general investing question.
- [ ] Clicking a starter affordance populates the composer without submitting; the affordances disappear after the first message is sent.
- [ ] The transcript shows concise run status without chain-of-thought, raw Eve events, credentials, or unrestricted payloads.
- [ ] Unit tests cover authenticated channel access, ordered event projection, title derivation, encrypted token persistence boundaries, and completed-transcript hydration without live model calls.
