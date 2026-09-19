# Step 6 — Locale, Schema, Accessibility & Responsive Polish

## What was built (planned)

- A cross-cutting pass over everything added in Steps 1–5, rather than new user-facing functionality: consolidate and finalize locale keys, run an accessibility audit, do a full responsive sweep, and clear `shopify theme check`.
- **Locale consolidation**: review every `products.*`, `general.accessibility.*`, and `cart.*` key added across Steps 1–5 for duplicates or naming drift (e.g. confirm Step 4's `products.add_to_bag` and Step 3's `cart.*` keys don't overlap in meaning), and confirm `en.default.schema.json`'s `general.*`/`labels.*` additions reused existing shared labels wherever possible instead of accumulating near-duplicates (e.g. `labels.heading` vs. a hypothetical `labels.section_heading`).
- **Accessibility audit**: `aria-labelledby`/heading `id` pairing on `sections/product.liquid` and `sections/product-styling.liquid` (matching `trending-now.liquid`'s `aria-labelledby="trending-now-heading"` pattern); `role="status"` on the cart drawer's line-item list so screen readers announce updates after `cart:updated`; focus management on drawer open (focus moves to the drawer's close button or first focusable element) and close (focus returns to the triggering cart icon); confirm all icon-only buttons (zoom, thumbnail nav, drawer close) have `aria-label`s; confirm the variant picker's disabled/sold-out states are announced, not just visually indicated.
- **Responsive sweep**: revisit all five prior steps together at ~390px and ~1440px in a single pass (rather than each step's isolated check) to catch any cross-step layout interference — e.g. confirm the gallery (Step 1) + info panel (Step 2) + accordion (Step 4) don't collectively exceed expected page height/scroll behavior on mobile, and that the styling section (Step 5) doesn't visually collide with the accordion above it.
- **`shopify theme check`** run against the full PDP template end-to-end; fix anything beyond the pre-existing Google Fonts `RemoteAsset` baseline warning.
- Update `docs/pdp/PROGRESS.md`, checking off all six steps.

## Files changed

New:
- `docs/pdp/STEP-6.md` — this file.

Modified (as needed, scope determined during the audit — not predetermined):
- `locales/en.default.json`, `locales/en.default.schema.json` — any key renames/dedup found during consolidation.
- `sections/product.liquid`, `sections/product-styling.liquid`, `snippets/product-media-gallery.liquid`, `snippets/product-variant-picker.liquid`, `snippets/cart-drawer.liquid`, `blocks/product-detail.liquid` — targeted accessibility fixes only, no structural rework (any structural gap found here should be treated as a bug in the originating step, not new scope for Step 6).
- `docs/pdp/PROGRESS.md` — all six items checked off.

No new sections, blocks, or snippets are expected in this step — it is a hardening pass, not a feature step.

## Design decisions & rationale

- **Polish as its own step, not folded into Step 5.** Each of Steps 1–4 introduces genuinely new interactive surface area (gallery, variant picker, cart drawer, add-to-cart + accordion) — bundling an accessibility/responsive audit into any single one of them would either shortchange the audit or bloat that step's review scope. A dedicated final pass, reviewed once against the whole assembled page, matches how the landing page's own Step 6 (Footer) served as the last piece before the full page was considered done — except here the "last piece" is verification, not a new section, since Step 5 already completes the PDP's visual scope.
- **No predetermined file list for modifications** — this step's job is to find and fix gaps, not implement a spec, so the exact diff is intentionally left open until the audit runs.
- **`shopify theme check` as the hard gate**, consistent with every prior step's own review checklist (Steps 1–6 of the landing page all required a clean check run before being marked done).

## Responsive behavior

Full end-to-end sweep at ~390px and ~1440px of the assembled PDP (gallery → info panel → accordion → styling section), plus a quick check at the theme's other documented breakpoint (768px, per `assets/critical.css`) to confirm no awkward mid-transition states between the mobile and desktop layouts defined in Steps 1–5.

## Design tokens introduced

None expected — this step consumes, audits, and (if needed) corrects usage of tokens already introduced in Steps 1–5; it should not need new tokens.

## How to review/preview

1. `shopify theme dev`, open a product page, do a full top-to-bottom pass at ~390px and ~1440px.
2. Keyboard-only pass: `Tab` through the entire PDP (gallery thumbnails → variant picker → Add to Bag → accordion → styling CTA → cart drawer once opened) confirming a logical focus order and visible focus states throughout.
3. Screen-reader spot check (VoiceOver/NVDA) on: the variant picker's sold-out announcement, the cart drawer opening and announcing its updated contents, and the accordion's expand/collapse state.
4. `shopify theme check` — must report only the pre-existing Google Fonts `RemoteAsset` baseline, no new errors or warnings.
5. Confirm `docs/pdp/PROGRESS.md` shows all six steps checked off.

## Checklist

- [ ] PROGRESS.md updated
- [ ] Committed
