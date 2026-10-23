# Lobby leaderboards and compact navigation

Use when: designing lobby ranking displays, player showcase areas, overhead labels,
class/service kiosks, queue pads, destination markers, or persistent navigation
around a visible 3D play space. These patterns do not
require a retro theme, wooden board, trophy statues, or the reference palette.

Status: screenshot observations and proposed adaptations, reviewed 2026-10-22.
No scrolling, navigation, persistence, ranking logic, or device behavior was tested.

## Reference observations

Reference: a user-supplied desktop Roblox lobby capture, supplied 2026-10-22,
showing a Total Wins board and a right-side Shop/Stats/Codes rail. The game is
unidentified. Keep the source capture, local attachment path, desktop surroundings,
and visible player identities out of the public skill pack.

- A freestanding board presents a large title, repeated colored rows, rank numbers,
  circular avatar portraits, names, and right-aligned counts. Warm brown surfaces
  and yellow accents contrast with the bright blue/green world.
- Avatar displays on pedestals flank the board, with trophy symbols above them.
  These visually celebrate players, but the capture does not establish their
  precise relationship to ranks or how they update.
- A small currency display and adjacent plus control sit near the top center.
  An Invite Friends label and plus control sit at the top-right. Three vertically
  arranged navigation tiles on the right pair large illustrations with text.
- Most of the screen remains available to view the world and other characters.
  The player's overhead title/name label visibly overlaps several ranking rows.
- The board title says Total Wins while the row values use clears. Their intended
  relationship is not established by the capture.

Do not infer SurfaceGui versus other implementation, exact fonts or measurements,
global versus server rankings, refresh frequency, scroll interaction, purchase
behavior, controller focus, or mobile arrangements. The screenshot proves visible
composition, not that any control works or that the ranking service is reliable.

### Additional capture: service kiosk and queue pads

A second user-supplied lobby capture, also supplied 2026-10-22, shows a Classes
stall with an NPC, a counter, and a bounded circular floor area. Nearby glowing
pads have a NORMAL heading, a 1/4 count, a TOUCH TO CREATE label, and a departure
countdown. A distant marker labels Chapter 3 & More above question-mark shapes.
The currency, invite, and right-edge navigation composition matches the earlier
capture, despite the changed world view.

The overhead player label overlaps the Classes sign. The queue area combines pale
text, overlapping labels, and nearly white glow, making the hierarchy harder to
read. These are observations from one camera angle, not proof the interface is
unreadable from all positions.

The visible wording suggests service access, queue creation, capacity, and travel,
but does not establish trigger volumes, selection rules, party ownership, countdown
behavior, available modes, destination locking, or a successful teleport. Do not
infer class abilities, an economy, or unavailable chapters from the scenery.

## Transferable layout decisions

Keep ranking columns consistent: a narrow rank region, reserved portrait region,
flexible identity region, and stable right-aligned metric region. Prevent long
names from pushing numbers or changing row height unpredictably. Use an explicit
ellipsis or wrap policy and retain access to full identity when it matters.

Name the ranked statistic consistently in the title, values, filters, and player
stats. If wins and clears are distinct concepts, explain the distinction rather
than treating them as interchangeable. Display ranking scope explicitly when
players could confuse global, server, friends, weekly, and all-time lists.

Treat large character displays as optional celebration. Keep them outside the
board's reading area and use a consistent staging convention where comparisons
are intended. Never assume a decorative display represents a specific rank without
data or a label establishing it.

For persistent navigation, pair recognizable icons with short labels and reserve
the central play area. A compact edge rail can work, but its size and placement
must adapt to safe areas, touch movement/jump controls, and other game UI. Match
icon weight, spacing, outline, and label treatment without copying the artwork.
Keep a plus control adjacent to the value or action it affects, and give it an
unambiguous accessible purpose; the symbol alone does not establish its behavior.

## Service kiosks, queues, and destination markers

Let the world communicate where an activity happens: a named kiosk, a recognizable
NPC or prop, a clear approach area, and a contextual action can work together.
Keep the service name readable above or beside the approach area rather than in
the same visual band as player nameplates. Make the actual interaction explicit;
do not assume that stepping on any decorative circle opens a menu.

