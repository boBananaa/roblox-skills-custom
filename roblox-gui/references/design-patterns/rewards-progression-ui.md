# Rewards and progression panels

Use when: designing screenshot-inspired reward calendars, login bonuses, milestone
grids, or similarly repetitive reward-selection screens. Adapt to the game's
visual language; bright colors, cartoon icons, gradients, and heavy outlines are
an optional direction, not a universal definition of good Roblox UI.

Status: visual observations and proposed adaptations, reviewed 2026-10-22.
No interaction, reward timing, mobile layout, or implementation was tested.

## Reference and evidence boundary

Reference: a user-supplied desktop capture of a Roblox Rewards panel, supplied
2026-10-22. The game is unidentified. Keep the original screenshot private and
outside the public skill repository; publish written analysis rather than the
desktop capture, account context, local attachment path, or extracted artwork.

Observed in this single landscape capture:

- A large centered, translucent dark panel leaves the bright 3D world visible.
  A blue gradient title tab overlaps its upper-left boundary; a red X sits at
  the upper-right. Black outlines and rounded corners define the shell.
- Eight equal-size cards form four columns and two rows. Each card repeats
  day heading, reward quantity, large item illustration, then a bottom action
  or countdown. Headings use decorative white lettering with dark outlines;
  smaller quantities and controls use heavier, simpler lettering.
- Most illustrations show one large blue gem; larger rewards use treasure
  chests. Subtle gray radial backgrounds support the illustrations. Certain
  cards have gold outlines, while others have gray outlines.
- One bottom control reads CLAIM in bright green; future-looking cards show
  countdowns on darker green controls. Gold borders occur on both the claim
  card and countdown cards, so a gold border alone does not establish eligibility.
- Currency and invite controls remain above the panel. A Shop/Stats/Codes rail
  remains visible beside it. Visibility does not establish whether those controls
  remain interactive while the panel is open.

Do not infer the exact font, asset IDs, UDim values, textures, click targets,
animations, clock source, streak rules, reset behavior, or availability from this
image. The numbered days and countdown differences do not prove a particular
daily or playtime-based schedule. The capture does not establish responsiveness,
gamepad navigation, touch usability, or actual server behavior.

## Reuse the hierarchy, not the screenshot pixels

Keep a consistent card anatomy: milestone/day, reward amount and unit, illustration,
then state/action. Align repeated baselines and give each region reserved space.
Item art can dominate the card without covering the quantity or button. Keep
essential descriptions out of busy artwork; test long translations and large
numbers before shrinking text or increasing outline thickness.

Use one clear title and a reliably reachable close action. Overlapping tabs can
give a playful silhouette, but remain inside safe screen bounds and must not
obscure neighboring content. Separate the title's display typography from the
more readable numeric and control typography. Recreate the effect with original
assets rather than copying the reference game's gems, chest art, or font assets.

Reserve extra illustration detail and border emphasis for meaningful milestones.
Use an explicit milestone badge or label if necessary; keep eligibility, keyboard/
controller focus, and milestone importance visually distinct. A locked milestone
must not look claimable simply because it has a gold border.

Choose panel opacity by testing actual backgrounds. Text stroke does not solve
every contrast problem. A vivid world seen through a transparent panel can compete
with reward art and controls. Provide a quiet reading surface or consistent dimming
where needed without requiring every game to use a dark full-screen overlay.

## Define interaction states before implementing

The following are proposed improvements, not behaviors established by the capture:

| State | Communicate | Action behavior |
| --- | --- | --- |
| Loading | Loading rewards; avoid a fabricated zero balance | Claims unavailable until authoritative state arrives |
| Locked | Lock icon/text plus an understandable unlock time | No successful claim; explain when it unlocks |
| Claimable | Strong primary treatment and Claim label | Submit one request and enter pending |
| Pending | Claiming feedback | Prevent repeated local submissions |
| Claimed | Checkmark and Claimed label | No second reward or claim request |
| Failure | Concise reason and a valid retry path | Reconcile state before allowing retry |

Avoid using near-identical green buttons for both unavailable and actionable
rewards. Keep states distinguishable through words and symbols as well as color.
Represent focused/pressed states separately from reward status.

Label time meaning explicitly, for example "Unlocks in 19h 51m". Choose precision
to match the task: a seconds countdown is useful only when that precision matters.
Explain whether eligibility is based on elapsed time, calendar days, playtime, or
another rule after the product rule has been established. Do not copy unusually
long HH:MM:SS strings without checking whether the format remains understandable.

A modal must have a deliberate input policy. If blocking, prevent underlying
shop/currency/navigation actions from firing, manage controller focus inside the
dialog, and restore it on close. If nonblocking, design the exposed navigation
explicitly. A visible side rail in the reference is not proof either policy is good.

## Roblox implementation mapping

Use a ScreenGui and a constrained centered shell for an in-game screen. A repeated
card component and UIGridLayout can express the desktop grid. Adapt column count
to available width and a useful card size; use a ScrollingFrame for overflow rather
than squeezing all eight cards into a phone viewport. Preserve reward order and
state information when reflowing. Keep the heading and close action accessible
while the card list scrolls. Use padding, size/aspect constraints, and dedicated
image regions to keep artwork from changing text/control geometry.

Use consistent outline, corner, and type roles through the project's theme. An
ImageLabel can hold original reward artwork; a low-contrast radial image or other
supporting treatment is optional. Choose specific component properties against
the existing UI framework and current Roblox documentation when implementing.

Clients display eligibility; the server authorizes it. A local countdown reaching
zero is not proof a reward can be granted. Validate the reward, unlock condition,
and prior claim, and ensure retries/concurrent requests cannot grant it twice.
Derive timers from authoritative timing information and reconcile on reopening or
rejoining. Return the resulting claim state and balance to the view. Define how
claim records persist before claiming a cross-session reward system is complete.

## Verification

After implementation approval, test a narrow phone and a wide desktop, long reward
names, large amounts, and countdowns beyond 24 hours. Check artwork and outlines
over bright and dark gameplay scenes, readable numeric text, touch-sized targets,
controller focus, close behavior, and the actual intended modal policy.

Test claimed/locked/claimable contrast without relying on color; loading, delayed
responses, rejection, rapid double activation, close/reopen during a request,
reconnect, and two simultaneous claim attempts. Verify both UI feedback and the
single server-side reward grant. A screenshot comparison alone verifies none of
these interactions or persistence guarantees.
