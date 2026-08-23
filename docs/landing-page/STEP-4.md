# Step 4 — Trending Now

## What was built

- A new `sections/trending-now.liquid`: a full-bleed, tinted-background section with a header row (heading + prev/next arrow buttons) above a horizontally-scrollable row of real Shopify products, sourced from a merchant-selected collection. The section renders nothing until a populated collection is configured.
- The theme's first product-loop section: unlike Step 3's category cards, product cards are **not** a curated block type — they loop `section.settings.collection.products` directly, so image, title, and price always match real inventory with no manual re-entry.
- A JS-driven slider (the theme's second `{% javascript %}` block, after Step 1's header mobile-nav dialog): prev/next buttons smooth-scroll the track by one card-width, using the same `querySelectorAll('.trending-now').forEach(...)` + `data-*` attribute scoping pattern as `sections/header.liquid`, so behavior stays correct if the section type is ever added twice to a template.
- New icons: `assets/icon-arrow-left.svg` (mirrored arrow, no existing CSS-mirroring precedent in this theme) and `assets/icon-heart.svg` (wishlist), both following the existing 20×20 `stroke="currentColor"` convention.

## Files changed

New:
- `sections/trending-now.liquid`
- `assets/icon-arrow-left.svg`
- `assets/icon-heart.svg`
- `docs/landing-page/STEP-4.md` — this file.

