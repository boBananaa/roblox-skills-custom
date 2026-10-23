---
name: roblox-gui
description: Plan, implement, and review Roblox HUDs, menus, inventories, shops, world-space UI, and Studio plugin panels. Use for responsive layout, UI state, input, screenshot-inspired design, medieval/fantasy interfaces, spectator-friendly minigame lobby layouts, or adding reusable UI techniques to this skill.
---

# Roblox UI

Build interfaces that fit the actual game, input methods, and existing UI stack. Screenshots inform design decisions; they do not establish hidden behavior or implementation details.

## Start with the project and a plan

Inspect the current hierarchy, scripts, UI framework, packages, and working examples. Reuse compatible components and libraries before introducing a new abstraction. Check a dependency's actual version and documentation; do not assume React, Fusion, or a particular API is installed. Propose a new dependency only when its maintenance and integration cost is justified.

Before implementation, present the intended layout, states, affected files/instances, client/server boundary, dependencies, and verification approach. Wait for the user's go-ahead unless the current instruction already approves that concrete plan. A request for a plan alone authorizes inspection, not implementation. Once approved, continue within that scope without repeatedly asking. A material change in design or scope needs a revised plan.

Choose the execution context:

| Context | Approach |
| --- | --- |
| In-game screen | Author reusable templates/components; run player-facing behavior on the client. |
| World-space UI | Choose SurfaceGui or BillboardGui and an appropriate Adornee. Interactive world UI needs PlayerGui ownership and the documented input setup. |
| Studio tool | Use a plugin-owned dock widget and edit-mode lifecycle. Do not assume LocalPlayer or PlayerGui exists. |

Read [implementation.md](references/implementation.md) for layout, input, UI state, cleanup, and review checks. Read [studio-workflow.md](references/studio-workflow.md) when establishing sync or implementing a plugin or persistent place edit.

## Design from screenshots

For reference images or requests to grow the public skill pack, read [design-learning.md](references/design-learning.md). First identify what the user wants to borrow and whether it is a project-specific direction or a lasting preference. If unspecified, extract useful observations without making them universal style rules.

Translate the reference into hierarchy, proportions, spacing, typography roles, surface treatments, and component states. Keep uncertain measurements approximate. Mark motion, interaction, fonts, and source code as unknown when the screenshot cannot establish them. Choose Roblox-native layout tools or existing framework components for the desired result, rather than guessing how the source was built.

Use a coherent visual direction that suits the game. Avoid prescribing the same palette, corner radius, or card layout for every project. Integrate unfamiliar ideas when they help the requested design and explain the tradeoff.

## Medieval and fantasy interfaces

When the user requests medieval, fantasy, or Deepwoken-inspired UI, read
[medieval-fantasy-ui.md](references/design-patterns/medieval-fantasy-ui.md). Use its
reference-analysis and hierarchy techniques within the requested palette and
execution context; it is an optional direction, not the default for every game.

## Colorful rewards and progression panels

For screenshot-inspired reward calendars, login bonuses, or milestone grids, read
[rewards-progression-ui.md](references/design-patterns/rewards-progression-ui.md).
Use its card hierarchy, state distinctions, and small-screen adaptations when the
requested interface fits; do not apply this colorful style to every game.

## Lobby leaderboards and navigation

For lobby ranking boards, avatar showcases, kiosks, queue pads, destination
markers, overhead-label overlap, or persistent edge navigation, read
[lobby-leaderboards-hud.md](references/design-patterns/lobby-leaderboards-hud.md).
Adapt its hierarchy and visibility checks to the actual map and target inputs.

## Class and loadout selection

For class lists, character previews, equipment slots, or explicitly requested
class-roll screens, read
[class-loadout-ui.md](references/design-patterns/class-loadout-ui.md). Separate
selection from equipped state and keep acquisition and purchase behavior explicit.

## Implementation decisions

- Keep UI presentation and state separate from authoritative inventory, currency, rewards, and purchases. Buttons request actions; the server validates and returns the resulting state.
- Reuse established spacing, colors, typography, and component variants through the project's token/theme system. Introduce a small one only if it solves real repetition.
- Combine relative sizing with padding, offsets, minimum/maximum constraints, and content-driven layouts. Scale-only UI is not automatically responsive.
- Support the project's target inputs. Prefer GuiButton.Activated for shared mouse, touch, and controller activation; implement controller selection/focus and touch-sized targets where needed.
- Design loading, empty, disabled, error, pending, and success states where applicable. Close stale requests and handle reopening a screen while a previous request is pending.
- Give every component/controller an owner that disconnects events, cancels work/tweens, and removes owned instances on teardown. Use the existing cleanup library if there is one.
- Treat UI persistence across respawns and character-bound bindings as separate decisions. Rebind on CharacterAdded and release the old character's listeners.

## Minigame lobbies and spectator views

Maintainer preference: usually place the active minigame arena below and in view of the spawn/lobby baseplate, so newly joined and eliminated players can watch their friends playing. Treat this as a default for minigame layouts, not a requirement for every map.

- Prefer an arena offset beside and below the lobby. Directly stacking it underneath can let the opaque lobby floor block the action. Choose the horizontal offset and vertical separation together to expose the playing surface from accessible lobby edges.
- Preserve safe spectator boundaries, including entrance gaps. Account for scenery, curbs, arena walls, and HUD panels that could obscure the action. Add viewing space only when needed and within the approved scope.
- Verify from normal player eye height at several reachable lobby viewing spots. Inspect a screenshot with the actual HUD and sample sightlines to the near, middle, far, and side regions of the arena. An aerial Studio overview alone does not establish visibility. When using raycasts to check visual occlusion, exclude fully transparent spectator barriers; keep opaque scenery and floors in the check.
- Do not add spectator cameras, round logic, or teleport behavior unless requested.

Tested example, not fixed dimensions: a roughly 160-by-160 lobby with its floor at Y=6 and a 192-by-192 arena centered 220 studs to one side, with its surface at Y=-30. At eye height 5.5 studs above the lobby floor, three viewing spots near that edge had 27 clear sampled sightlines across the arena; a player-height screenshot also showed the playing surface with the HUD present. The maintainer preferred this arrangement to the arena centered directly beneath the lobby at Y=-100. Adapt these proportions to the actual map and camera, then verify again; this example does not establish visibility from every lobby position or on every device.

## Verify and report

Use the project's existing build/type checks, then inspect representative viewport sizes, long text/localization, actual target input, overlapping dialogs, and respawns. Test server rejection and slow responses for action-driven UI. Use device emulation for layout and real input testing when available; emulation alone does not prove gamepad navigation or device performance.

Prefer direct files/APIs for edits. Reuse an existing Studio session when visual verification is needed. If one extra session is genuinely necessary, track and close that session without disturbing the user's original session or unsaved work. Preserve existing model names, geometry, transforms, and unrelated UI. Do not add demo models unless requested.

Report what changed, checks actually performed, and what still needs Studio or device testing. Never claim a local source edit persisted into a place or passed a visual check without observing it.

## Sources

Reviewed 2026-10-20. Recheck the relevant source when using unfamiliar or changed APIs.

- [Size modifiers and constraints](https://create.roblox.com/docs/ui/size-modifiers)
- [UI positioning and sizing](https://create.roblox.com/docs/ui/position-and-size)
- [GuiButton](https://create.roblox.com/docs/reference/engine/classes/GuiButton)
- [SurfaceGui](https://create.roblox.com/docs/reference/engine/classes/SurfaceGui)
- [Studio plugins](https://create.roblox.com/docs/studio/plugins)
