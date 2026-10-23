# Class and loadout selection

Use when: designing class or hero selection, equipment loadouts, slot-based character
customization, or a class-roll screen explicitly requested by the user. A class
selector does not imply a random acquisition system, purchasable boosts, or this
reference's cartoon styling.

Status: visual observations and proposed adaptations, reviewed 2026-10-22.
No selection interaction, rolling, purchase, animation, or device behavior was tested.

## Reference observations

Reference: a private user-supplied desktop Roblox Classes capture, supplied
2026-10-22. The game is unidentified. Do not publish the original capture, local
attachment path, player identity, desktop surroundings, or extracted artwork.

- The left column lists nine classes as illustrated horizontal tiles with names
  and percentages. A pale border emphasizes the Starter tile.
- A large character appears in the center within a furnished 3D setting. The
  current class title and opposing arrow shapes sit above the character.
- The right side has a blue Slot 1 control and a locked +1 Slot offer with a
  displayed Robux price. Below is a translucent class-details panel containing
  a title, short description, and Equipped status. A large red Back control
  sits separately below it.
- A large green Spin Class control at the bottom center shows a 50-gem cost.
  Nearby offers advertise Fast Rolls, 2X Luck, and two gem packages, with
  Robux-icon prices. The top currency counter displays zero gems.
- Decorative class-tile art suggests different themes. Numeric labels and price
  rows use simpler lettering than the decorative class names. The world remains
  visible between the UI regions and through parts of the details panel.

The displayed percentages are 40, 25, 15, 10, 6, 3.39, 0.5, 0.1, and 0.01; they
sum to 100%. That arithmetic does not validate real outcome probabilities. The
image does not establish whether a boost is active, how luck modifies weights,
whether duplicates exist, what a roll replaces, or what the selected border and
arrows do. It also does not prove the green action is enabled at zero balance.

Do not infer exact fonts, asset identities, camera behavior, world versus viewport
preview implementation, slot persistence, product types, or purchase success.

### Comparative reference: four PHIGHTING! roster captures

Four additional private desktop captures supplied 2026-10-22 show Biograft,
Ban Hammer, Coil, and Medkit within the same selection composition. Treat them
as a set of visible selection states, not an observed recording of the transition.
Keep screenshots, chat identities, desktop context, and extracted character art
out of the public skill pack.

- A large angled heading occupies the upper-left. The center/right is dominated
  by the candidate character and its equipment, against a consistent city scene.
- A right-side roster keeps a consistent sequence of named horizontal strips.
  In each capture, the row matching the large preview expands into a brighter
  portrait banner, while other rows remain darker and more compact.
- A lower-left information cluster changes with the candidate: a prominent name,
  three short descriptor tags, a star-based Difficulty display, and an A/D skin
  hint. Beneath it is a row of five ability-like silhouettes; their meanings and
  interaction are not established by the captures.
- Candidate-specific accents connect the name, descriptor tags, highlighted row,
  and character treatment: orange and violet are visible among the examples,
  while Medkit uses turquoise/green. The accent relationship is more useful than
  copying any individual color.
- The framing varies with the model: Ban Hammer's large equipment spans much of
  the scene; other candidates show different silhouettes and proportions. The
  displayed roster entry remains the clearest link between list and preview.

These captures establish coordinated differences between displayed candidates.
They do not establish actual abilities, difficulty values or calibration, selected
versus committed/equipped state, skin cycling, team availability, input hit areas,
model rotation, animations, or transition timing. Do not infer that black icons
are inactive or locked, or that all descriptor tags are gameplay roles.

## Coordinated hero identity without changing the layout

Keep browse, preview, metadata, and ability details in stable regions while
switching candidates. Update them from one candidate identity so the portrait,
name, role information, model, and actions cannot briefly describe different
characters. If preview loading is asynchronous, reject stale completions from a
previous candidate and show a clear loading/fallback state when an asset fails.

Use a shared accent token to connect the candidate across regions, supplemented
by the character's name and portrait. Keep role, mechanical attributes, and flavor
descriptors distinguishable; an expressive adjective is not a substitute for
explaining what the character does. Do not rely on color alone to communicate
candidate identity or selected state.

An expanded selected roster row can strengthen the connection to the preview.
If implementing this treatment, preserve browsing order and keep the active row
visible without surprising scroll jumps. Selection focus, hover, availability,
and confirmed choice need independent treatments. Do not assume the highlighted
candidate is equipped until the product's selection contract says so.

Preserve a consistent sequence for ability slots across characters when the game
has corresponding inputs or roles. Provide names and concise explanations on
focus/selection, with an equally usable touch path; expressive silhouettes alone
do not teach unfamiliar abilities. Define what a difficulty scale measures and
supplement stars with a readable value or description when needed. Do not copy
the reference's implied rating system without a design decision.

Frame previews against reserved UI bounds and actual model/equipment extents.
Show enough silhouette and signature equipment to make comparisons useful while
protecting the roster and metadata. Check both the largest weapon and smallest
character; intentional cinematic cropping must not hide identifying features or
essential controls. Avoid uncontrolled camera zoom changes on every switch.

