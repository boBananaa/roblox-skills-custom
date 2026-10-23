# Lobby leaderboards and compact navigation

Use when: designing lobby ranking displays, player showcase areas, overhead labels,
or persistent navigation around a visible 3D play space. These patterns do not
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
