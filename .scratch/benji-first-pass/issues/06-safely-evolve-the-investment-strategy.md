# 06 — Safely evolve the Investment Strategy

**What to build:** An Investor can refine the current Investment Strategy while Benji distinguishes harmless Strategy Clarifications from advice-altering Material Strategy Changes and prevents stale overwrites.

**Blocked by:** 05 — Create a confirmed Investment Strategy.

**Status:** ready-for-agent

- [ ] An explicit wording change that preserves strategy meaning is treated as a Strategy Clarification.
- [ ] Benji applies a Strategy Clarification immediately and then tells the Investor exactly what changed.
- [ ] A change that could alter recommendations or depends on a Benji assumption is treated as a Material Strategy Change.
- [ ] A Material Strategy Change remains a proposal until the Investor confirms it through a fixed application-owned control.
- [ ] The proposal shows changed fields with before-and-after text and the complete resulting Investment Strategy.
- [ ] Confirmation applies only the exact displayed proposal; an edited proposal requires a new confirmation.
- [ ] The strategy has a version, and each Material Strategy Change records the version from which it was drafted.
- [ ] If another Chat updates the strategy first, confirming the stale proposal fails closed and asks Benji to draft a fresh proposal.
- [ ] Unknown dimensions remain unknown unless the Investor explicitly resolves them.
- [ ] Unit tests cover clarification, materiality, assumptions, exact proposal application, version increments, stale-version rejection, and cross-Chat concurrency.
