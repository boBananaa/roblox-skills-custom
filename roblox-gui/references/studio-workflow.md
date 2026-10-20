# Studio, external files, and plugin UI

## Discover the source of truth

Inspect existing paths/configuration and ask only for information that cannot be established directly. An empty workspace does not mean the place is empty. Do not force a move to Rojo or set up sync merely to answer a design question.

| Existing setup | Work within it |
| --- | --- |
| Studio + Script Sync | Edit verified synced script paths; use Studio for authored non-script content. |
| Rojo/project mapping | Inspect mapping, project files, dependencies, and exclusions before editing. |
| Manual Studio workflow | Prepare clearly placed source/assets or use available approved edit APIs; explain the insertion step if no direct edit path exists. |
| Unknown | Identify place/project and available tools first; do not write into guessed folders. |

Prefer terminal, existing APIs/connectors, and direct file operations. Use screen control when a visual or input behavior check actually requires it. Available tool access is not proof of a live connection to the intended place; verify the target before editing.

## Script Sync onboarding, when requested

1. Identify the authoritative scripts in Studio and choose a new empty local destination for initial export.
2. Sync an appropriate script folder through Studio's Sync to… action. Inspect the conflict preview; Keep Studio means Studio's scripts win, while Keep Disk makes the disk version win. Do not guess in a non-empty destination.
3. Verify the exported script paths and types before editing them externally.
4. Explain that later edits flow in both directions while sync is active. Team Create can bring shared changes into Studio and then onto disk; concurrent editors of the same scripts can overwrite each other.

Script Sync synchronizes Script, LocalScript, ModuleScript, and Folder instances. It does not carry arbitrary models, parts, images, or their authored properties. Tags/attributes are not preserved by Script Sync; inspect that risk before creating/deleting or resolving older versions. A folder's presence on disk is not proof its map or GUI instances were exported.

Current naming rules distinguish script types: `.luau` is a ModuleScript; `.local.luau` is a LocalScript; `.client.luau` and `.server.luau` are Scripts with Client/Server RunContext; `.legacy.luau` and `.plugin.luau` select those Script contexts. Read the project's actual mapping instead of renaming established files. Rojo conventions and Script Sync conventions are not interchangeable.

Keep `.rbxl`/`.rbxlx` backups outside synced roots and version control. Preserve the original place and unsaved work. Reuse the current Studio session; create at most one extra session if genuinely necessary, identify it, and close only that extra session when done. Do not relaunch sessions repeatedly to work around a lost automation connection.

For authored GUI/model changes, distinguish runtime construction from persisted edit-mode content. A runtime preview disappears when play stops. Verify intended edit-mode content in the data model, and perform save/reopen verification only when saving/reopening is authorized and unsaved work is protected.

## Studio plugin panels

Use a plugin-owned DockWidgetPluginGui for docked tooling, with the current Plugin API and the project's existing UI library. A dock widget is not a ScreenGui under PlayerGui. Do not assume Players.LocalPlayer, a live server, or character lifecycle in edit mode.

Separate three kinds of state:

- Tool preferences: plugin settings or the tool's existing preference layer.
- Authored place content: documented instances/attributes under the chosen project location.
- Temporary panel state: selection, pending edits, and current view, released on unload.

Validate Selection results and allowed target types before mutation. Display the intended target/action and operate only within the approved scope. Preserve names, hierarchy, transforms, and existing models unless the requested change requires them.

For undoable authored changes, use ChangeHistoryService:TryBeginRecording before mutation and FinishRecording with the appropriate operation afterward. If recording cannot begin, do not proceed as though undo is available. Design failure handling to restore partially changed objects; do not assume canceling a recording automatically undoes every mutation. Keep user-facing undo units meaningful and avoid recording mere hover/selection UI updates.

Disconnect Selection listeners, toolbar bindings, input handlers, tasks, and owned UI on plugin teardown/unload. Check repeated reloads so the plugin cannot duplicate panels or callbacks. Publishing the plugin is a separate action from implementing its source.

## Sources

Reviewed 2026-10-20.

- [Script Sync, scope, conflicts, and naming](https://create.roblox.com/docs/scripting/sync)
- [Studio plugins](https://create.roblox.com/docs/studio/plugins)
- [Plugin API](https://create.roblox.com/docs/reference/engine/classes/Plugin)
- [ChangeHistoryService](https://create.roblox.com/docs/reference/engine/classes/ChangeHistoryService)