Modified:
- `templates/index.json` — added a `trending` entry (type `trending-now`) after `categories`, with `"collection": "all"` (Shopify's implicit "All products" handle) and `product_limit: 8`, so the section shows real content on first load rather than an empty band.
- `locales/en.default.json` — extended the existing `general.accessibility` namespace with `previous`/`next`/`add_to_wishlist`, and added a new `products.new_badge` key (no `products.*` namespace existed before this step).
- `locales/en.default.schema.json` — added `general.trending_now`, `labels.collection`, `labels.product_limit`. Reused the existing `labels.heading`. Also fixed a pre-existing trailing-comma JSON syntax error in the `options.text_style` block (unrelated to this step, but this file needed a valid save).
- `snippets/css-variables.liquid` — added one new token, `--color-secondary-container-glass` (`color_modify: 'alpha', 0.2`), following the exact existing pattern of `--color-surface-glass`/`--color-outline-variant-glass`, for the section's translucent tinted background.
- `docs/landing-page/PROGRESS.md` — checked off "Trending Now".

## Design decisions & rationale

- **Real Shopify products via a `collection` picker, not a curated block type — per explicit user decision.** Unlike Step 3's category cards, prices/images/titles come straight from `product.price | money`, `product.featured_image`, and `product.title`, so they can never drift from live inventory and require no merchant re-entry. This also means there is no per-item merchant control over card content — the tradeoff is accepted as part of the decision.
- **"New" badge bound to `product.tags contains 'New'`**, not a metafield or manual toggle. Every merchant already has tags and can bulk-edit them from the product list; a metafield would require defining one in Settings → Custom data before anything could ever show, and there's no block/setting per product to hang a manual toggle on since the loop is collection-driven.
- **Wireframe's subtitle line ("Taupe / Italian Wool") mapped to `product.type`, omitted when blank.** Rejected alternatives: first variant title (renders the literal string `"Default Title"` on simple products — visibly broken), vendor (usually the brand, not the material), a new metafield (setup burden, no existing metafield convention in this theme). `product.type` is a plain field every product already has and degrades gracefully.
- **Wishlist button is decorative only — no click handler, no persistence.** The theme has no cart/wishlist backend or localStorage precedent anywhere; faking a toggle would silently not persist across sessions/devices, which is worse than an inert button. This is a known, intentional scope limitation.
- **Arrow buttons only render when the collection has more than one product** (`section.settings.collection.products.size > 1`) — Liquid can't reliably compute actual per-breakpoint overflow, so this is the simplest rule that avoids a "stuck" arrow with nothing to scroll to.
- **Section is `full-width`** (like Hero), with the header row and product track independently constrained/padded via `--content-grid` and `--page-margin` — needed because, unlike Curated Categories, this section has both a full-bleed tinted background *and* an edge-bleeding scroll track.
- **DESIGN.md exceptions kept from the wireframe** (same rigor as Step 3's documented exceptions):
  1. `.trending-now__wishlist` is circular (`border-radius: 50%`) — DESIGN.md mandates sharp 0px corners; kept circular per wireframe, same category as the header's circular account avatar (Step 1 precedent).
  2. The wireframe's `shadow-sm` on the wishlist button was **dropped**, not carried in as an exception — DESIGN.md's "no shadows, use opacity fades" is unambiguous, unlike the rounded-corner or hover-zoom cases, and the button reads fine shadow-free against the image.
  3. The "New" badge uses `text-label-caps` (the wireframe's literal class) rather than DESIGN.md's Chips/Tags spec of `label-sm` — wireframe wins per Step 3's established precedent when the two conflict.
  4. `.trending-now__card-image img` keeps the wireframe's literal `500ms` hover-zoom duration rather than the `--motion-fade` token (200ms), matching Step 3's category-card zoom precedent.
  5. `.trending-now__title` adds a scoped `font-weight: 500` — no existing typography utility has medium weight; this is a section-local override rather than a new global utility for one usage.
- **JS scoping**: reused `sections/header.liquid`'s `querySelectorAll(...).forEach(...)` + `data-*` attribute pattern instead of the wireframe's global `id="slider-prev"`/`id="product-slider"`, so the section works correctly even if duplicated in a template.
- **New global token `--color-secondary-container-glass`** was added rather than using CSS `color-mix()` inline, because the theme's established convention for translucent colors is a precomputed `color_modify` filter in `snippets/css-variables.liquid` (see `--color-surface-glass`, `--color-outline-variant-glass`) — no `color-mix()` precedent exists anywhere in this codebase.

## Responsive behavior

- **<768px**: cards are 280px wide, track padding uses `var(--page-margin)` (20px mobile), header row wraps via `flex-wrap` if the heading and buttons don't fit on one line.
- **≥768px**: cards are 320px wide; track padding switches to the desktop margin (64px) automatically via the existing `--page-margin` media query — no new breakpoint logic was needed.

Verified visually at ~1440px (desktop: 320px cards, tinted band, hover zoom + wishlist fade-in, prev/next scroll confirmed) and ~390px (mobile: 280px cards, single-card-plus-peek horizontal scroll, arrows and heading render correctly below the fixed header) via `shopify theme dev`.

## Design tokens introduced

One: `--color-secondary-container-glass` in `snippets/css-variables.liquid` (see rationale above). Everything else reuses tokens/utilities built in Steps 1–3: `--spacing-gutter`, `--spacing-section-gap`, `--page-margin`, `--content-grid`, `--color-outline`/`-variant`, `--color-surface-dim`/`-container`/`-glass`, `--motion-fade`, `.text-headline-md`/`.text-body-md`/`.text-label-sm`/`.text-label-caps`.

## How to review/preview

1. `shopify theme dev` and open the homepage.
2. Below Curated Categories, confirm a tinted band with "Trending Now" heading and two square prev/next arrow buttons.
3. Confirm real products from the "All products" collection render with image, title, `product.type` subtitle (when set), and price.
4. Hover a card: image zooms slightly (1.05, 500ms) and a circular wishlist heart icon fades in top-right.
5. Click the next/prev arrows: the row scrolls smoothly by roughly one card width; no console errors.
6. Tag one product `New` in the Shopify admin and confirm the badge appears only on that card.
7. In the theme editor, change the section's Collection setting and confirm the grid updates live; clear it and confirm the section disappears cleanly (no broken empty markup); adjust "Number of products" and confirm the loop is capped.
8. Resize to mobile (~390px): confirm 280px cards, horizontal scroll-snap, and that the header row and arrows still render correctly below the fixed site header.
9. `shopify theme check` — should report only the same 3 pre-existing `RemoteAsset` warnings, no errors.

## Checklist

- [x] PROGRESS.md updated
- [ ] Committed
