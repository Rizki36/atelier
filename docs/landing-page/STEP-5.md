# Step 5 — Designed for Intentional Living

## What was built

- A new `sections/intentional-living.liquid`: the wireframe's "Editorial Spotlight" block — a static, constrained-width (not full-width) two-column section pairing an editorial image with a heading, richtext body, a short feature list, and a secondary CTA button. Column order swaps responsively: image first on mobile, content-left/image-right on desktop (≥768px), matching the wireframe's `order-1 lg:order-2` / `order-2 lg:order-1` behavior.
- The feature list is **block-based**, like Step 3's category cards, not a data loop like Step 4's products — this is curated brand-philosophy content, not live inventory, so the merchant-editable-blocks pattern applies.
- A new `blocks/feature-item.liquid` theme block: icon (select: Precision/Texture) + title + description, with a short hairline divider rendered via `::before` on every block after the first (matching the wireframe's short `w-12` divider between the two feature rows) rather than extra per-row divider markup.
- Two new icons, `assets/icon-precision.svg` and `assets/icon-texture.svg` (20×20, `stroke="currentColor"`, following the existing icon convention), replacing the wireframe's Material Symbols (`architecture`, `texture`) which the theme doesn't use — the same kind of substitution Step 4 made for its own icons.

## Files changed

New:
- `sections/intentional-living.liquid`
- `blocks/feature-item.liquid`
- `assets/icon-precision.svg`
- `assets/icon-texture.svg`
- `docs/landing-page/STEP-5.md` — this file.

Modified:
- `templates/index.json` — added a `story` entry (type `intentional-living`) after `trending`, pre-populated with the wireframe's two default feature-item blocks (Structural Precision / Tactile Materials) so the section isn't empty on first load. `image` and `button_link` are left blank — no existing `shopify://shop_images/...` placeholder matches this section's editorial photo, and inventing a filename that doesn't exist in the dev store's file library would render broken in the editor preview. The section degrades gracefully without an image (see below).
- `locales/en.default.schema.json` — added `general.feature_item` ("Feature"), `general.intentional_living` ("Intentional Living"), `labels.feature_description`, `labels.feature_title`, `labels.icon`. Reused the existing `labels.heading`, `labels.body`, `labels.image`, `labels.button_label`, `labels.button_link`.
- `docs/landing-page/PROGRESS.md` — checked off "Designed for Intentional Living".

No changes to `locales/en.default.json` — the section has no interactive JS and no new runtime-facing strings (icons are `aria-hidden` decorative).

## Design decisions & rationale

- **Feature list is block-based (`blocks/feature-item.liquid`, max 4), not hardcoded settings fields.** Follows Step 3's precedent (curated block content) rather than Step 4's (collection loop), since this is merchant-authored brand copy, not product data. `max_blocks: 4` gives a little headroom over the wireframe's 2, the same ratio of headroom Curated Categories gives its 3-card default (max 6).
- **Reused `blocks/category-card.liquid`'s exact `filter: saturate(0.75)` desaturation** for the editorial image instead of inventing a new value — the wireframe's `desaturate-25` class and DESIGN.md's "desaturated filter" guidance are already satisfied by the existing precedent, so no new exception needed (unlike Steps 3–4, which each had to document a couple of literal wireframe-vs-DESIGN.md conflicts).
- **Reused `snippets/button.liquid`'s `style: 'secondary'`** for the "Read the Journal" CTA — it already renders the wireframe's exact outline-then-invert-on-hover treatment with zero new CSS.
- **No new CSS custom properties.** The decorative offset block behind the image reuses the existing `--color-secondary-container` token with a plain `opacity: 0.5` (matching the wireframe's `bg-secondary-container/50`) rather than adding a new `-glass`-style alpha token like Step 4 did — that token was needed for a blurred, layered glass surface; this is a flat decorative shape where runtime `opacity` is sufficient and matches existing usage elsewhere (e.g. `category-card__cta`'s `opacity: 0.8`).
- **Section is constrained-width, not `full-width`.** The wireframe wraps this block in `max-w-container-max mx-auto px-margin-mobile md:px-margin-desktop` (unlike Hero/Trending Now's full-bleed treatment), matching Curated Categories' precedent — it renders as an ordinary child of `.shopify-section`, centered automatically, no `--content-grid` needed internally.
- **Section/block left without a starter image** (see Files changed above) — a documented, intentional scope limitation rather than guessing at a nonexistent asset filename; the section's `{% if section.settings.image %}` guard means it still renders cleanly (heading, body, features, CTA) with the image column and decorative block simply absent until a merchant picks an image.

## Responsive behavior

- **<768px**: single column, image (when set) appears above the text content, decorative offset block hidden (`display: none`), feature list and CTA stack normally.
- **≥768px**: two-column grid (`5fr 6fr`), content column left (`order: 1`) with `padding-right`, image column right (`order: 2`); the decorative offset block becomes visible, positioned `-32px` bottom/right behind the image at 66% width/height.

Verified visually at ~1440px (desktop: text left, image right, offset block peeking out bottom-right, divider between the two feature rows) and ~390px (mobile: single column, image-then-content order, no decorative block) via `shopify theme dev`.

## Design tokens introduced

None — reuses `--spacing-section-gap`, `--spacing-gutter`, `--color-secondary-container`, `--color-outline`/`-variant`, `--color-on-surface`/`-variant`, `.text-headline-md`/`.text-body-md`/`.text-label-caps`/`.text-label-sm` from prior steps.

## How to review/preview

1. `shopify theme dev` and open the homepage.
2. Below Trending Now, confirm a new section: heading "Designed for Intentional Living", body copy, two feature rows (Structural Precision / Tactile Materials, each with an icon and a hairline divider between them), and an outline "Read the Journal" button.
3. In the theme editor, set the section's Image setting and confirm it renders at a 4:5 ratio, desaturated, with a beige decorative block peeking out behind it at desktop widths.
4. Add/remove feature-item blocks (up to the max of 4) and confirm the divider only appears between rows, never above the first.
5. Resize to mobile (~390px): confirm the image (if set) moves above the text, the decorative block disappears, and everything stays legible in one column.
6. Resize to desktop (~1440px): confirm the two-column split with content on the left, image on the right.
7. `shopify theme check` — should report only the same pre-existing warning count as Step 4, no new errors.

## Checklist

- [x] PROGRESS.md updated
- [x] Committed
