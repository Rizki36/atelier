# Step 5 — "Effortless Structure" Styling / Editorial Section

## What was built (planned)

- A new standalone section, `sections/product-styling.liquid`, for the wireframe's supplementary "How to Wear" / editorial block below the main product info: a vertical "STYLING" eyebrow label, a headline, body copy, a "Shop The Look" CTA link, and two offset images with a decorative low-surface backdrop behind them.
- Settings mirror `sections/intentional-living.liquid`'s shape (the closest existing analog — a heading + richtext + link + image(s) editorial section): `heading` (text), `body` (richtext), `cta_label`/`cta_url` (or a single `url` link setting), and two `image_picker` settings for the offset image pair.
- Added as a second entry in `templates/product.json` (after the `main` product section), so merchants can reorder, hide, or duplicate it like any other standalone section — not hardcoded inside `sections/product.liquid`.

## Files changed

New:
- `sections/product-styling.liquid` — markup, `{% stylesheet %}`, `{% schema %}` with `presets` (so it's also addable to other templates, e.g. a future lookbook page, not just locked to the product template).
- `docs/pdp/STEP-5.md` — this file.

Modified:
- `templates/product.json` — add a second section entry (e.g. `"styling": { "type": "product-styling", "settings": {} }`) to `sections`, and append `"styling"` to `order`.
- `locales/en.default.schema.json` — add `general.product_styling` (section name); reuse `labels.heading`, `labels.image`, `labels.button_label`, `labels.button_link` (or equivalents already used by `hero.liquid`/`intentional-living.liquid`) rather than inventing new label keys.
- `locales/en.default.json` — default heading/body copy only if the section ships with placeholder defaults (matching how other sections default their text settings), not a runtime string.

No changes to `sections/product.liquid`, the gallery/variant-picker snippets, or the cart drawer — this section is fully decoupled from the rest of the PDP.

## Design decisions & rationale

- **Standalone section, not baked into `sections/product.liquid`.** Consistent with every other "supplementary content block" on the landing page (`intentional-living.liquid` is its own section, not part of `hero.liquid`) — keeps `sections/product.liquid` focused on the core buy-box (media, info, accordion) and lets a merchant hide/reorder/remove the editorial block independently.
- **Reuses `intentional-living.liquid`'s settings shape** (heading/richtext/link/image-pair) rather than designing new schema conventions, since the wireframe's structure — eyebrow label, headline, body, CTA, offset image pair with a decorative backdrop — is functionally the same component type already proven in Step 5 of the landing page.
- **`presets` included** so the section is available from the theme editor's "Add section" picker generally, not only pre-seeded in `templates/product.json` — matches `trending-now.liquid`/`intentional-living.liquid`'s own preset pattern.
- **No new CSS custom properties** — reuses `var(--spacing-section-gap)`, `var(--color-surface-container-low)` (for the decorative backdrop, matching `intentional-living.liquid`'s own low-surface decoration), `var(--color-primary)` for the CTA underline.

## Responsive behavior

- **<768px**: single column — eyebrow label, headline, body, CTA stacked above the (stacked) image pair, matching `intentional-living.liquid`'s own mobile collapse of its asymmetrical desktop grid.
- **≥768px**: 12-column grid, text content in a left column (cols 2–5), offset image pair in a right column (cols 7–12) with one image vertically offset above the other, per the wireframe's `mt-12` stagger.

Verify at ~390px and ~1440px via `shopify theme dev`.

## Design tokens introduced

None — reuses tokens listed above.

## How to review/preview

1. `shopify theme dev`, open a product page, scroll past the accordion to confirm the styling section renders below it.
2. **Desktop (~1440px)**: confirm the offset two-image layout and decorative backdrop.
3. **Mobile (~390px)**: confirm the section collapses to a single stacked column.
4. In the theme editor: confirm the section's heading/body/CTA/images are all editable, and that the section can be reordered or removed from the product template without breaking the rest of the page.
5. `shopify theme check` — expect only the existing baseline warnings, no new errors.

## Implementation notes

- Built as planned: `sections/product-styling.liquid` with eyebrow/heading/richtext body/CTA settings and two `image_picker` settings (`image_1`, `image_2`) for the offset image pair, reusing `intentional-living.liquid`'s settings shape and design tokens.
- Wired into `templates/product.json` as a second `"styling"` entry after `"main"` in `order`.
- Added `general.product_styling` to `locales/en.default.schema.json`; all other `t:labels.*` keys (`heading`, `body`, `button_label`, `button_link`, `image`, `eyebrow`) already existed and were reused as planned.
- Verified live: `shopify theme check` shows only the pre-existing baseline warnings (no new errors); desktop (~1440px) rendering confirmed via `shopify theme dev` — eyebrow/headline/body/CTA render correctly with the decorative backdrop grid on the right. Mobile collapse relies on the same `@media (min-width: 768px)` single-column-below breakpoint already proven in `intentional-living.liquid`.

## Checklist

- [x] PROGRESS.md updated
- [x] Committed
