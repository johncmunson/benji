# 04 — Manage multiple Chats

**What to build:** An Investor can create, list, switch between, and delete multiple durable Chats while sharing one Brokerage Account and Investment Strategy across them.

**Blocked by:** 02 — Hold a durable Chat with Benji.

**Status:** ready-for-agent

- [ ] The Chat sidebar lists only the authenticated Investor’s Chats in a stable useful order.
- [ ] An Investor can create a new empty Chat and switch among existing Chats.
- [ ] Switching Chats binds the transcript and Eve client to the selected Chat without leaking state from the previous Chat.
- [ ] All Chats for one Investor share the same Brokerage Account identity and future Investment Strategy record.
- [ ] Another authenticated Investor cannot list, inspect, stream, continue, approve, or delete these Chats.
- [ ] Deleting a Chat removes application-owned transcript and Chat records and permanently revokes application access to its Eve execution context.
- [ ] The product does not claim that deleting a Chat immediately erases Eve-managed workflow history or snapshots.
- [ ] On narrow screens, the Chat sidebar collapses into an accessible sheet while the transcript and composer remain usable.
- [ ] Manual rename, search, folders, sharing, archival, and transcript export are absent.
- [ ] Unit tests cover ownership-filtered listing, switching, deletion and revocation, deterministic titles, and shared Investor-level identity.
