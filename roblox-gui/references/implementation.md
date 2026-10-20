# Roblox UI implementation and review

## Layout and rendering context

First inspect the project's UI root and hierarchy. ScreenGui instances normally render through PlayerGui; StarterGui holds authored templates. Decide whether the screen should survive respawns, and choose ResetOnSpawn plus controller ownership accordingly. A controller surviving respawn still needs to release and rebind character-dependent subscriptions.

For an existing GUI, modify the smallest responsible component. Avoid spawning a second HUD on initialization or every CharacterAdded. Give UI roots stable ownership and make teardown/reinitialization predictable in the project's framework.

| Need | Useful native tools | Check |
| --- | --- | --- |
| Centered panel | AnchorPoint, relative Position, UISizeConstraint | Narrow and ultrawide screens; avoid tiny touch targets. |
| Content-driven stack | UIListLayout, UIPadding, AutomaticSize | Wrapping/localization and child-size feedback loops. |
| Inventory grid | UIGridLayout with bounded cell sizes or calculated columns | Selected item stays visible when columns change. |
| Scrollable content | ScrollingFrame, AutomaticCanvasSize | Correct scroll axis, padding, and footer outside scroll region. |
| Aspect-sensitive art | UIAspectRatioConstraint, appropriate ScaleType | Preserve content and avoid distorting the whole layout. |
| Responsive text | Readable type scale, wrapping, constraints where TextScaled is used | Long labels, text expansion, minimum legible size. |
| Safe screen placement | ScreenGui.ScreenInsets and documented inset settings | Top bar, device cutouts, touch controls. |

Use scale for relationships and offsets for useful fixed padding/minimums. Neither one is universally better. TextScaled alone does not establish legibility, and AutomaticSize does not eliminate layout constraints. Test combinations rather than blindly applying every modifier.

BillboardGui/SurfaceGui use an Adornee to refer to world objects. For interactive world-space UI, follow the class documentation: place the GUI in PlayerGui and set Adornee, plus the required interaction properties (including SurfaceGui.Active and the target part's CanQuery where applicable). A visible world label is not evidence its embedded button receives input.

## Interaction and focus

- Use GuiButton.Activated for shared pointer/touch/controller activation, with framework bindings matching its actual API.
- Provide visible controller selection and meaningful navigation order; preserve/restore focus when dialogs open and close. Hover is supplementary, not the only way to discover an action.
- Model selection, pressed, disabled, pending, and success/error states separately from hover color.
- When a dialog blocks the screen, manage DisplayOrder/ZIndex and use an input-catching backdrop. Gate gameplay actions while the dialog is active and handle keyboard/controller focus explicitly. A decorative transparent Frame is not automatically an input blocker.
- GuiButton.Modal relates to mouse locking behavior; it is not a general dialog/input-isolation switch. GuiObject.Active can sink pointer input, but it does not alone solve all keyboard, controller, or game-action routing.
- Keep the close path and selection restoration available. If dismissal is disabled during a transaction, explain why and provide a useful pending state rather than freezing the interface.

## State and server integration

Keep the view a projection of state. Connect an existing store/controller or a small state model before adding ad hoc events to every widget. Inventory data and money belong to the authoritative server state.

For asynchronous actions:

1. Record the current screen/request generation and enter a pending state.
2. Send minimal intent through the established network API.
3. Receive a correlated result and authoritative state/revision; display only if the screen/request is still current.
4. On timeout or close, stop waiting in the view without assuming the server operation was canceled.
5. Reconcile current server state on reopening/retry. Never mint rewards because the UI did not receive a reply.

Use a request ID to associate responses, not as proof the request is authorized. A client-side debounce improves UX but does not secure the transaction.

## Motion and lifecycle

Design motion around purpose: reveal hierarchy, explain a state change, or acknowledge an action. Prefer a small set of durations/easing choices consistent with the game. Honor available reduced-motion/accessibility preferences and provide an alternative when the project exposes them. Do not infer exact timing from a still image.

Cancel/replace an existing tween before animating the same properties. A canceled tween can still trigger completion handling; inspect the playback state and current transition generation before running completion effects. Do not let an old closing transition destroy a reopened screen.

The component/controller that creates an instance, connection, task, tween, or binding owns its cleanup. Use the installed cleanup abstraction if available. Framework effects/hooks require their matching cleanup semantics; a raw connection in render can leak across rerenders.

Separate screen lifetime, character lifetime, and request lifetime. After repeated open/close/respawn cycles, no stale view should continue receiving updates or capture input.

## Useful review scenarios

Choose scenarios relevant to the actual screen rather than generating tests for every property.

| Scenario | Observable result |
| --- | --- |
| Narrow phone + long item names | Primary action remains usable; text wraps/truncates intentionally. |
| Controller opens/closes dialog | Focus enters dialog, cannot activate obscured actions, and restores afterward. |
| Double press under latency | UI is coherent and server commits the permitted action once. |
| Screen closes before response | No destroyed-instance update or stale success banner on a newer screen. |
| Character respawns | Persistent screens stay singular; character-bound UI reconnects once. |
| Rapid open/close during tween | Final visible state matches intent; no old completion destroys it. |
| Server rejects action | Error/pending state clears and authoritative inventory/balance remains correct. |
| Repeated cycles | No accumulating listeners, tasks, instances, or memory trend from the screen. |

## Sources

Reviewed 2026-10-20.

- [UI sizing](https://create.roblox.com/docs/ui/position-and-size)
- [Constraints and automatic sizing](https://create.roblox.com/docs/ui/size-modifiers)
- [GuiButton](https://create.roblox.com/docs/reference/engine/classes/GuiButton)
- [GuiObject](https://create.roblox.com/docs/reference/engine/classes/GuiObject)
- [ScreenGui](https://create.roblox.com/docs/reference/engine/classes/ScreenGui)
- [SurfaceGui](https://create.roblox.com/docs/reference/engine/classes/SurfaceGui)
- [BillboardGui](https://create.roblox.com/docs/reference/engine/classes/BillboardGui)
- [TweenBase lifecycle](https://create.roblox.com/docs/reference/engine/classes/TweenBase)
