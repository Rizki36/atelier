# Step 1 — Product Media Gallery

## What was built

- Rebuilt the stock `sections/product.liquid` media block (previously a flat `.product-images` loop with no gallery, thumbnails, or zoom) into the wireframe's three-part gallery: a desktop-only vertical thumbnail rail, a large center main image with a zoom affordance, and a mobile-only horizontal snap-scroll thumbnail strip.
- Extracted the gallery into a new `snippets/product-media-gallery.liquid` so the section stays a thin composition layer, per `AGENTS.md`'s snippet guidance ("reusable code fragments … not directly edited in the theme editor").
- Thumbnail-click interaction (desktop rail + mobile strip, kept in sync with each other) swaps the active main image and updates the active thumbnail's styling — implemented with the snippet's own `{% javascript %}` tag (vanilla `addEventListener`, no build step), matching the pattern already used for `trending-now.liquid`'s carousel buttons. All of a product's media images render up front as stacked `.gallery__main-image` divs; only the active one is displayed, avoiding any client-side image-swap/network-request logic.
- A zoom button overlay on the main image (hover-revealed, per the wireframe), using a new `assets/icon-zoom.svg` following the theme's existing SVG icon set convention (`assets/icon-*.svg` via `inline_asset_content`) rather than the wireframe's Material Symbols placeholder. The wireframe's "+" thumbnail-rail affordance was dropped — it had no defined destination/behavior in this step's scope (no lightbox yet), so an icon asset for it was not added.

## Files changed

New:
- `snippets/product-media-gallery.liquid` — LiquidDoc header (`@param {product} product`), renders `product.media`, own `{% stylesheet %}` + `{% javascript %}`.
- `assets/icon-zoom.svg` — zoom affordance icon, matches existing icon set's viewBox/`currentColor`/`var(--icon-stroke-width)` convention.
- `docs/pdp/STEP-1.md` — this file.

Modified:
- `sections/product.liquid` — replaced the stock `.product-images` div with `{% render 'product-media-gallery', product: product %}`.
- `locales/en.default.json` — added `general.accessibility.zoom_image`, `general.accessibility.view_image` (thumbnail alt-trigger labels).

No changes to `templates/product.json`, `config/settings_schema.json`, or other sections.

## Design decisions & rationale

- **Snippet, not inline markup in the section.** The gallery has no merchant-editable settings of its own (it's always driven by `product.media`), so per `AGENTS.md` it belongs in `snippets/`, not as a block — there is nothing here a merchant would add/remove/reorder in the theme editor.
- **Reuse `snippets/image.liquid`** for every `<img>` in the gallery instead of hand-rolling `image_url`/`image_tag` calls, to stay consistent with every other image in the theme (`trending-now.liquid`, `intentional-living.liquid`).
- **Desaturated filter + 4:5/3:4 aspect ratios** per `DESIGN.md`'s "Image Placeholders" component spec — same `filter: saturate(0.75)`-style treatment already applied to `trending-now`/`intentional-living` images, so the PDP doesn't introduce a visually inconsistent product shot.
- **No new cross-component dependency.** This step is fully self-contained and independently reviewable before Step 2 adds variant-driven media swapping on top of it.

## Responsive behavior

- **<768px**: main image full-width, thumbnail rail hidden, horizontal snap-scroll thumbnail strip (`overflow-x: auto; scroll-snap-type: x mandatory;`) below the main image, matching the wireframe's mobile layout exactly.
- **≥1024px** (theme's existing `lg` breakpoint for structural layout switches, per `assets/critical.css`): 12-column grid — thumbnail rail (1 col), main image (6 cols), info panel begins at col 9 (built in Step 2).

To verify: `shopify theme dev`, open a product page, check ~390px and ~1440px viewports.

## Design tokens introduced

None — reuses `var(--spacing-gutter)`, `var(--color-outline)`, `var(--radius)` (0px sharp corners per `DESIGN.md`), `var(--motion-fade)` for the hover/opacity transitions.

## How to review/preview

1. `shopify theme dev`, open any product page.
2. **Desktop (~1440px)**: confirm the vertical thumbnail rail on the left, clicking a thumbnail swaps the main image and highlights the active thumbnail; hover the main image and confirm the zoom button fades in. Verified against `selling-plans-ski-wax` (3 images) at 1440px — thumbnail click swap and active-state highlighting both work.
3. **Mobile (~390px)**: confirm the thumbnail rail is hidden and a horizontal snap-scroll strip appears below the main image. Not verified in-browser this pass (viewport resize was unreliable in the automation tooling used); the mobile layout is the CSS's unconditional base state (`.gallery__rail { display: none }` / `.gallery__strip { display: flex }`, only overridden above the 1024px `min-width` query), the same mobile-first pattern already shipped in `trending-now.liquid`/`footer.liquid`/`intentional-living.liquid`. Please confirm on a real device/viewport before merging.
4. `shopify theme check` — 3 pre-existing baseline warnings (remote font assets in `layout/theme.liquid`), no new errors or warnings introduced.

## Checklist

- [x] PROGRESS.md updated
- [ ] Committed
