# 01 — Sign in and enter Benji

**What to build:** An Investor can sign in with Google and enter the authenticated Benji shell, with a clear but discreet understanding that the experience is simulated.

**Blocked by:** None — can start immediately.

**Status:** ready-for-agent

- [ ] Better Auth supports Google OAuth as the only sign-in method.
- [ ] Any valid Google account can sign in; there are no allowlists, invitations, roles, organizations, or administration flows.
- [ ] A signed-out visitor is directed to sign in and cannot enter the authenticated shell.
- [ ] A signed-in Investor reaches the chat-centric shell and can sign out.
- [ ] Better Auth’s stable identity is available as the canonical Paper Trade Investor identifier without a separate Investor record.
- [ ] A persistent but discreet “Simulated investing” indicator is visible in the authenticated experience.
- [ ] The interface follows the operating system light or dark preference and offers no manual override.
- [ ] The shell uses application-owned shadcn/ui foundations without modifying foundational components.
- [ ] Authentication configuration is validated for development and production.
- [ ] Authentication and shell behavior have focused tests at the highest practical boundary, with provider behavior mocked where necessary.
