---
name: roblox-remote-events
description: Design, implement, and debug Roblox RemoteEvent, RemoteFunction, and UnreliableRemoteEvent protocols, UI requests, replication, payload validation, and request lifecycle.
---

# Roblox client/server communication

## Define the protocol

Inspect current remotes, network wrappers, startup ordering, and server-owned game state. Reuse the installed networking library and its actual version; do not create a parallel set of remotes or silently change a payload contract used elsewhere.

Before implementation, describe each message's direction, schema, authority, response/error behavior, rate limit, affected files, and verification. Wait for approval unless the concrete plan is already approved. For a shop or inventory screen, plan UI pending/rejected states together with the server contract.

Read [protocols.md](references/protocols.md) for validation order, request lifecycle, finite-number checks, and transport choices.

## Choose transport by semantics

| Transport | Suitable use | Boundary |
| --- | --- | --- |
| RemoteEvent | Discrete requests and state notifications | Application still handles validation, races, replies, and retries. |
| RemoteFunction | A bounded client request for a server response | Yields the caller; avoid making server progress depend on InvokeClient. |
| UnreliableRemoteEvent | Disposable cosmetic/transient updates | No delivery or ordering guarantee; never use for purchases, inventory commits, or essential state. |

ReplicatedStorage is a conventional shared location, not the only valid parent for remotes. Create/configure the actual endpoint once, make startup idempotent, and have clients wait for the endpoint hierarchy. Do not assume server Scripts always initialize before all clients. A normal ModuleScript belongs to one runtime; use BindableEvent/direct calls for same-side communication when appropriate.

## Validate and commit

- OnServerEvent receives the sender Player from Roblox. Every payload field remains untrusted.
- Apply cheap rate/shape limits before expensive work. Bound strings, table size/depth, and work per request according to the protocol.
- Reject NaN, positive/negative infinity, fractional counts, and out-of-range values before arithmetic or clamping. Validate vector/CFrame components when they affect authoritative decisions.
- Clients request actions using stable IDs and minimal intent. The server derives prices, damage, rewards, ownership, and legal transitions from its own state.
- Resolve targets on the server or verify Instance class, allowed ancestry, ownership, and eligibility. A valid Instance or UserId is not proof the sender may act on it.
- Check alive/state, equipment, team, distance/line of sight, cooldown, and permissions as the action requires. Physics-derived positions may themselves be client-influenced.
- Prevent races across yielding checks: revalidate at commit and serialize/lock state as needed. A cooldown does not make an economic transaction atomic.
- Clean up per-player rate/request state on departure. For valuable retryable operations, use durable idempotency appropriate to the data layer; an arbitrary client request ID alone does not authorize a new reward.

## Responses and testing

Expose sanitized result codes and authoritative state/revisions. Correlate overlapping UI requests and ignore stale responses. Timeouts are local UX decisions; they do not cancel a server operation or prove it did not commit. Reconcile before retrying consequential actions.

Verify malformed payloads, permission failures, spam, duplicate requests, concurrent/yielding handlers, startup, disconnects, packet loss for cosmetic traffic, and UI reopening. Use a multi-client Studio test for replication when available; local helper checks are not network integration tests.

These are running-game boundaries. A Studio edit-mode plugin should use plugin APIs and local state, not invent a fake LocalPlayer/remote server connection. Prefer existing file/sync tooling, preserve the data model, and reuse Studio sessions; close any extra session you create without losing unsaved work.

## Sources

Reviewed 2026-10-20.

- [Remote events and callbacks](https://create.roblox.com/docs/scripting/events/remote)
- [Client/server validation](https://create.roblox.com/docs/scripting/security/client-server-boundary)
- [UnreliableRemoteEvent](https://create.roblox.com/docs/reference/engine/classes/UnreliableRemoteEvent)
