# Playback, interruption, and diagnosis

## Keep tracks with their Animator

For a player-character LocalScript, this scoped example uses the server-created Animator. Replace the asset ID with an accessible animation compatible with the actual rig. It illustrates loading and release, not a complete controller.

```luau
local character = script.Parent
local humanoid = character:WaitForChild("Humanoid")
local animator = humanoid:WaitForChild("Animator")

local animation = Instance.new("Animation")
animation.AnimationId = "rbxassetid://YOUR_ACCESSIBLE_ANIMATION_ID"
local track = animator:LoadAnimation(animation)
animation:Destroy()

track.Priority = Enum.AnimationPriority.Action
track.Looped = false

local ended = track.Ended:Connect(function()
    -- Cosmetic completion only. A controller may choose to reuse this track.
end)

track:Play(0.15)

-- Have the character/controller's existing cleanup owner call this.
local function dispose()
    ended:Disconnect()
    track:Stop(0)
    track:Destroy()
end
```

Wire dispose to the real cleanup owner; merely defining it does not release anything. For NPC/non-player rigs, obtain/create the Animator on the server and play replicated tracks there. Do not clone a second Humanoid/Animator to bypass an initialization or permission problem.

## Edit-mode preview in a Studio plugin

Use an existing preview controller if the project has one. Otherwise verify the current Animator:StepAnimations API, available to plugin/command-bar contexts, rather than assuming a gameplay LocalScript runs in edit mode. Inspect the selected rig/controller and asset compatibility before loading; do not replace, move, or clone the user's rig just to produce a preview.

For an approved preview implementation:

1. Identify the selected existing rig and snapshot the original joint/bone transforms the preview may change, including relevant Motor6D.Transform/Bone.Transform and other pose properties it actually touches. Preserve authored poses rather than resetting everything to identity.
2. Load the preview track on the intended Animator, connect cleanup first, and start playback. Drive Animator:StepAnimations(deltaTime) through one owned, bounded stepping loop/connection while Studio is stopped. Do not simultaneously step an Animator already owned by a running simulation or another preview controller.
3. Handle not-yet-loaded track length and failed content access with a bounded readiness path. A zero-length initial observation does not justify an endless preview loop.
4. On stop, selection change, target removal, error, or plugin unload: disconnect stepping first, stop/release preview-owned tracks, and restore the surviving affected joints to their saved poses. Remove any controller/Animator created solely by this preview only if it is still owned and safe to remove. Do not stop unrelated tracks or leave temporary instances behind.

Revalidate the target before mutation and avoid conflicting with another tool editing the same joints. Transient preview state should not become persistent authored pose changes or noisy undo history. If authoring a pose is requested instead, make that a separate explicit edit with undo support. Verify stop/restart, selection change, failure, and unload restore the original pose in Studio; static inspection cannot prove this behavior.

## Blend intentionally

Priority is evaluated per joint, and track weight affects blending. Choose Idle/Movement for locomotion and the necessary Action level for actions rather than assigning Action4 everywhere. Core is the lowest priority despite its enum number.

If a wave must leave locomotion visible, verify the animation asset keys only the intended upper-body joints. A higher priority does not itself identify “arms only.” Inspect default Animate behavior and all relevant slots/variants before replacing default animation IDs; editing the first idle child can miss another weighted idle variant.

For transitions, specify interruption behavior: cancel cosmetic tasks, release temporary state, restore speed if this controller changed it, and prevent an old completion callback from modifying a new action. Do not make server authority depend on a client's animation playback reaching a marker.

## Stopped versus Ended

Stopped signals that playback finished or was stopped. The track may continue influencing joints during fade-out. Ended is the event for completion of that influence. A loop can keep running indefinitely; it is not a reliable one-shot completion source until stopped or made non-looping.

Track length may be zero before content finishes loading. Do not treat every zero-length observation as a bad Looped setting. Bound any loading wait and inspect permission/content errors.

## Diagnose before replacing code

| Symptom | Evidence to inspect |
| --- | --- |
| No movement | Asset permission, animation compatibility, Animator hierarchy/Workspace ancestry, track loading and weight. |
| Only owner sees it | Animator creation context, character ownership, server versus client playback, second-client observation. |
| Locomotion overwritten | Keyed joints, current Animate/controller tracks, priorities, weights, and interruption rules. |
| Completion too early | Stopped versus Ended, explicit Stop calls, cancellation, and fade settings. |
| Repeated actions leak effects | Marker connections, task ownership, reused tracks, and stale callback generations. |
| Works once, fails after respawn | References/tracks belong to the old Animator or listeners were not reconnected. |

See [Animator](https://create.roblox.com/docs/reference/engine/classes/Animator), [AnimationTrack](https://create.roblox.com/docs/reference/engine/classes/AnimationTrack), and [AnimationPriority](https://create.roblox.com/docs/reference/engine/enums/AnimationPriority). Reviewed 2026-10-20.
