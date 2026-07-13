# Reference Architecture: Durable Eve Agents with json-render UI

**Status:** Proposed baseline · **Date:** 2026-07-13 · **Applies to:** Eve 0.22.5, Next.js, React, json-render

## 1. Executive summary

Use **Eve as the durable agent execution engine** and **json-render as the generated presentation layer**. Eve streams normal conversational output. When rich UI is useful, the agent calls a typed `present_ui` tool with one complete json-render spec. The browser renders it only after the tool succeeds; progressive generative UI is intentionally excluded.

```text
Browser: useEveAgent
  ├─ streamed text, reasoning, tools, and errors
  ├─ fixed HITL and authorization controls
  └─ successful present_ui input
      └─ catalog validation → json-render registry → trusted React/shadcn UI

Application: authentication, session ownership, event projection, domain truth
Eve: durable sessions, streams, tools, HITL, workspace, subagents, schedules
```

The sole Eve/json-render seam is `present_ui`; no AI SDK stream adapter is required.

## 2. Requirements and non-goals

The system must provide:

- durable multi-turn sessions and reconnectable ChatGPT-like streams;
- persistent per-session workspaces;
- tools, HITL, authenticated connections, subagents, and lifecycle events;
- safe, atomic generative UI from an application-owned catalog;
- authenticated tenant-safe access to root and child sessions;
- recovery after browser refresh, process restart, and deployment.

The baseline does not provide:

- progressive json-render patches;
- arbitrary generated code, HTML, URLs, or network calls;
- generated HITL or OAuth controls;
- immediate server-side cancellation of an Eve run;
- guaranteed immediate deletion of Eve workflow history or sandbox snapshots;
- detailed live transcripts from every subagent.

“Stop generating” stops receiving the stream, not backend work. “Delete chat” revokes application access and deletes application-owned records, without promising immediate erasure of Eve-managed data.

## 3. Architectural decisions

1. **Eve owns execution.** It owns history, model/tool loops, durable pauses, events, workspaces, and child sessions. Next.js must not recreate the loop or keep process-local agent sessions.
2. **The application owns authority.** Domain data, identity, tenant membership, chat ownership, and retention policy stay in application storage.
3. **UI crosses Eve as a tool call.** `present_ui` transports a complete spec through normal typed tool events.
4. **Generated UI expresses intent, never authority.** Catalog actions map only to trusted application handlers.
5. **Platform controls are fixed.** Eve approvals, questions, OAuth, retry, and status use ordinary application components.
6. **Turns are serialized.** Eve is not a durable per-session FIFO; the baseline disables sending while a turn is active.

## 4. Logical modules

| Module               | Interface and responsibility                                            |
| -------------------- | ----------------------------------------------------------------------- |
| Agent Chat           | Sends turns, renders Eve parts, controls composer state, reconnects.    |
| Agent Access         | Authenticates and authorizes root/child session operations.             |
| Session Directory    | Maps app chat IDs to Eve sessions, owners, tenants, and cursors.        |
| Event Projection     | Stores or rebuilds renderable history from authoritative Eve events.    |
| UI Catalog           | Defines components, props, actions, functions, and spec validation.     |
| UI Registry          | Maps catalog names to trusted React/shadcn implementations.             |
| Presentation Adapter | Validates successful `present_ui` input and invokes json-render.        |
| Eve Agent            | Holds instructions, tools, skills, sandbox, connections, and subagents. |

The Presentation Adapter should expose one deep interface:

```ts
renderPresentedUI(part: EveMessagePart): ReactNode
```

It hides tool-state checks, validation, fallback UI, and renderer providers from chat messages.

## 5. Suggested layout

```text
agent/
  agent.ts                 instructions.ts
  channels/eve.ts          sandbox.ts
  tools/present_ui.ts      tools/...domain tools
  subagents/...specialists
lib/agent/
  session-directory.ts     access.ts     event-projection.ts
lib/render/
  catalog.ts               registry.tsx  generated-ui.tsx
components/agent-chat/
  chat.tsx  message.tsx  approval.tsx  authorization.tsx  tool-status.tsx
```

`catalog.ts` must be server-safe: Eve imports it while compiling the tool schema, so it must not import React or browser-only registry code.

## 6. The `present_ui` contract

Use an object wrapper so future spec schemas need not have an object root:

```ts
// agent/tools/present_ui.ts
import { defineTool } from "eve/tools"
import { z } from "zod"
import { catalog } from "../../lib/render/catalog"

export default defineTool({
  description: "Present one complete generated interface to the user.",
  inputSchema: z.object({ spec: catalog.zodSchema() }),
  execute: () => ({ presented: true }),
  toModelOutput: () => ({ type: "text", value: "The interface was accepted." }),
})
```

