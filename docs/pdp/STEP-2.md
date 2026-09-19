# Step 2 — Product Info Panel & Variant Picker

## What was built

- `.product` two-column layout wrapping the Step 1 gallery and a new `.product__info` panel: breadcrumb, `<h1>` title, price (regular or sale + strikethrough compare-at, via the `money` filter), and the variant picker.
- New `snippets/product-variant-picker.liquid`, driven by `product.options_with_values` and `product.variants`. Renders a circular swatch button per value for a `Color`/`Colour` option, and a rectangular `label-caps` chip button for every other option, per `DESIGN.md`'s "Chips/Tags" spec.
- The picker's own `{% javascript %}` reads a `<script type="application/json">` payload of pre-formatted (Liquid `money`-filtered) variant data, tracks the selected option values, resolves the matching variant client-side, and on change: updates the price markup, writes the resolved id into the cart form's hidden `id` input, reflects the choice in the URL via `history.replaceState` (no reload), and dispatches a `pdp:variant-change` `CustomEvent` carrying the variant.
- `snippets/product-media-gallery.liquid` (Step 1) now tags each thumbnail/main-image with `data-media-id="{{ media.id }}"` and listens for `pdp:variant-change`, swapping to the variant's `featured_media` when present — the cross-component wiring described in the Step 1/2 plan.
- Unavailable option combinations are disabled (native `disabled` + `aria-disabled="true"`) rather than hidden, recomputed after every selection.

## Files changed

New:
- `snippets/product-variant-picker.liquid`

Modified:
- `sections/product.liquid` — `.product` grid wrapper, breadcrumb, `<h1>`, price markup, renders the variant picker, cart form reduced to hidden `id`/`quantity` inputs (the dropdown `<select>` is superseded by the visual picker; the description paragraph was dropped here since it will live in the Step 4 details accordion instead).
- `snippets/product-media-gallery.liquid` — `data-media-id` attributes + `pdp:variant-change` listener; the two thumbnail-set click handlers were refactored into a shared `activateIndex()` used by both the click handler and the new listener.
- `locales/en.default.json` — added `products.on_sale` (visually-hidden context next to a sale price).

No changes to `templates/product.json`, `locales/en.default.schema.json`, or other sections.

## Design decisions & rationale

- **Snippet, not a block** — same rationale as the gallery: this is derived entirely from the live `product` object, nothing for a merchant to add/remove/reorder.
- **Option names come from the option itself, not new locale keys.** The original plan sketched `products.color`/`products.size` locale keys, but Shopify option names (`option.name`) are merchant-defined and already localized by the merchant — hardcoding "Color"/"Size" strings would fight that rather than support it. Only true UI-only strings (`products.on_sale`) got locale keys.
- **Breadcrumb uses `collection` (current collection context) falling back to `product.collections.first`,** paired with `product.type` as the non-link current segment — reversed from the wireframe's literal "Tops / Blouses" copy (which isn't derivable data), but it's the standard, real-data equivalent: a link to an actual collection URL, plus the type as page context.
- **No hex/swatch-color mapping.** Swatch buttons render `background-color: {{ value }}` directly — this renders correctly for values that happen to be valid CSS colors/keywords, and gracefully falls back to a neutral gray (`--color-surface-container-high`) otherwise. Real color swatches need a merchant-configured mapping (e.g. a metafield or a `swatch` resource), which doesn't exist in this theme yet; flagging for a future step rather than inventing one now.
- **"Size Guide" trigger from the wireframe was intentionally omitted.** It implies a modal/drawer that isn't scoped in any of the 6 planned steps — a dead button felt worse than leaving it out; can be added alongside whichever step introduces that content.
- **Variant JSON is pre-formatted in Liquid**, not raw cents. `{{ variant.price | money | json }}` renders through the shop's actual money settings once, in Liquid, so the JS never needs to reimplement `Shopify.formatMoney`.
- **Disabled-not-hidden**, matching `DESIGN.md`'s full-grid spirit and standard PDP accessibility practice.

## Responsive behavior

- Single column (`.product { grid-template-columns: 1fr }`) below 1024px; the gallery stacks above the info panel.
- `≥1024px`: `.product` switches to a `2fr 1fr` grid (gallery, then info), matching the wireframe's roughly 8/4-column split.
- Swatches/chips wrap via `flex-wrap: wrap` at all sizes — no separate mobile variant needed for the picker.

## Design tokens introduced

None — reuses existing color/spacing/motion tokens from `snippets/css-variables.liquid` plus the existing `.text-label-caps`/`.text-headline-md`/`.text-body-lg` utilities.

## Bug found & fixed during review

The picker's "selected value" label (e.g. "Alabaster" next to "Color") lives in a sibling element outside the `[data-variant-option]` container the JS scopes its query to. First pass silently failed to update it on click. Fixed by scoping the lookup to `.closest('.variant-picker__option')` instead. Verified live: swatch active state, selected-value label, hidden `id` input, and the URL's `?variant=` all update correctly on click.

## How to review/preview

1. `shopify theme dev`, open `/products/the-complete-snowboard` (5 color variants) — click through swatches, confirm the active ring, the "Color: X" label, and the URL's `?variant=` all update with no reload.
2. Open `/products/the-compare-at-price-snowboard` — confirm the sale price renders with a strikethrough compare-at price.
3. Open `/products/the-out-of-stock-snowboard` — confirm its single option button renders `disabled`.
4. Resize to ~390px; confirm swatches/chips wrap with no horizontal overflow (verified via viewport resize; recommend a real-device check too, consistent with Step 1).
5. `shopify theme check` — 3 pre-existing baseline warnings only, no new errors.

## Checklist

- [x] PROGRESS.md updated
- [ ] Committed (awaiting review)
