# Message contracts and validation

## An action is not an outcome

For an attack, the client can request use of an equipped tool and provide bounded aim/input hints. The server resolves the equipped tool, legal cadence, hit context, teams, and damage from its own state and checks. Do not accept `damage` and merely clamp it to the weapon's maximum: the client could still choose the maximum on every hit.

For a shop, the client sends a catalog item ID and a legal integer count. The server resolves price, availability, proximity/permissions, inventory capacity, and the current balance. All checks must still hold at the commit point. If a check yields, it can interleave with another request; serialize/revalidate before changing state.

## Finite numbers before arithmetic

This pure Luau helper illustrates the numeric precondition. Keep a single authoritative validator in the actual project's existing module rather than copying variants into each handler.

```luau
local function isFiniteNumber(value: unknown): boolean
    return type(value) == "number"
        and value == value
        and value > -math.huge
        and value < math.huge
end

local function isCount(value: unknown, maximum: number): boolean
    if not isFiniteNumber(value) then
        return false
    end
    local count = value :: number
    return count >= 1 and count <= maximum and count % 1 == 0
end
```

Here `maximum` is a finite positive server constant, not client input. Validate every vector component (and CFrame rotation/position components when relevant) before using them in authoritative geometric checks. A comparison such as `distance > limit` can fail open when distance is NaN.

Use reject-versus-normalize semantics intentionally. Reject invalid shop counts; normalizing a purely cosmetic slider after finite-value validation may be appropriate. Type checks alone do not establish bounded work or permissions.

## Handler shape

1. Apply a cheap per-player/protocol limiter and bound payload structure/size.
2. Validate field types and finite/integer/range constraints before arithmetic.
3. Resolve allowed targets/catalog entries; check sender permission and context.
4. Perform expensive verification only after cheap rejection paths.
5. Commit via the owning state layer, accounting for interleaving and idempotency.
6. Return a bounded, sanitized result and authoritative revision/state.

Validate Instance class and membership in the allowed container; do not permit arbitrary descendants elsewhere just because they are nearby. Check ownership separately. For UserIds, valid lookup does not establish that a target may be attacked or traded with.

Rate limits are feature-specific. Bound both allowed operations and the bookkeeping work needed to reject traffic; do not keep unbounded payload/history tables per player. Clean state on departure. Client debouncing is only a UX optimization.

## UI request lifetime

Use correlation IDs plus a screen/request generation where replies may arrive after close/reopen. Bound outstanding requests and define expiry/cleanup. A timeout marks the view as waiting too long, not the server mutation as canceled. Reconcile before retrying a purchase/trade/reward. If request IDs implement idempotency, bind them to sender, operation, payload, and an appropriate retention scope; client choice of a new ID cannot authorize another grant.

A RemoteFunction invocation yields its caller. Spawning it in a task does not give the invocation a real transport timeout/cancel operation; abandoned calls can accumulate. For UI operations needing controlled lifetime, a RemoteEvent request/response protocol can expose explicit pending/correlation state. It still needs bounded bookkeeping and semantic reconciliation.

Never wait on InvokeClient to decide server-authoritative progress. A client can fail, disconnect, or never return. Do not confuse OnClientEvent, which has no return value, with OnClientInvoke.

## Unreliable traffic

Use UnreliableRemoteEvent only when dropped/reordered messages are acceptable. Include a sequence/timestamp strategy when useful; do not trust a client timestamp as an authoritative clock. Bound send frequency and payload against the current platform limit. Periodic reliable snapshots or existing replication can recover essential state; a cosmetic stream should not become the only record of a meaningful action.

Do not stream authoritative character position from a new per-frame remote without inspecting Roblox's existing replication and movement ownership. A CFrame type check is insufficient for accepting movement or combat state.

## Cases to exercise

Malformed/massive payloads, NaN/Inf/fractions, disallowed targets, rate-limit cleanup, duplicate requests, two overlapping spends, slow handlers, clients joining during endpoint creation, disconnects, stale responses, and unreliable message reordering. Check the final authoritative state as well as response codes.

See [remote events and callbacks](https://create.roblox.com/docs/scripting/events/remote) and [client/server validation](https://create.roblox.com/docs/scripting/security/client-server-boundary). Reviewed 2026-10-20.
