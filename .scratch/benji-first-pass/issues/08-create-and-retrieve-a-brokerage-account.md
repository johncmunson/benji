# 08 — Create and retrieve a Brokerage Account

**What to build:** An Investor without a Brokerage Account can choose Starting Cash, approve exact account creation, and then retrieve the resulting simulated account safely through Benji.

**Blocked by:** 05 — Create a confirmed Investment Strategy.

**Status:** ready-for-agent

- [ ] Benji can detect that the authenticated Investor has no Brokerage Account and asks for a Starting Cash amount rather than inventing one.
- [ ] Investment Strategy setup is not required before account creation.
- [ ] A fixed application-owned Investor Approval displays the exact USD Starting Cash before creation.
- [ ] Generated UI may explain account creation but cannot approve or execute it.
- [ ] The Paper Trade tool schema omits `investorId`; the server derives Better Auth’s stable identity from the authenticated Chat owner.
- [ ] Narrow client operations create and retrieve the Brokerage Account through the external Paper Trade contract.
- [ ] Each proposed account creation persists its parameters, approval state, application-generated idempotency key, status, and terminal result.
- [ ] The browser and model cannot supply or alter the idempotency key.
- [ ] Eve replay of the same approved operation reuses the persisted key.
- [ ] An ambiguous mutation transport interruption retries once with the same key; ordinary API errors do not retry.
- [ ] Retrieved cash and account values preserve safe integer cents internally and display as USD.
- [ ] Unit tests exercise the authenticated operation seam and MSW client seam for missing accounts, exact approval, identity derivation, successful creation, idempotent replay, ambiguous transport recovery, malformed responses, and ordinary errors.
