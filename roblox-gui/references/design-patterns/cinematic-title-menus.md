# Cinematic title menus

Use when: designing a title screen or a small main menu where a strong world scene
and original branding should establish the game's personality. Avoid forcing this
composition onto dense inventories, technical panels, or routine gameplay HUDs.

Status: screenshot observations and proposed adaptations, reviewed 2026-10-22.
No camera movement, menu transitions, input, performance, or device behavior tested.

## Reference and expressed preference

Reference: a private user-supplied desktop capture showing a PHIGHTING! title menu,
supplied 2026-10-22. The user explicitly called it "really cool UI". Record this as
an appreciated visual direction: a vivid scene, expressive oversized branding,
strong contrast, and sparse navigation. It is an option to consider on fitting
projects, not a mandate to imitate this game or apply its palette everywhere.

Do not publish the desktop capture, local attachment path, unrelated desktop
context, or extracted logo/art. Design original assets for the target game.

Observed:

- A colorful city scene fills the game view. A low, tilted perspective, tall
  structures, foreground forms, and distant architecture create a strong depth
  impression. Blue sky and cool structures contrast with warmer building edges.
- A large, slanted, white hand-drawn-style logo with a thick black silhouette
  occupies the upper-right region and meets or extends beyond the right edge.
- PLAY, QUICK PLAY, and CREDITS form a sparse group below the logo, using bold
  white lettering with substantial black outlines. The labels have different
  sizes and slight angles rather than identical conventional button rectangles.
- The scene remains the dominant image; the main choices have no visible card
  shells. Native Roblox controls remain at the upper-left, with faint chat UI
  elsewhere on that side.

The capture does not establish whether the backdrop is a live scene, image, or
special preview; whether camera motion exists; what Play versus Quick Play does;
or whether any choice is selected, hovered, or animated. It cannot establish exact
font names, rotations, assets, dimensions, motion timing, or mobile layout.

## Build one composition, not a menu over arbitrary scenery

Compose the backdrop and interface together. Choose a recognizable focal structure,
foreground/midground/background separation, and a quieter region for navigation.
Let the environment communicate setting before filling the screen with panels.
A rich background plus a sparse menu is useful only when the next action remains
easy to find.

Use a deliberate logo silhouette and limited expressive typography. Large display
lettering can establish character while shorter, clearer action labels handle
navigation. Apply visual tilt sparingly and keep a coherent reading order. Do not
make every label a different size or angle merely to create novelty.

White lettering on a busy scene needs robust contrast. A dark outline or backing
shape can support the silhouette without a full modal panel, but test counters,
letter gaps, thin strokes, and small labels. Avoid relying on excessively thick
text strokes that close the letters. Preserve the identity of the background
instead of automatically adding a heavy dark wash.

Treat edge-cropped branding as a choice to adapt, not permission for accidental
clipping. Keep essential title identity and every actionable label readable across
aspect ratios. On a narrow phone, reposition or scale the logo and menu as a
group; do not simply crop the desktop composition until actions disappear.

## Clear behavior beneath expressive presentation

The visible label can be free-form while its interactive region remains generous,
predictable, and non-overlapping. Provide visible hover/focus and pressed feedback
without layout jumps. Controller selection and touch activation must not depend
on detecting a narrow painted letter or a desktop hover effect.

Explain or visibly distinguish similar entry choices such as Play and Quick Play.
Do not invent their semantics from this reference. For the target game, establish
whether they mean mode selection, party setup, direct matchmaking, or something
else. Keep secondary information subordinate to the actual play action.

Plan loading, connecting, unavailable, failed-join, and cancel/back states. Repeated
activation must not launch concurrent join flows. Credits should have a reachable
return path. These are proposed behaviors, not observed reference interactions.

## Roblox mapping and verification

Use a safe-area-aware ScreenGui for interactive menu controls and an original logo
asset where appropriate. Choose scene presentation independently: a maintained
menu scene/camera, a suitable preview, or approved static artwork may fit different
budgets. Do not assume the reference uses any specific technique. If taking camera
control, own its setup, cleanup, and restoration when entering gameplay.

Animation is optional. If proposed, use restrained movement, preserve legibility
during motion, account for reduced-motion needs, and test actual device cost.
A single still reference does not justify claims about parallax or an orbiting
camera.

After implementation approval, inspect wide and narrow screens, safe-area edges,
translated labels, letter readability over changing backgrounds, and mouse/touch/
controller hit areas. Verify that users can identify the intended play path.
Exercise repeated clicks, slow/failed joins, backing out, and returning from
credits. A beautiful title screenshot is evidence of composition, not proof of
usable navigation, responsive layout, or a reliable game-entry flow.
