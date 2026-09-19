# Cart Page — Build Progress

Reference wireframe: [`docs/cart/wireframe.html`](wireframe.html)
Related template: [`templates/cart.json`](../../templates/cart.json)

---

## Steps

| # | Step | Status | File(s) |
|---|---|---|---|
| 1 | Main Cart Section Layout & Empty State | ✅ Done | [`sections/cart.liquid`](../../sections/cart.liquid) |
| 2 | Cart Line Items & Quantity Controls | ✅ Done | [`sections/cart.liquid`](../../sections/cart.liquid) |
| 3 | Order Summary & Checkout Action | ✅ Done | [`sections/cart.liquid`](../../sections/cart.liquid) |
| 4 | Locales, Schema & JavaScript Interactivity | ✅ Done | [`sections/cart.liquid`](../../sections/cart.liquid), [`locales/en.default.json`](../../locales/en.default.json) |

---

## Step Details

### ✅ Step 1 — Main Cart Section Layout & Empty State
[`docs/cart/step-1-layout-empty-state.md`](step-1-layout-empty-state.md)

- Setup the main container with grid layout (`grid-cols-1 lg:grid-cols-12`).
- Include the main title "Your Cart ({{ cart.item_count }} Items)" and the "Clear All" button.
- Handle the empty cart state (when `cart.item_count == 0`), showing a message and a link to continue shopping.
- Use `{% form 'cart', cart %}` to wrap the form elements.

### ⏳ Step 2 — Cart Line Items & Quantity Controls
[`docs/cart/step-2-line-items.md`](step-2-line-items.md)

- Loop through `cart.items`.
- Display item image, product title, selected variant options, and price.
- Build the quantity adjuster (- / +) and link it to the item's quantity.
- Implement the "Remove" button using a link to `/cart/change` or JS API.
- Add UI placeholder for "Save for Later".

### ✅ Step 3 — Order Summary & Checkout Action
[`docs/cart/step-3-order-summary.md`](step-3-order-summary.md)

- Build the sticky order summary sidebar.
- Display cart subtotal, shipping notice, and taxes message.
- Display the total price (`cart.total_price`).
- Add the `Proceed to Checkout` button (`<button type="submit" name="checkout">`).
- Add the "Complimentary Shipping" info block.

### ✅ Step 4 — Locales, Schema & JavaScript Interactivity
[`docs/cart/step-4-locales-schema.md`](step-4-locales-schema.md)

- Add schema settings to `sections/cart.liquid` (e.g., shipping threshold text, enable/disable specific elements).
- Add JavaScript to handle quantity updates and cart clearing without full page reloads (using Shopify Cart API).
- Add `"cart"` namespace to `locales/en.default.json` for all text strings.

---

## Manual Verification Checklist

- [ ] `templates/cart.json` references `"type": "cart"` section
- [ ] Cart shows empty state correctly when no items are present
- [ ] Cart renders line items with correct images, titles, variants, and prices
- [ ] Quantity can be increased/decreased and updates the total price
- [ ] Items can be removed from the cart
- [ ] "Clear All" empties the cart
- [ ] Checkout button navigates to Shopify checkout
- [ ] Schema settings visible and working in theme editor
- [ ] Responsive layout looks good on mobile and desktop