For a queue, organize the information in a stable order: mode/destination,
occupancy/capacity, current state, then the available action or time to departure.
Separate Create, Join, Full, Starting, and Departing states rather than stacking
all their labels. A countdown needs a subject such as "Departing in 12s" and
should remain understandable when multiple pads are close together. Show the
local player's membership and a way to leave when the design allows it.

Standing on a pad, pressing an explicit prompt, and activating a screen button
are different interaction designs. Choose one that suits the project and input
methods; do not blindly reproduce TOUCH TO CREATE on controller or desktop.
If presence triggers joining, use the server's actual membership state, handle
multiple character body parts and repeated entry, and avoid accidental repeated
join/leave cycles at the edge. These are implementation proposals, not evidence
about the reference game's source code.

Keep glow below the level where it erases silhouettes, pad boundaries, or reading
surfaces. Put queue text on a quiet surface or offset it from bright effects;
heavier strokes alone will not resolve overlapping labels. Preserve distinct
labels when crowded pads, characters, or distant markers project onto one another.

Destination markers should describe what the player can do. Question marks can
communicate mystery but cannot by themselves explain whether content is locked,
unfinished, or selectable. If there is a restriction, display its real reason and
next step; if not, do not invent an unlock requirement based on the screenshot.

Keep queue creation, admission, capacity, countdown deadlines, and travel outcomes
authoritative. Define whether departures accept late joiners and what happens on
owner disconnect, cancellation, or failed transfer. Report a failed or pending
transfer without implying that players reached the destination. Specify and test
these behaviors only if implementing queue/travel functionality is authorized.

## World-space readability and occlusion

Judge a board from reachable player positions and actual camera angles, not just
a close, unobstructed editor view. Provide viewing space and account for players
standing in front of rows. In crowds, overhead labels and decorative billboards
can erase the benefit of well-aligned columns.

Possible adaptations include a locally simplified or distance-limited nameplate,
adjusted board placement and scale, or an optional readable details view. Choose
the least intrusive treatment for the game, explain any loss of player identity,
and preserve names where teammates need them. Do not globally hide all player
labels, force the camera, or make every board render through walls by default.

Distinguish world-space from screen-space ownership. A static SurfaceGui is a
candidate for a world board; a BillboardGui may suit a nameplate; a ScreenGui may
suit persistent navigation. These are proposals, not observed source code. If a
world board becomes interactive, verify documented Adornee, input, ownership, and
focus behavior instead of assuming an in-world visual receives clicks.

## Data and interaction proposals

Use the project's authoritative ranking source. Never infer a global leaderboard
from a visible top-six list or treat client-submitted win totals as valid scores.
Show loading, empty, and failed/stale data states deliberately; do not replace a
failed fetch with fabricated zero wins. Keep the last valid list when appropriate
and avoid visually rebuilding unchanged rows on every refresh.

Scope and time-period changes should retain selection and show which list is
active. If a list scrolls, preserve scroll position during passive updates unless
the user deliberately switches scope. Handle departed players, missing portraits,
ties, long identities, and large totals. These behaviors need product rules and
testing; they were not demonstrated in the reference.

## Verification

After implementation approval, check close/far and oblique views, multiple avatar
heights, a crowded board, overhead-label overlap, long names, and large scores.
Test contrast under the map's actual lighting. Check both bright and shaded scenes
before relying on thin yellow or white lettering on a colored world surface.

On phones, verify navigation against the movement and jump controls and preserve
a usable view of the world. On controller, verify entry, order, visible selection,
activation, and restoration when opening/closing a panel. Test real actions; a
desktop screenshot cannot establish touch or controller support.

For data-driven boards, test refresh failure, empty results, stale results, changed
rank order, missing avatar images, and repeated opening/closing. Verify persistent
score behavior separately from the visual layout. Do not claim backend reliability
or performance from the reference image.

For kiosks and queues, test view angles, nameplate/sign overlap, crowding, different
effect/graphics settings, and long localized status text. If implemented, exercise
simultaneous joins to the last slot, edge re-entry, leave during countdown, owner
disconnect, cancellation, and failed travel. Check that the same membership and
departure state is visible to every affected client, not just the activating player.
