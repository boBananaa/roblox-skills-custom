# Maintaining the skill pack

## Change the skill that owns the decision

Keep triggers concise and topic-specific. Do not make all seven skills load for every Roblox request. Put shared essential behavior in each independently installable entrypoint, and longer conditional guidance in that skill's references. Avoid cross-skill file dependencies: users can install only one skill.

Preserve the plan-first preference and existing authorization. A plan should describe the actual approach, files/instances, integration, and checks; it is not a fixed ceremony for every read-only lookup. Once the concrete work is approved, complete it without repeatedly requesting the same permission. Installation, production-data changes, publishing, and spending are not implied by a request for advice.

Prefer instructions that improve decisions over copied API manuals or large untested code samples. Examples must distinguish real Roblox APIs, project-specific APIs, illustrative pseudocode, and incomplete integration sketches. Do not present an untested persistence/purchase sketch as a production system.

## Source review

Use current official Roblox documentation for API semantics and platform rules. For an installed third-party framework/library, use its actual version and official project documentation. Do not recommend a dependency merely because an older skill named it. Keep the source links close to the claims they support and record the review date for materially changed guidance.

Verify linked pages actually describe the feature, not just that the URL returns a success response. Do not infer animation priority from enum numeric values. Treat business-policy values, library APIs, and feature availability as potentially changing.

For screenshot-derived guidance, follow [the design-learning workflow](roblox-gui/references/design-learning.md). Keep project-specific choices scoped and observations separate from tested implementation. Do not publish private content or a third-party reference image by default. Avoid creating pattern files before there is a concrete technique to document.

## Validate before committing

For each changed skill:

1. Run the available skill-creator `quick_validate.py` against its directory. Check metadata descriptions/default prompts and reference discovery as well as frontmatter syntax.
2. Check relative links, code fences, and the full diff. Remove scaffold TODOs, broken links, unused resources, credentials, private paths, and generated test artifacts.
3. Verify new or changed examples appropriately. A Luau parser/type check does not establish engine or service integration; pure helper tests do not establish transaction safety.
4. Exercise a realistic task using the skill. For complex or risky changes, use an independent evaluator with the skill and a natural request, without feeding it the desired answer. Keep its artifacts outside the repository until reviewed.
5. Fix actionable findings and recheck affected cases. Report remaining limits rather than turning missing Studio access into a claim of success.

Useful behavior scenarios include screenshot-to-UI translation in an existing framework; touch/controller interaction; a stale response after UI close/reopen; a plugin operating in edit mode; duplicate/concurrent receipts; failed profile loads and stale saves; animation interruption/replication; and a measured performance investigation. Select scenarios relevant to the actual change.

## Publish the approved diff

Review repository status and remote branch before committing. Include only intended skill/docs changes. Do not add place backups, private screenshots, audit downloads, personal configuration, or evaluator scratch files. Push only when publication is authorized, then verify the remote commit and expected files. Avoid force-pushing or overwriting unrelated remote work.

Installing a skill is a separate operation from publishing its source. This repository does not silently install itself or automatically update local skill folders.
