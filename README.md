# Roblox Skills Custom

A collection of seven Roblox development skills for AI coding assistants, maintained by [boBananaa](https://github.com/boBananaa).

**Status: evolving instruction pack.** The skills support planning, implementation, and review. They are not a Roblox runtime library, a Studio plugin, or a guarantee that generated game code is production-ready. No framework or installer is bundled.

## Start with the UI designer

[**Explore the UI patterns**](#ui-design-patterns) · [Install](#install) · [All seven skills](#skills) · [Help](#help-and-contributions)

Use `roblox-gui` to turn a screen idea or reference into a concrete Roblox UI plan: layout, visual hierarchy, input, state, and checks for the actual project. Supporting patterns cover rewards, lobby HUDs, character selection, cinematic menus, and medieval interfaces.

A first request to try after installing:

```text
Use the roblox-gui skill to inspect my existing UI setup and plan a character
selector with a roster, character preview, role details, and ability information.
Adapt it for phone and controller input. Show the plan before editing.
```

**Expected result:** a project-specific plan with proposed components, states, and validation steps. After approval, the assistant can help implement it using the tools available in your environment. Installing instructions alone does not connect it to Roblox Studio.

## Install

You need an AI coding assistant that supports agent skills. The CLI route also needs Node.js 22.20 or later and network access. Check existing skills with the same names before installing so you can preserve local customizations.

Run this from your game project's folder to install the UI designer for OpenCode:

```bash
npx skills add boBananaa/roblox-skills-custom --skill roblox-gui -a opencode
```

For Codex, replace `-a opencode` with `-a codex`. Add `-g` for installation across projects. These commands use the third-party [Skills CLI](https://github.com/vercel-labs/skills/blob/main/docs/commands.md), which downloads the skill instructions and supporting files.

Want the full pack or another assistant? Choose interactively:

```bash
npx skills add boBananaa/roblox-skills-custom
```

### Manual OpenCode setup

Download the repository using GitHub's **Code → Download ZIP**, then copy the complete `roblox-gui` folder into your game project:

```text
.opencode/
  skills/
    roblox-gui/
      SKILL.md
      references/
      agents/
```

Keep the references with `SKILL.md`. Open your project in OpenCode and ask it to load the `roblox-gui` skill. It should appear among the available skills; if it does not, check the folder nesting, exact `SKILL.md` filename, and skill permissions. See [OpenCode's skill discovery and troubleshooting guide](https://opencode.ai/docs/skills/) for details.

Manual copies do not update themselves. Before replacing one, compare any local edits with the repository version. Other assistants can use the complete skill folders through their supported installation workflow.

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

## UI design patterns

Choose a pattern for the screen you are building. These are reusable techniques with stated limits, rather than a single required art style.

- **[Character and loadout selection](roblox-gui/references/design-patterns/class-loadout-ui.md):** coordinate the roster, preview, role tags, ability information, and selected character's accent color; frame large equipment without covering controls.
- **[Cinematic title menus](roblox-gui/references/design-patterns/cinematic-title-menus.md):** combine an expressive title, a vivid scene, and a small set of readable actions.
- **[Rewards and progression](roblox-gui/references/design-patterns/rewards-progression-ui.md):** organize repeated cards, emphasize milestones, and distinguish claimable, locked, and claimed rewards.
- **[Lobby leaderboards and HUDs](roblox-gui/references/design-patterns/lobby-leaderboards-hud.md):** align rankings, label compact navigation, clarify queue information, and check world-label occlusion.
- **[Medieval and fantasy UI](roblox-gui/references/design-patterns/medieval-fantasy-ui.md):** use themed resource frames and compact tool layouts when that visual direction fits.

These notes include written observations from supplied references and proposed Roblox adaptations. They do not establish the source games' implementation, animations, or interaction behavior. Private screenshots and third-party reference images are not bundled.

To add a reference, tell the assistant what you like and whether it is for one screen or a lasting preference. The [design-learning workflow](roblox-gui/references/design-learning.md) separates visible evidence from inference and records how a proposed technique should be checked.

## Scope and validation

The pack supplies instructions, supporting references, and optional Codex UI metadata in `agents/openai.yaml`. It bundles no runtime library, Studio plugin, installer, or automatic updater. It does not modify an experience on installation.

The [initial evaluation](EVALUATION.md) records source checks and independent scenario walkthroughs from **2026-10-20**, along with their limits. Later UI patterns have their own evidence and verification notes. Neither a walkthrough nor a screenshot analysis establishes working game code, device performance, purchase delivery, or multiplayer behavior. Changing APIs and platform policies must be checked when used.

## Help and contributions

For an unclear instruction or proposed improvement, [open an issue](https://github.com/boBananaa/roblox-skills-custom/issues). Include the affected skill, your assistant and project setup, the task you attempted, and the observed problem. Remove credentials, private game details, and personal information from examples.

For a change, read [MAINTAINING.md](MAINTAINING.md), keep the diff focused on the skill that owns the decision, and explain what you checked. Preserve the distinction between observed references, proposed behavior, and tested implementation.

## Background

Inspired by [sentinelcore/roblox-skills](https://github.com/sentinelcore/roblox-skills), reviewed at commit `f2b1910a7fb898ed35cf2f856e2a2e48e38276bf`. Credit to sentinelcore for the original pack and topic selection.

The instructions and examples in this pack were newly authored with AI assistance around the maintainer's requirements and official Roblox references. Upstream skill files are not included. Attribution does not imply that sentinelcore maintains, reviewed, or endorses this version. Any future incorporation of upstream material requires appropriate permission and attribution.