Invariants:

- `spec` is JSON-serializable and valid against the catalog;
- one call represents one intended interface;
- the tool performs no domain mutation and needs no approval;
- large datasets stay in domain storage; specs contain bounded display data;
- output is a small acknowledgement and does not repeat the spec.

## 7. Agent instructions

Do not use `catalog.prompt({ mode: "inline" })` unchanged. Documented json-render prompt modes teach JSONL patches, while this design requires a complete tool argument.

```text
When rich presentation improves the answer, call present_ui with one complete spec.
Do not emit JSONL, JSON Patch, markdown spec fences, or raw specs in prose.
Use only components, props, bindings, and actions accepted by the tool schema.
Gather and verify required data before presenting it.
Keep specs bounded and place initial renderer state in spec.state.
A text-only answer is valid when rich UI adds no value.
```

Catalog-specific composition guidance may be added, but the tool schema is authoritative.

## 8. Request and presentation flow

```text
1. Browser sends an authenticated turn with the current Eve cursor.
2. Eve streams message.appended/reasoning/tool lifecycle events.
3. Agent gathers data through tools or subagents.
4. Agent calls present_ui with a complete spec.
5. Eve validates the tool input and emits actions.requested.
6. present_ui returns its acknowledgement; Eve emits action.result.
7. useEveAgent projects the call as dynamic-tool/output-available and retains input.
8. Presentation Adapter validates input.spec again and renders it atomically.
9. Eve may stream final prose, then emits a terminal session event.
10. Browser enables input only at session.waiting/completed/failed.
```

Render only after successful completion:

```tsx
function PresentedUI({ part }: { part: EveMessagePart }) {
  if (
    part.type !== "dynamic-tool" ||
    part.toolName !== "present_ui" ||
    part.state !== "output-available"
  )
    return null

  const parsed = catalog.validate((part.input as { spec?: unknown }).spec)
  return parsed.success ? (
    <GeneratedUI spec={parsed.data} />
  ) : (
    <InvalidGeneratedUI />
  )
}
```

The second validation protects the rendering seam from stale events, version skew, corrupted persistence, and unsafe casts. Unknown components fail visibly without crashing chat.

`GeneratedUI` supplies json-render’s `StateProvider`, `VisibilityProvider`, `ActionProvider`, and `Renderer` using the trusted registry.

## 9. Generated actions and renderer state

Actions are either:

- **local**, such as tabs, filters, and draft input; or
- **server-bound**, crossing one narrow trusted handler.

Example declaration:

```ts
send_agent_intent: {
  params: z.object({
    intent: z.enum(["explain", "refresh", "compare"]),
    subjectId: z.string(),
  }),
}
```

A server-bound handler may call `agent.send()`, but the server derives user, tenant, chat, and permissions from authenticated context. Generated actions must never call arbitrary URLs/tools, submit Eve `inputResponses`, approve operations, provide identity, or mutate canonical data without validation and idempotency.

Renderer state is local by default. Persist it only when required, and store it as application data rather than Eve orchestration state.

## 10. Session persistence and reconnect

Persist the complete Eve cursor:

```text
sessionId + continuationToken + streamIndex
```

Also persist the event log or a lossless message projection. The cursor resumes execution; it is not a transcript.

```text
AgentChat
  id
  ownerUserId
  tenantId
  eveSessionId
  encryptedContinuationToken
  streamIndex
  status
  createdAt / updatedAt
```

Required flow:

1. Create the application chat before the first turn.
2. Bind the returned Eve session before exposing it for reuse.
3. Persist events idempotently in stream order, or persist a lossless projection.
4. Save cursor progress while streaming, not only at completion.
5. On refresh, hydrate saved history and cursor.
6. If a turn was active, reconnect with `session.stream({ startIndex })`.
7. Remount the chat client when switching threads.

## 11. Authentication and authorization

Use same-origin cookie authentication when possible and author an Eve channel for production browser access. Eve authenticates routes but explicitly does not add a session ownership ACL.

Every operation must establish:

```text
authenticated principal
  + chat ownership or tenant membership
  + root/child session mapping
  + operation-specific permission
```

Enforce this for create, continue, stream, HITL, authorization, and child streams. Never treat `sessionId` or `continuationToken` as authorization, and never trust identity supplied by prompts, client context, tool input, or generated actions.

Ownership may be enforced in a custom Eve channel/auth adapter or a thin pass-through gateway. Preserve Eve JSON/NDJSON unchanged; no `pipeJsonRender` or `data-spec` translation is needed.

## 12. HITL and connection authorization

Eve `input.requested` and `authorization.required` events are authoritative:

