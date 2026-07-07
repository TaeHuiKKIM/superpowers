# UI, Game Feel, And Visual Asset QA

Use this for frontend, game, mockup, sprite, tutorial, character, or visual polish work.

## Design Goals

- Preserve the product's current emotional identity unless the user explicitly wants a new art direction.
- Separate system and concept: system is what the player can learn/do; concept is why the fantasy feels alive.
- Optimize the first 5 seconds and first 15 minutes. Put the next obvious action above optional information.
- Keep visible choices limited. If a screen feels dense, separate sections with rhythm, color, and hierarchy instead of adding more text.
- Prefer actual visual assets for game fantasy: characters, items, room objects, thumbnails, and store/social imagery.

## First-Screen Checklist

Check mobile and desktop:

- Primary action is visible without scrolling.
- Header/profile, cash/net worth, action buttons, news, and nav have distinct visual hierarchy.
- News or tips do not push the main action out of view.
- Sticky elements are actually viewport-attached and do not overlap modals/sheets.
- Large numbers do not cover buttons or adjacent text.
- Disabled/cooldown buttons keep stable labels, not only timers.

## Tutorial And Help

- Use short steps with visuals, not dense manual text.
- Verify replay path and first-run path.
- Test 360px, 390px, and desktop widths.
- Check arrows/dots do not move between slides.
- Use one primary scroll owner inside modals.
- Add a vertical-drift guard for swipe gestures when horizontal swipes exist.
- In review-safe routes, remove sensitive copy from visible UI and searchable DOM.

## Generated Image Policy

Use generated raster assets when:

- CSS/SVG placeholders feel cheap or fail the game fantasy.
- A sprite, mockup, thumbnail, store image, or background needs real visual richness.
- The user explicitly asks to make images.

Use repo-native SVG/CSS instead when:

- The asset is a simple icon in an existing icon system.
- Deterministic shapes are enough.
- The visual must be tiny, themeable, or code-driven.

When generating:

1. Use the `imagegen` skill.
2. Keep prompt constraints aligned with the current art direction.
3. Preserve intentionally goofy/cute character feeling if the user asked for it.
4. Generate variants, inspect them, select one, and save it under the repo.
5. Update references in code, manifests, service worker, README/docs, and tests.
6. Verify the asset in the actual UI, not only as a standalone image.

## Visual Regression Checks

For every changed UI surface, verify:

- Element boxes are inside viewport.
- No text overflow or accidental horizontal scroll.
- Important objects do not overlap.
- `display`, `visibility`, opacity, and natural image size are correct.
- Mobile safe areas and bottom nav are correct.
- Screenshots show the expected visual change.

For game room/item scenes, include item matrix tests:

- Each purchased item appears.
- Unpurchased items stay hidden.
- Watch/character accessories are attached to the character if that is the fantasy.
- Cars, jets, homes, or other flex items are large enough to be felt.
- Window, wall art, character, and objects do not collide.