Map the visible A/D hint to the target game's actual input scheme if skin cycling
is implemented. Provide discoverable touch/controller alternatives and identify
the current skin. Keep skin changes distinct from changing character, purchasing,
or confirming a loadout; the screenshot does not prove any of those interactions.

## Preserve the browse, preview, and details relationship

Use a stable relationship between three tasks: browse classes, inspect the current
candidate, then take the relevant action. Show the class name, bonus, availability,
and active slot together. Selection/highlight, equipped state, owned state, and a
newly rolled result are distinct concepts; name them explicitly rather than using
one border color for all of them.

The preview should help evaluate the class. A large unchanged avatar alone does
not communicate an ability difference. Where useful, add original equipment,
pose, effects, or a short demonstrable ability preview, while giving the same
information in text. Treat that behavior as a proposed addition, not an observed
feature of the reference.

Reserve predictable space for names, percentages, and controls. Use artwork as
support, not as the only way to distinguish classes. Keep descriptions on a quiet
surface; a transparent panel over scenery can compete with its own text. Long
class names, localized descriptions, and tiny fractional percentages must remain
readable without constantly changing the type scale.

## Clarify actions, slots, and costs

Choose the primary action from the actual task: Equip for an owned candidate,
Unlock for a known acquisition path, or Roll only for a deliberately designed
random acquisition flow. A large green button is not automatically the right
action for every class screen.

Keep the action cost adjacent to its label and distinguish earnable currency from
Robux through icons and explicit names where needed. If funds are insufficient,
show the missing amount and an understandable unavailable state. Never assume a
green-looking button proves affordability or that buying gems should happen
automatically when someone tries to roll.

Keep secondary offers visually subordinate and spaced away from the main action.
Multiple large purchase buttons can compete with class information and create
accidental activations. Preserve a clear Back/Close path without placing it in
the same action group as purchases. Do not invent product IDs, prices, or whether
an offer is permanent, consumable, or temporary from the screenshot.

Represent active, inactive, locked, and unavailable slots distinctly. Before a
reroll feature is built, establish whether it replaces the class in the active
slot, stores a result, or offers a later replacement choice. Make that consequence
visible before activation; do not silently overwrite a valuable loadout. Do not
add slot purchases or extra prompts to a deterministic selector merely because
they appear in this reference.

## Random acquisition, only if requested

If paid randomness is part of the actual design, consult the current
[Roblox paid random items guidance](https://create.roblox.com/docs/production/monetization/paid-random-items)
before implementation; reviewed here on 2026-10-22. Robux-funded currency rolls
are included. Disclose actual outcomes and numerical odds before spending, and
update displayed odds for active modifiers. A 2X Luck label alone does not explain
its numerical effect. Enforce applicable per-player eligibility through
PolicyService rather than relying on the screen's visible controls. These rules
do not establish that the photographed game implements them correctly.

The server chooses and commits an outcome with its cost once. Clients display
the result; a spinner animation must not independently choose it or grant it.
On close/reopen or delayed response, reconcile the committed class and balance.
Purchasing an offer, receiving currency, spending it on a roll, and equipping a
result are separate operations with separate success/pending/failure states.

## Roblox mapping and device adaptation

A repeated class-row component inside a ScrollingFrame can express the list.
Use a constrained details panel and deliberate layout changes at narrow widths;
do not shrink three desktop columns and all purchase offers into tiny phone UI.
For example, let players browse a list, inspect details, and act in one reachable
region, keeping active-slot context visible. This is a proposed flow to test.

For the character preview, select an actual world camera or a ViewportFrame based
on whether the game needs live surroundings and teammates visible. The screenshot
does not establish which method was used. Own any preview camera/rig lifecycle
and restore control when closing. Distinguish decorative arrows from genuinely
interactive previous/next controls; if interactive, provide usable hit areas,
labels, and controller focus.

## Verification

After implementation approval, test selected versus equipped versus newly obtained
states; no active slot; locked/full slots; insufficient funds; long names and
descriptions; smallest displayed probabilities; loading; slow/rejected actions;
and close/reopen or respawn while an action is pending.

If rolls exist, test rapid double activation, concurrent requests, duplicates,
disconnect/rejoin after charging, reroll replacement semantics, modifier changes,
and restricted-user flows. Verify server outcomes match the disclosed distribution
and that animation speed settings do not change acquisition semantics. Test
purchases through the project's real fulfillment integration, not button appearance.

Check keyboard/controller focus order between list, slots, details, primary action,
and Back; ensure phone controls remain reachable and the preview does not consume
all reading space. Static observations do not establish any of these behaviors.

For coordinated roster previews, switch candidates rapidly, including before
model loading finishes. Check that all regions settle on the same identity, old
rigs/connections are released, keyboard/controller focus survives roster expansion,
and the largest equipment does not cover controls. Exercise unavailable candidates,
missing portraits/models, long descriptor tags, and touch access to ability details.
