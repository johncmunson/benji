<!-- BEGIN:nextjs-agent-rules -->

# This is NOT the Next.js you know

This version has breaking changes — APIs, conventions, and file structure may all differ from your training data. Read the relevant guide in `node_modules/next/dist/docs/` before writing any code. Heed deprecation notices.

<!-- END:nextjs-agent-rules -->

## Agent skills

### Issue tracker

Issues are tracked as local Markdown files under `.scratch/`. See `docs/agents/issue-tracker.md`.

### Triage labels

Triage uses the five default canonical labels. See `docs/agents/triage-labels.md`.

### Domain docs

Domain docs use a single-context layout. See `docs/agents/domain.md`.

## Additional notes

- Use `pnpm` and/or `pnpx` as opposed to `npm` or `yarn`.
- A dev server will always be running for you at `http://localhost:3000`
- This project does not use CI. All testing, linting, and other checks are performed locally.
- Available subagents: planner, reviewer, researcher, scout, and worker. Agent skill files might mention other subagent types. Map them to one of the available subagents (task dependent).
- When launching `agent-browser` for this app, reuse the authenticated profile with `--profile ~/.agent-browser/profiles/wealth-manager`. Alert the user if you discover that the profile is no longer authenticated and coach them through restoring it:
  1. Have them sign in through their normal browser, then copy the `better-auth.session_token` value from DevTools → Application → Cookies → `http://localhost:3000`.
  2. Have them save only the value to `/tmp/wealth-manager-session-cookie` by running the following command: `bash -c 'umask 077; IFS= read -rsp "Paste session cookie value: " cookie && printf "%s" "$cookie" > /tmp/wealth-manager-session-cookie && printf "\nSaved.\n"'`
  3. Import it with `agent-browser cookies set` for `http://localhost:3000`, verify an authenticated page, and immediately delete the temporary file.
- When building frontend UI components and pages, always seek to leverage shadcn/ui components as the foundation
- No need to install any new shadcn components... they've all been installed already
- Never mutate any of the foundational shadcn/ui components inside of `components/ui`
- If you ever need to run or execute Python, the command is `python3` and not `python`
- Never write db migrations manually. Instead, modify the db schema in `db/schema/` and then run `pnpm db:generate`.
- This project does not use CI. All testing, linting, and other checks are performed locally.
- Make efforts to design responsive layouts, but do not go out of your way to support mobile. We do not claim to have strong support for mobile at this time.
- Testing posture:
  - unit tests with mocked boundaries (HTTP, database) using vitest and mock-service-worker
  - manual smoke testing and sanity checks using agent-browser
