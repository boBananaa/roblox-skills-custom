# Performance diagnosis and verification

## Choose evidence that answers the symptom

| Symptom | First evidence | Useful comparison |
| --- | --- | --- |
| UI stutters when opening inventory | Client MicroProfiler/script time, layout/instance creation, allocation | Same inventory size and device before/after; opening and scrolling. |
| Server slows with players | Server frame/script/physics time, network traffic | Representative active player counts and identical actions. |
| Memory rises over time | Instance/connection/task ownership, memory categories, scene analysis | Repeated open/close/respawn cycles after warmup. |
| Slow join/teleport | Asset/content loading, replication, streaming | Same destination and comparable network conditions. |
| Effects cause frame spikes | Spawn/destroy churn, GPU/particles, draw calls, physics | Same burst sizes; pooled versus nonpooled after warmup. |

Capture where the data came from: Studio, Roblox client, server profiler, device model, graphics settings, player count, and workload. Use the current shortcut/menu for the platform rather than copying an obsolete keyboard shortcut. MicroProfiler currently appears in the official tools table as Ctrl+Alt+F6 / Command+Option+F6; recheck when giving step-by-step UI help.

## A practical UI pass

For a large inventory, first inspect whether the screen recreates every cell on each balance change or attaches listeners on every rerender. Separate item identity from transient view state, reduce unrelated updates, reuse framework reconciliation, and virtualize only when the item count/cost warrants it. Pooling or virtualization adds focus, selection, and lifecycle complexity; verify those behaviors.

Avoid repeated whole-tree scans merely to update one component. Cache stable references with clear invalidation. Batch related updates where supported and useful. Prefer a state event over a per-frame text assignment for a balance that changes only on actions. A countdown may need a coarse tick; a drag may legitimately need frame updates.

Evaluate large transparent layers, effects/strokes, clipping, and redraw costs on target devices. Do not declare any one visual property universally expensive without measurement. Make one justified change, repeat the workload, and inspect appearance and input as well as timings.

## Streaming corrections

Workspace streaming settings documented as non-scriptable must be authored in Studio. Do not place `workspace.StreamingEnabled = true` in a runtime Script. A prefetch request belongs to Player:RequestStreamAroundAsync, not Workspace, and may yield or fail. Use its supported timeout semantics and readiness checks for the actual critical objects; streaming remains dynamic.

Model.LevelOfDetail affects visual representation. It does not pin instances or replace ModelStreamingMode. Use Atomic when parts must arrive/depart together; choose Persistent/PersistentPerPlayer only for the limited situations requiring that behavior. Account for device memory and the cost of keeping content resident.

Streaming out is not necessarily equivalent to Destroy. A cached reference can exist while its instance is no longer in the active hierarchy. Check membership/readiness, stop dependent work, and reconnect safely on re-entry. Recheck static assumptions around respawns and streamed models.

## Pool lifecycle

If profiling supports a pool, define an explicit acquire/reset/use/release contract. Reset physics, appearance, ownership, event connections, task/tween state, and parent as applicable. Bound capacity; an idle pool can retain too much memory.

Prevent double return and callbacks belonging to an earlier lease. A generation/token per acquire can make an old delayed expiry harmless after the object has been returned and reused. When a pool is torn down, cancel pending callbacks and destroy retained objects. Verify repeated leases under overlap rather than only the first spawn.

## Report a useful outcome

Record baseline, change, comparable after measurements, behavior checks, and tradeoffs. If the test environment is unavailable, provide a measurement procedure and label the change unverified. Do not replace a missing baseline with an invented percentage improvement.

See [diagnostic tools](https://create.roblox.com/docs/performance-optimization/identify), [instance streaming](https://create.roblox.com/docs/workspace/streaming), and [ModelStreamingMode](https://create.roblox.com/docs/reference/engine/enums/ModelStreamingMode). Reviewed 2026-10-20.