- render them with fixed components;
- preserve request IDs and options exactly;
- reauthorize the current caller before accepting a response;
- resume through the same session;
- disable ordinary input while authorization is pending;
- keep approval separate from idempotency and domain authorization.

Sensitive tools re-check current permissions inside `execute`, even after approval.

## 13. Subagents

The parent stream reports delegation and completion; detailed child progress lives on child streams. Initially:

- show delegated/running/completed status from the parent;
- map every child session to the root chat owner;
- authorize child streams like root streams;
- defer nested live child transcripts until required.

Declared subagents have separate capabilities and workspaces. Eve’s built-in copied agent shares the parent workspace. Choose based on whether filesystem sharing is required.

## 14. Durability and failure rules

Eve replays completed steps, but an interrupted step may execute again. Therefore:

- domain side effects require application idempotency keys;
- approval does not replace idempotency;
- projections tolerate duplicate/replayed events;
- invalid `present_ui` calls fail closed and render nothing;
- unknown events are retained and shown through a safe fallback;
- disconnects resume from `streamIndex`;
- UI distinguishes stream disconnect from session failure;
- workspace survives turns and ordinary idle recovery, but sandbox source, seed, or revalidation changes may replace it.

## 15. Deployment and security

For Next.js, `withEve()` mounts same-origin routes and avoids CORS. On Vercel, use the platform Workflow world and Vercel Sandbox unless another backend is required.

Production requires:

- authenticated and ownership-checked Eve routes;
- durable Workflow storage and a persistent sandbox backend;
- stored session ownership, cursor, and renderable history;
- secrets outside the sandbox and explicit sandbox network policy;
- least-privilege connection credentials and tool approvals;
- observability for root and child runs.

Self-hosting must persist Workflow data and proxy both Eve routes and Workflow callbacks.

## 16. Observability

Record chat/session/turn/tool IDs, authenticated user and tenant, lifecycle events, tool duration/status, approval outcomes, `present_ui` validation failures and size, reconnect count, last stream index, child relationships, token usage, and terminal failure reason.

Never log continuation tokens, credentials, OAuth challenges, unrestricted specs, or sensitive tool output.

## 17. Verification gates

Before production, prove:

1. Text streams before and after a normal tool and one atomic `present_ui` call.
2. Refresh works during text, after `present_ui`, during a subagent, and while HITL is parked.
3. Replay yields one transcript and one generated interface without duplicates.
4. Another user cannot inspect, stream, continue, or approve root/child sessions.
5. Malformed, oversized, unknown-component, and unknown-action specs fail closed.
6. Generated parameters cannot change identity, tenant, chat, or permissions.
7. Interrupted side effects remain single through idempotency.
8. Restart/redeploy preserves session, stream, HITL, and workspace behavior.
9. “Stop” is presented honestly as disconnect, not backend cancellation.

## 18. Implementation sequence

1. Mount Eve and stream text with `useEveAgent`.
2. Add authenticated ownership and cursor/history persistence.
3. Add a three-component catalog and `present_ui`.
4. Render only validated, successful calls.
5. Add fixed HITL and authorization components.
6. Add one local action and one constrained server-bound intent.
7. Add workspace recovery and one subagent.
8. Pass verification before expanding the catalog or tools.

## 19. Evidence base

This design follows the installed documentation and runtime:

- durability/replay: `node_modules/eve/docs/concepts/execution-model-and-durability.md`;
- sessions/NDJSON: `node_modules/eve/docs/concepts/sessions-runs-and-streaming.md`;
- React/reconnect: `node_modules/eve/docs/guides/frontend/overview.mdx`;
- Next.js: `node_modules/eve/docs/guides/frontend/nextjs.mdx`;
- auth/no ownership ACL: `node_modules/eve/docs/guides/auth-and-route-protection.md`;
- tools/Standard Schema: `node_modules/eve/docs/tools/overview.mdx`;
- HITL, sandbox, subagents: `node_modules/eve/docs/tools/human-in-the-loop.md`, `sandbox.mdx`, `subagents.mdx`;
- cancellation limitation: `node_modules/eve/CHANGELOG.md` (`c233a6a`);
- catalog/validation/actions: `.agents/skills/json-render-docs-and-example-apps/docs/catalog/page.mdx` and `registry/page.mdx`;
- generation modes: `.agents/skills/json-render-docs-and-example-apps/docs/generation-modes/page.mdx`;
- working UI patterns: `.agents/skills/json-render-docs-and-example-apps/example-apps/chat` and `example-apps/harness-chat`.

## 20. Final position

Eve is the durable execution engine; json-render is the constrained presentation engine; the application remains the authority for identity, ownership, domain state, and side effects. The systems integrate cleanly through one typed tool seam while explicitly accepting Eve 0.22.5’s lack of server-side cancellation and hard-deletion controls.
