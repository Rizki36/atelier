# Step 4 — Add to Bag & Product Details Accordion

## What was built

- Wire the PDP's `{% form 'product', product %}` Add to Bag button to the Step 3 cart drawer: on submit, intercept via `fetch('/cart/add.js', { method: 'POST', body: new FormData(form) })`; on success, `fetch('/cart.js')` and `document.dispatchEvent(new CustomEvent('cart:updated', { detail: cart }))` so the drawer refreshes and opens itself — no page reload. If JS fails to load or errors, the form falls back to a native submit (progressive enhancement, no dead-end).
- Add the shipping-note copy below the button ("Complimentary shipping on orders over $X"), with the threshold sourced from a new section setting (not hardcoded), matching the wireframe.
- Build the Product Details / Size & Fit / Care Instructions accordion as a new repeatable theme block, `blocks/product-detail.liquid`: a `text` setting for the summary title and a `richtext` setting for the body, rendered as native `<details>/<summary>` — the first use of `<details>` anywhere in this theme. Three default blocks (Product Details, Size & Fit, Care Instructions) are seeded via the section's block defaults/presets, matching how `footer-group.json` pre-seeds `footer-link-column` blocks.
- `sections/product.liquid` renders the accordion via `{% content_for 'blocks' %}` inside a wrapper div, following the exact `footer.liquid` block-rendering pattern.

## Files changed

New:
- `blocks/product-detail.liquid` — `{% doc %}` header, `block.shopify_attributes` on the root `<details>`, own `{% stylesheet %}` (chevron rotate-on-open animation, matching `footer.liquid`'s `group-open:rotate-180`-equivalent vanilla CSS).
- `docs/pdp/STEP-4.md` — this file.

Modified:
- `sections/product.liquid` — add the fetch-based add-to-cart `{% javascript %}`, shipping-note markup, accordion wrapper + `{% content_for 'blocks' %}`; add `"blocks": [{ "type": "product-detail" }]` and a `free_shipping_threshold` (number/range) setting to `{% schema %}`.
- `templates/product.json` — seed the `main` section's block list with three `product-detail` blocks (Product Details / Size & Fit / Care Instructions), same seeding approach as `footer-group.json`.
- `locales/en.default.json` — add `products.add_to_bag`, `products.shipping_note` (with a `{{ threshold }}` variable per `AGENTS.md`'s interpolation guidance), `general.accessibility.details_expand`/`collapse` if needed beyond the native `<summary>` semantics.
- `locales/en.default.schema.json` — add `general.product_detail` (block name), reuse `labels.heading`/`labels.text` if suitable or add `labels.free_shipping_threshold`.

No changes to `snippets/cart-drawer.liquid`'s internals beyond what Step 3 already defined (it only ever consumes `cart:updated`).

New (not originally planned):
- `assets/icon-chevron-down.svg` — accordion chevron, matching `icon-close.svg`'s `currentColor`/`var(--icon-stroke-width)` convention.

Deviation from plan: the `free_shipping_threshold` setting and `product-detail` blocks live directly on `sections/product.liquid`'s schema (no separate settings group was needed — one `number` setting was enough). `templates/product.json` seeds the three default blocks via a `blocks`/`block_order` map, mirroring `footer-group.json`'s pattern, rather than relying solely on the block's `presets`.

## Design decisions & rationale

- **Fetch with native-submit fallback**, not a hard JS dependency, so Add to Bag still works if JavaScript fails — consistent with the rest of the theme's forms (e.g. the footer newsletter form degrades to a full-page Shopify-hosted response).
- **Accordion items are theme blocks, not hardcoded markup.** The wireframe's three sections (Details/Fit/Care) are exactly the kind of merchant-editable, reorderable, add/remove-able content `AGENTS.md` calls out blocks for — mirrors `blocks/footer-link-column.liquid`'s "block for repeatable content" precedent rather than three one-off `<details>` elements baked into the section.
- **Native `<details>/<summary>`**, not a custom JS accordion, for built-in accessibility (keyboard toggling, screen-reader state) and zero JS cost — the wireframe's `group-open:rotate-180` chevron animation is achievable with pure CSS (`details[open] summary .icon { transform: rotate(180deg); }`).
- **Shipping threshold as a section setting**, not a hardcoded locale string, so merchants can change the dollar amount without a code deploy — the copy itself stays translatable via a `{{ threshold }}` interpolation variable.
- **No new CSS custom properties** — reuses `var(--color-outline-variant)` for the hairline dividers between accordion items (matching `footer.liquid`'s own hairline pattern) and `var(--motion-fade)` for the chevron rotation.

## Responsive behavior

- Accordion and Add to Bag button are full-width at all viewport sizes within the info panel column — no distinct mobile layout needed beyond the panel's existing single-column stacking (from Step 2).
- Verify the fetch/drawer-open flow works identically at ~390px and ~1440px.

## Design tokens introduced

None — reuses tokens listed above.

## How to review/preview

1. `shopify theme dev`, open a product page, select a valid variant (per Step 2), click Add to Bag.
2. Confirm the cart drawer (Step 3) opens automatically showing the newly added line item, with no full-page reload.
3. Disable JavaScript (or simulate a fetch failure) and confirm the form still submits natively and lands on `/cart` (or wherever Shopify redirects) — the fallback path.
4. In the theme editor, confirm the three accordion blocks (Product Details / Size & Fit / Care Instructions) are editable, reorderable, and a merchant can add a fourth or remove one; confirm the shipping-threshold setting updates the on-page copy.
5. Keyboard-only pass: `Tab` to a `<summary>`, press `Enter`/`Space` to expand/collapse.
6. `shopify theme check` — expect only the existing baseline warnings, no new errors.

## Checklist

- [x] PROGRESS.md updated
- [x] Committed
