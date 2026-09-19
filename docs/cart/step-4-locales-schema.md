# Step 4: Locales, Schema & JavaScript Interactivity

**Goal:** Finalize the cart section by extracting hardcoded text to locale files, adding merchant configuration via schema, and enhancing UX with JavaScript.

## Tasks

1.  **Locales Extraction:**
    *   Add a `"cart"` object to `locales/en.default.json`.
    *   Extract strings like "Your Cart", "Clear All", "Size", "Remove", "Save for Later", "Order Summary", "Subtotal", "Shipping", "Taxes", "Calculated at checkout", "Total", "Proceed to Checkout", and empty state messages.
    *   Replace hardcoded text in `sections/cart.liquid` with `{{ 'cart.general.title' | t }}` syntax.
2.  **Section Schema:**
    *   Add `{% schema %}` to `sections/cart.liquid`.
    *   Define settings for merchant customization (e.g., enabling/disabling the shipping notice, setting the free shipping threshold text).
3.  **JavaScript Enhancement (Optional but recommended):**
    *   Add JS (within an inline `<script>` or external asset) to handle quantity changes via the Shopify AJAX Cart API (`/cart/change.js`).
    *   Update item totals, subtotal, and total price in the DOM dynamically without a full page reload when quantities change or items are removed.
    *   Alternatively, rely on standard form submission if AJAX is out of scope.

## Reference

*   Shopify Localization: https://shopify.dev/docs/themes/architecture/locales
*   Shopify Section Schema: https://shopify.dev/docs/themes/architecture/sections/section-schema
*   Shopify AJAX Cart API: https://shopify.dev/docs/api/ajax/reference/cart

