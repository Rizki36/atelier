# Step 2 — Product Info Panel & Variant Picker

## What was built (planned)

- Build the right-hand info panel from the wireframe: breadcrumb (product type → collection), `<h1>` title, price (regular + compare-at via the `money` filter), color swatches, and size chips.
- New `snippets/product-variant-picker.liquid`, driven by `product.options_with_values` and `product.variants` — renders each color-type option as a circular swatch button and every other option (size) as a `label-caps` chip button, per `DESIGN.md`'s "Chips/Tags" spec ("Minimalist rectangles with 1px Soft Gray borders").
- The picker's own `{% javascript %}` tracks selected option values, resolves the matching variant client-side, and on change: updates the displayed price, swaps the gallery's main image to the variant's featured media (dispatches a small custom event the Step 1 gallery snippet listens for), updates a hidden `id` input consumed by the add-to-cart form (wired in Step 4), and reflects the choice in the URL via `history.replaceState` (no reload) — the same "progressive, no-framework" approach as the theme's existing inline scripts.
- Unavailable variant combinations are disabled (`aria-disabled="true"`, non-interactive) rather than hidden, so merchants and shoppers can see the full option matrix.

## Files changed

New:
- `snippets/product-variant-picker.liquid` — LiquidDoc header (`@param {product} product`), own `{% stylesheet %}` + `{% javascript %}`.
- `docs/pdp/STEP-2.md` — this file.

Modified:
- `sections/product.liquid` — add breadcrumb markup, `<h1>{{ product.title }}</h1>`, price markup, and `{% render 'product-variant-picker', product: product %}`.
- `locales/en.default.json` — add under a new `products` object: `products.price.sale` (reuse where relevant), `products.color`, `products.size`, `products.size_guide`, `products.sold_out`, `products.breadcrumb_separator` (or use a plain `/` per the wireframe, no locale key needed for a symbol).
- `locales/en.default.schema.json` — no new schema settings needed yet (no merchant-configurable options on the picker itself); revisit if a "show compare-at price" toggle is wanted later.

No changes to `templates/product.json` or other sections.

## Design decisions & rationale

- **Snippet, not a block.** Like the gallery, this content is entirely derived from the live `product` object — there's nothing here for a merchant to add/remove/reorder in the theme editor, so it stays a `snippets/` component per `AGENTS.md`.
- **Client-side variant resolution, no server round-trip.** Matches Shopify's standard PDP pattern and keeps the interaction instant; the theme has no existing variant-picker precedent, so this introduces the pattern fresh, documented here for reuse if a quick-view/quick-add feature is ever added to `trending-now.liquid`.
- **Disabled-not-hidden unavailable variants**, matching `DESIGN.md`'s spirit of showing the full structured grid rather than collapsing it, and standard PDP accessibility practice (`aria-disabled` + `aria-label` explaining why, e.g. "Sold out").
- **No new CSS custom properties** — swatch/chip styling reuses `var(--color-primary)`, `var(--color-outline)`, `var(--radius)` (0px), and the existing `.text-label-caps` utility class.

## Responsive behavior

- Single-column info panel on mobile (`<768px`), matching the section's overall stacked layout; swatches/chips wrap via `flex-wrap: wrap` at all sizes, so no separate mobile variant exists for the picker itself.
- Desktop (`≥1024px`): info panel occupies the right column (cols 9–12) alongside the Step 1 gallery.

## Design tokens introduced

None — reuses tokens listed above plus `var(--spacing-unit)` for swatch/chip gaps.

## How to review/preview

1. `shopify theme dev`, open a product with multiple variants (color + size).
2. Click a color swatch and a size chip; confirm the price and main image update instantly with no page reload, and the URL reflects the selected variant.
3. Select a combination that has no matching variant (out of stock); confirm it renders disabled with a "Sold out" label rather than disappearing.
4. Resize to ~390px; confirm swatches/chips wrap cleanly with no horizontal overflow.
5. `shopify theme check` — expect only the existing baseline warnings, no new errors.

## Checklist

- [ ] PROGRESS.md updated
- [ ] Committed
