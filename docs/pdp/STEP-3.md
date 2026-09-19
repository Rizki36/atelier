# Step 3 — Cart Drawer Foundation

## What was built (planned)

- Net-new cart infrastructure: the theme currently has **no cart JS at all** — the header cart icon is a plain `<a href="{{ routes.cart_url }}">` and `sections/cart.liquid` is the stock full-page table. This step adds an AJAX cart drawer so the PDP's Add to Bag (Step 4) can confirm instantly without a page reload.
- New `snippets/cart-drawer.liquid`, rendered once from `layout/theme.liquid` (so it's available on every page): a `<dialog>`-based slide-out panel, following the exact `<dialog>` open/close pattern already used for `sections/header.liquid`'s mobile nav. Server-rendered from the live `cart` object on first paint (so it works even before any JS runs), listing line items (image, title, variant, quantity, line price) with a subtotal and checkout link.
- The drawer's own `{% javascript %}` listens for a `cart:updated` custom event (`document.addEventListener('cart:updated', ...)`, `detail` carrying the fresh `/cart.js` response), re-renders its line items from that payload, and opens itself (`dialog.showModal()`). It also updates the header's cart-count badge on the same event, so any future add-to-cart trigger (PDP, and later collection quick-add) only needs to `fetch` and dispatch — it doesn't need to know the drawer exists.
- `sections/header.liquid`'s cart icon changes from a link to a `<button>` that calls `showModal()` on the drawer directly (no event needed for the "open on click" case — only cross-component updates go through the event).

## Files changed

New:
- `snippets/cart-drawer.liquid` — LiquidDoc header (`@param {cart} cart`), own `{% stylesheet %}` + `{% javascript %}`.
- `docs/pdp/STEP-3.md` — this file.

Modified:
- `layout/theme.liquid` — render `{% render 'cart-drawer', cart: cart %}` once, near the closing `</body>`.
- `sections/header.liquid` — cart icon `<a>` → `<button type="button" aria-controls="cart-drawer">`, wired to open the drawer.
- `locales/en.default.json` — add `general.accessibility.open_cart`, `general.accessibility.close_cart`, `cart.subtotal`, `cart.empty`, `cart.view_cart`, `cart.checkout` (some may already exist under the stock `cart.*` keys — reuse before adding).

No changes to `templates/*.json`, `sections/cart.liquid` (the full `/cart` page is untouched — the drawer is a lightweight preview, not a replacement), or `config/settings_schema.json`.

## Design decisions & rationale

- **`<dialog>`, not a hand-rolled overlay.** Matches the header's existing mobile-nav pattern exactly (native focus-trapping, `Esc`-to-close, backdrop), so this introduces zero new modal/overlay conventions to the theme.
- **Custom-event bus (`cart:updated`) instead of direct function calls between the header, drawer, and future PDP script.** Keeps the three components decoupled — the PDP's add-to-cart script (Step 4) doesn't need a reference to the drawer at all, it just dispatches an event on `document`. This is the same "vanilla, no build step, no shared module system" constraint the rest of the theme already operates under.
- **Server-rendered initial state + client-side refresh on update**, rather than always fetching `/cart.js` on page load, avoids an extra network request on every page view — the drawer only needs live data once something changes the cart.
- **Independently testable before Step 4.** Opening the drawer via the header icon and seeing the current cart contents (even with zero PDP wiring) is a complete, reviewable unit of work — Step 4 only has to add the "fetch + dispatch" call from the PDP form.
- **No new CSS custom properties** — panel styling reuses `var(--color-surface-container-lowest)`, `var(--color-outline-variant)`, `var(--spacing-gutter)`, `var(--motion-fade)`.

## Responsive behavior

- Full-height slide-in panel from the right at all viewport sizes (a `<dialog>` sized via CSS to `width: min(420px, 100vw)`), consistent with common cart-drawer conventions and simplest to implement without a separate mobile layout.
- Verify at ~390px (drawer covers full width) and ~1440px (drawer is a fixed-width panel, backdrop dims the rest of the page).

## Design tokens introduced

None — reuses tokens listed above.

## How to review/preview

1. `shopify theme dev`, click the header cart icon on any page (not just the PDP).
2. Confirm the drawer slides in showing current cart contents (or an empty-cart message), the backdrop dims the page, `Esc` and a close button both dismiss it.
3. Add an item to the cart via `/cart/add` (e.g. through the stock `/cart` page or curl) in another tab, then reopen the drawer and confirm it reflects the update (this validates the event/fetch plumbing independent of the PDP).
4. `shopify theme check` — expect only the existing baseline warnings, no new errors.

## Checklist

- [ ] PROGRESS.md updated
- [ ] Committed
