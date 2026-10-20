# Review invariants, not just type checks

## Finite value example

For a server catalog purchase, a count validator needs finite, integral, bounded input before multiplication or deduction. This pure Luau example assumes a finite positive server-defined maximum:

```luau
local function validCount(value: unknown, maximum: number): boolean
    if type(value) ~= "number" then
        return false
    end
    if value ~= value or value <= -math.huge or value >= math.huge then
        return false
    end
    return value >= 1 and value <= maximum and value % 1 == 0
end
```

Once valid, the handler must still check allowed item, real server price, balance, capacity, permission, and commit semantics. The validator alone does not make a purchase secure. In the project, reuse the same validation module across consumers instead of maintaining subtly different copies.

For spatial validation, check all relevant components and the derived distance/result before comparing. NaN makes ordinary comparisons unreliable. A finite position can still be physically impossible or client-influenced; value validation does not establish movement authority.

## Interaction authority

Suppose a client sends an Instance for a nearby button. Check its class, that it is part of the server's allowed interaction set/hierarchy, and that the player may operate this specific button in the current state. A valid BasePart within ten studs could belong to another player's locked property.

The same reasoning applies to ProximityPrompt/ClickDetector/touched flows. Inspect what the particular event guarantees on the server; do not assume every visual client property is an enforced permission. Resolve eligibility again before applying the action, using trustworthy state and the feature's actual range/line-of-sight rules.

## Economic state

For a spend-and-grant transaction, record the authoritative price and intended result, then ensure balance and inventory changes commit together in the owning state layer. If checks yield, protect against another request passing the same old-balance check. Reject requests after profile ownership is lost.

For valuable retries, a durable operation identity prevents duplicate application only when it is checked/committed with the effect. A flag written separately from the reward creates a crash window. Do not bypass a session-locked profile with direct DataStore calls.

## Movement and physics

Network ownership can give clients authority over assemblies and movement. A server's current observation of a root part is therefore evidence to validate, not unquestionable truth. Inspect which state the game currently simulates authoritatively and verify any beta/new engine authority mode before depending on it.

Use checks tied to allowed mechanics. Horizontal displacement may be useful for ground movement, but jumps, vehicles, moving platforms, knockback, teleports, and latency need different treatment. Maintain server-issued exemptions with bounded lifetimes rather than disabling validation globally. Treat detections as signals; use context and evidence before punishment. One simplistic speed threshold is not an anti-cheat system.

Prefer preventing the unauthorized benefit (rejecting a hit/reward, denying an illegal interaction) to relying solely on later punishment. Test legitimate exceptions along with malicious requests so the fix does not break the game.

## Example review matrix

| Attempt | Required invariant |
| --- | --- |
| NaN/Inf or fraction as count | No balance/inventory mutation or unsafe arithmetic. |
| Another player's valid target | Permission denied despite correct type and proximity. |
| Two simultaneous spends | No negative balance or duplicate grant through interleaving. |
| Replayed rewarded request | Eligibility/operation state prevents repeat rewards. |
| Old owner writes after rejoin | Ownership token invalidates stale writes. |
| Client warps then claims reward | Server verifies the reward's real eligibility, not proximity alone. |
| Authorized teleport/knockback | Legitimate player is not punished by generic speed checks. |
| Player leaves under spam | Per-player limiter/detection/request state is released. |

For findings, give a reproduction path, server-side impact, smallest corrective change, and a meaningful check. Label hypothetical paths as unverified. Do not report a fixed vulnerability solely because a helper returns false in isolation.

See [client/server boundary](https://create.roblox.com/docs/scripting/security/client-server-boundary) and [network ownership and movement](https://create.roblox.com/docs/scripting/security/network-ownership). Reviewed 2026-10-20.
