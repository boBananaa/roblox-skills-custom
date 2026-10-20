# Roblox Skills Custom

A collection of seven Roblox development skills for AI coding assistants, maintained by [boBananaa](https://github.com/boBananaa).

**Status: initial instruction pack.** The skills support planning, implementation, and review. They are not a Roblox runtime library, a Studio plugin, or a guarantee that generated game code is production-ready. No framework or installer is bundled.

## Skills

| Skill | Focus |
| --- | --- |
| [roblox-gui](roblox-gui/SKILL.md) | HUDs, menus, responsive layout, input, state, Studio panels, and screenshot-inspired design. |
| [roblox-animations](roblox-animations/SKILL.md) | Rig playback, replication, blending, interruption, and lifecycle. |
| [roblox-datastores](roblox-datastores/SKILL.md) | Profile ownership, reliable saves, migration, recovery, and library integration. |
| [roblox-monetization](roblox-monetization/SKILL.md) | Passes, durable developer-product receipts, purchase UI, and current policy sources. |
| [roblox-performance](roblox-performance/SKILL.md) | Profiling, streaming, UI cost, memory, and measured optimization. |
| [roblox-remote-events](roblox-remote-events/SKILL.md) | Message contracts, validation, authority, responses, and request lifetime. |
| [roblox-security](roblox-security/SKILL.md) | Interaction permissions, economic races, physics trust, and exploit review. |

## How the pack works

UI is the main emphasis, with the other skills supporting the systems behind it. Each skill is self-contained and loads detailed references only when relevant.

- Inspect the existing game and available tools before choosing a workflow.
- Present a concrete plan before implementation; continue when that plan is approved.
- Reuse suitable existing frameworks, libraries, components, and conventions.
- Distinguish running-game code from Studio edit-mode plugins and authored place changes.
- Prefer direct files/APIs, preserve existing models and unsaved Studio work, and verify the actual result.
- Consult official Roblox documentation for unfamiliar APIs and changing platform rules.

No particular UI framework, persistence library, networking wrapper, genre, or sync system is mandatory. The pack can support Studio/Script Sync, Rojo, or manual authoring after identifying the actual project setup.

## Use locally

Each `roblox-*` directory contains `SKILL.md`, supporting references, and optional Codex UI metadata in `agents/openai.yaml`. Install selected skill folders through your agent's supported skill workflow. For Codex, you can ask the built-in skill installer to install the desired folders from this repository, or copy the entire chosen folders into the project's `.agents/skills` directory. Consult the [current Codex skill documentation](https://developers.openai.com/codex/skills/) for discovery locations and installation behavior.

Inspect existing skills with the same names before installing; do not overwrite a different pack or assume two identically named skills are merged. Copy each complete folder with its references, not just SKILL.md. No `npx` command, install hook, or automatic update mechanism is included.

Example requests after installation:

```text
Use $roblox-gui to inspect my current UI stack and plan a responsive inventory screen.

Use $roblox-monetization to review how our receipt handler integrates with saved profiles.

Use $roblox-performance to propose a measured investigation of inventory-opening stutters.
```

Installing these skills does not connect an agent to Studio or a private game, and does not modify an experience. Source checks, mocked behavior, Studio visuals, multi-client replication, device performance, and live service behavior are separate validation results.

## Grow the design guidance

Send screenshots with what you like and whether the direction is for one project or a lasting preference. The GUI skill's [design-learning workflow](roblox-gui/references/design-learning.md) turns observations into scoped techniques, Roblox implementation proposals, and verified notes. It distinguishes visible evidence from inferred behavior and does not assume exact fonts, animation timings, or source code from a still image.

There are no screenshot-derived patterns yet. Future public updates can include written analysis and original examples; private screenshots and third-party images are not uploaded by default. Skill changes and publication follow the approved plan.

See [MAINTAINING.md](MAINTAINING.md) for validation and source-review practices and [EVALUATION.md](EVALUATION.md) for the initial independent review and its limits. The initial official-documentation review date is **2026-10-20**; changing APIs, policy, pricing, and library versions must be rechecked when used.

## Background

Inspired by [sentinelcore/roblox-skills](https://github.com/sentinelcore/roblox-skills), reviewed at commit `f2b1910a7fb898ed35cf2f856e2a2e48e38276bf`. Credit to sentinelcore for the original pack and topic selection.

The instructions and examples in this pack were newly authored with AI assistance around the maintainer's requirements and official Roblox references. Upstream skill files are not included. Attribution does not imply that sentinelcore maintains, reviewed, or endorses this version. Any future incorporation of upstream material requires appropriate permission and attribution.
