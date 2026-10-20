---
name: roblox-animations
description: Plan and implement Roblox character, NPC, and rig animations; diagnose playback, replication, priorities, blending, markers, and animation lifecycle issues.
---

# Roblox animations

## Inspect and plan

Identify the rig (including R6/R15 or a custom skeleton), asset ownership/access, current Animate/controller code, and whether the request concerns gameplay or an edit-mode preview. Reuse the project's animation controllers and cleanup tools. Do not replace the rig or reposition an existing model just to make an example run.

Present the intended tracks, priorities, ownership/replication behavior, affected files, and checks before implementing. Wait for approval unless the current instruction already approves this concrete plan; continue through the approved scope without asking again.

## Playback and replication in running games

Use Animator:LoadAnimation rather than the deprecated Humanoid or AnimationController proxy methods. Load/cache tracks per Animator and release them with the character or rig. A new character has a new Animator; an old cached track does not transfer.

- The Animator must initially be created on the server for normal replication. A client-created Animator does not provide replicated playback.
- An owning client can start replicated animations on its own player character's server-created Animator. Do not claim that server-started character animations can never work.
- For a rig outside a player's character, load/start replicated animations on the server. Client-only cosmetic previews should be labeled as local.
- Ensure the Animator is in Workspace before loading animation content. Confirm asset permissions and rig compatibility when playback fails.

Read [playback.md](references/playback.md) for priorities, event semantics, diagnosis, and a scoped loading example.

For edit-mode plugin previews, use the reference's separate stepping and pose-restoration procedure. Gameplay client/server placement does not apply to that mode; do not insert a LocalScript and assume it will preview while Studio is stopped.

## Coordinate behavior

Choose priorities by intended joint ownership. From lowest to highest:

`Core < Idle < Movement < Action < Action2 < Action3 < Action4`

Core's enum number is 1000, but it is still the lowest priority. Tracks at the same priority blend by weight; higher priorities override lower priorities on contested joints according to evaluated weights. Author upper-body actions with appropriate keyed joints instead of assuming priority creates a body mask.

Use markers for timed cosmetic effects. Validate combat and rewards on the server; a client reporting a marker is not proof of a hit. Decide how interruption, looping, equipment swaps, death, and respawn affect effects and state.

Stopped means playback stopped and can occur before fade-out completes. Use Ended when cleanup must wait until the track has stopped influencing the subject. Connect one-shot listeners before starting playback when missing a short track's event would matter. Handle cancellation independently of natural completion.

## Verify

Check the intended rig, asset loading/permissions, transitions and interruption, blend/joint behavior, and cleanup after repeated spawns. For replicated behavior, verify from another client as well as the owner. Do not infer replication success from a single Studio viewport.

Prefer existing direct file/sync tooling and reuse the current Studio instance. Track and close any necessary extra session while preserving unsaved work. Source compilation and visual animation checks are different results; report both honestly.

## Sources

Reviewed 2026-10-20.

- [Animator and replication rules](https://create.roblox.com/docs/reference/engine/classes/Animator)
- [AnimationTrack and lifecycle](https://create.roblox.com/docs/reference/engine/classes/AnimationTrack)
- [AnimationPriority](https://create.roblox.com/docs/reference/engine/enums/AnimationPriority)
