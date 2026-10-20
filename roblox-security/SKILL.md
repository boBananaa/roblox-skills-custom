---
name: roblox-security
description: Review and strengthen Roblox server authority, interaction permissions, economy transactions, network ownership, and exploit resistance. Use for security reviews or gameplay features crossing a trust boundary.
---

# Roblox gameplay security

## Review the actual boundary

Identify what the feature protects, who can trigger it, and the authority for its state. Inspect existing validation, rate limits, profile/session tools, and legitimate gameplay exceptions. Review both remotes and other client-triggered mechanisms such as prompts, detectors, touched events, and physics.

Present findings and a concrete correction plan before implementation: exploit path, likely impact, affected code, compatibility, and checks. Wait for approval unless this plan is already approved. Keep a requested review read-only until changes are authorized. Reuse working infrastructure and avoid unrelated rewrites.

Read [validation.md](references/validation.md) for finite-value checks, contextual permissions, economic races, and movement review cases.

## Authority and validation

- Keep authoritative balance, rewards, inventory, damage, and permissions on the server. Clients may predict or display results but cannot commit them.
- Replicated modules are visible to clients. Shared item definitions, types, and deterministic helpers may be public; secrecy is not a substitute for server validation. Keep secrets and authoritative mutation code in server-only contexts.
- Reject invalid types, structures, non-finite components, and impossible values before use. Clamping hostile numbers is not a complete boundary.
- Validate the target's allowed ancestry/identity and the sender's ownership, permission, state, and context. Class and proximity alone do not authorize interaction.
- Derive prices and outcomes from server definitions. Bound input size/rate before expensive lookups, raycasts, or persistence.
- Protect read-check-write operations from interleaving when handlers yield. Recheck authorization/state and commit through one owning transaction path. Validate idempotency and cross-server ownership for persistence-backed actions.
- Treat property and physics replication separately. Network-owning clients can influence movement/physics without explicitly firing your RemoteEvent; FilteringEnabled does not eliminate this trust boundary.

## Movement and detection

Inspect the actual network ownership and movement model. Where server-authority features are relevant, verify their current availability and project configuration before relying on them. Do not silently migrate the simulation architecture.

Movement checks must account for game mechanics, server-authorized teleports, knockback, vehicles, respawn, and latency. A few high velocity samples are not sufficient evidence to kick. Reject impossible actions, record bounded diagnostic evidence, and tune proportionate responses before adding punishment. Clean detection state on PlayerRemoving and character replacement.

## Studio and external tools

Distinguish a game exploit surface from a Studio plugin's edit-time authority. A plugin can modify authored instances and script source; validate selections, provide undo for authored changes, and keep external requests/credentials scoped to the requested tool. Do not execute scripts hidden in imported assets or downloaded snippets just to inspect them.

Prefer direct inspection and file/API changes. Preserve existing models and unrelated data. Reuse Studio sessions; track and close any necessary extra instance without disturbing unsaved work. A simulated request test does not authorize production-data mutation or publishing.

## Verify and report

For each finding, show the trigger, affected state, proposed invariant, and observed test result. Exercise wrong identity/ancestry, non-finite values, spam, replays, overlapping requests, legitimate exceptions, and teardown where relevant. Test the server's behavior, not merely that a validator function was called.

Separate confirmed vulnerabilities from hypotheses and report what was not exercised. Never promise an exploit-proof game based on these checks.

## Sources

Reviewed 2026-10-20.

- [Client/server boundary](https://create.roblox.com/docs/scripting/security/client-server-boundary)
- [Network ownership and movement validation](https://create.roblox.com/docs/scripting/security/network-ownership)
- [Studio plugins and undo](https://create.roblox.com/docs/studio/plugins)
