# PLP Page — Build Progress

Reference wireframe: [`docs/plp/wireframe.html`](wireframe.html)
Related template: [`templates/collection.json`](../../templates/collection.json)

---

## Steps

| # | Step | Status | File(s) |
|---|---|---|---|
| 1 | Product Card Snippet | ✅ Done | [`snippets/product-card.liquid`](../../snippets/product-card.liquid) |
| 2 | Collection Section (PLP layout) | ✅ Done | [`sections/collection.liquid`](../../sections/collection.liquid) |
| 3 | Locale Keys | ✅ Done | [`locales/en.default.json`](../../locales/en.default.json) |

---

## Step Details

### ✅ Step 1 — Product Card Snippet
[`docs/plp/step-1-product-card-snippet.md`](step-1-product-card-snippet.md)

- Reusable `snippets/product-card.liquid` with 3:4 image, hover zoom, title, color label, price
- Accepts `product` (required) and `lazy` (optional) params
- First 3 cards eager-load for LCP, rest lazy-load

### ✅ Step 2 — Collection Section
[`docs/plp/step-2-collection-section.md`](step-2-collection-section.md)

- Full PLP layout: header, sidebar filters, product grid, Load More
- Sidebar uses `collection.filters` (Shopify Storefront Filtering)
- Supports `list` / `boolean` (checkboxes + color swatches), `price_range` filters
- Sort-by dropdown with `collection.sort_options`
- `{% paginate collection.products by products_per_page %}`
- Load More = link to `paginate.next.url`
- Mobile filter toggle via JS + `aria-expanded`
- Schema: `products_per_page`, `show_filters`, `enable_color_swatches`, `show_sort`, `show_count`

### ✅ Step 3 — Locale Keys
[`docs/plp/step-3-locales.md`](step-3-locales.md)

- Added `"collection"` namespace to `locales/en.default.json`
- Keys: `sort_by`, `filters`, `active_filters`, `clear_filters`, `apply`, `load_more`, `no_products`, `showing_x_of_y`, `price_min`, `price_max`, and schema labels

---

## Manual Verification Checklist

- [x] `templates/collection.json` references `"type": "collection"` section → already configured
- [x] Preview collection page on Shopify theme editor
- [x] Sidebar renders on desktop (≥1024px), toggles on mobile
- [x] Sort dropdown changes `?sort_by=` URL param
- [x] Filter checkboxes auto-submit and filter products
- [x] Color swatches appear when filter `presentation` = `swatch` (requires merchant to configure filters in Shopify Admin)
- [x] Load More link navigates to page 2 correctly
- [x] Empty state message shows when no products match filters
- [x] Schema settings visible and working in theme editor

---

## Notes

> Filters only appear when the merchant has configured them in **Shopify Admin → Online Store → Navigation → Filters**. The section renders gracefully with no sidebar when no filters are set.

> Color swatches fall back to checkboxes if `enable_color_swatches` is disabled in theme editor or if the filter `presentation` is not `swatch`.

