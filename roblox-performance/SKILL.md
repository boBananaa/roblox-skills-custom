---
name: roblox-performance
description: Diagnose and improve Roblox client or server performance, memory, loading, networking, streaming, UI cost, and frame-time bottlenecks using measured evidence.
---

# Roblox performance

## Measure before prescribing

Identify the symptom, target device/input, representative player count, scene, and whether the cost is client CPU, GPU, server simulation, memory, network traffic, or loading. Inspect existing profiling and optimization code before replacing it. Frame rates vary; per-frame work is not inherently 60 Hz.

Before implementation, propose the suspected bottleneck, evidence needed, limited change, affected files/instances, and before/after measurement. Wait for approval unless that concrete plan is already approved. Reuse existing libraries and instruments; adding a profiler dependency is not automatically necessary.

Read [diagnosis.md](references/diagnosis.md) for streaming correctness, profiling choices, pooling, and a UI-oriented performance pass.

## Reliable choices

- Use MicroProfiler, Developer Console, Scene Analysis, and script profiling to narrow the problem. A Studio desktop result alone does not establish mobile performance or a full server's cost.
- Capture a comparable baseline and repeat the same workload after a change. Report frame-time distribution and observed memory/traffic where relevant, not just a momentary FPS maximum.
- Prefer event-driven changes for state that does not need a frame loop. Cache stable references where worthwhile, but invalidate them on respawn, destruction, and streaming changes.
- Do not impose a universal ban on FindFirstChild/GetChildren in frame callbacks or claim every cache is a large win. Inspect frequency, instance count, allocation, and measured cost.
- Use task scheduling according to required timing; task.spawn is not a performance-equivalent replacement for every coroutine pattern. task.wait does not guarantee exact intervals.
- Introduce pooling for demonstrated allocation/churn costs. Reset state, cancel old work, bound retained capacity, and prevent a stale delayed callback from returning a reused object.
- Treat visual LOD, streaming presence, physics cost, and replication as different mechanisms. Making parts transparent does not necessarily eliminate their physics, networking, or all rendering work.
- Avoid universal MeshPart/Union claims, arbitrary dynamic-light limits, or merging geometry without checking culling, collision, draw calls, and reuse.

## Streaming

Configure Workspace.StreamingEnabled and the documented non-scriptable streaming properties in Studio. Do not generate runtime assignments to them. Use Player:RequestStreamAroundAsync for an intentional preload request, handling yield/failure and partial availability; a request is not a permanent pin or a guarantee of full loaded content.

Use ModelStreamingMode for Atomic/Persistent/PersistentPerPlayer behavior. LevelOfDetail controls visual representation, not whether a model is always present. Persistent models are exceptional; overusing them defeats streaming's resource benefits. Client code must tolerate instances appearing/disappearing and bind/unbind safely.

## Verification and workflow

Check both the resource improvement and preserved behavior. Include respawns/streaming, repeat open/close cycles for UI, pool reuse and teardown, and realistic load when relevant. Optimize a Studio plugin's edit-time event/update lifecycle separately from a running game's frame loop.

Prefer direct files, APIs, and saved profiler evidence. Reuse the user's existing Studio session; if an extra session is genuinely needed, track and close it while preserving unsaved work. Do not claim device or live-server results from static source inspection.

## Sources

Reviewed 2026-10-20.

- [Identify performance issues](https://create.roblox.com/docs/performance-optimization/identify)
- [Instance streaming](https://create.roblox.com/docs/workspace/streaming)
- [ModelStreamingMode](https://create.roblox.com/docs/reference/engine/enums/ModelStreamingMode)
- [Player:RequestStreamAroundAsync](https://create.roblox.com/docs/reference/engine/classes/Player#RequestStreamAroundAsync)
