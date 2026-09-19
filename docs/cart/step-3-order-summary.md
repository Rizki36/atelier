# Step 3: Order Summary & Checkout Action

**Goal:** Build the right-hand column (sidebar) containing the order summary totals and the checkout button.

## Tasks

1.  **Summary Container:** In the right column (`lg:col-span-4`) of `sections/cart.liquid`, create the sticky container for the order summary.
2.  **Totals:**
    *   **Subtotal:** Display the cart's subtotal using `cart.items_subtotal_price | money`.
    *   **Shipping & Taxes:** Add text placeholders "Calculated at checkout" as shown in the wireframe.
    *   **Total:** Display the final total using `cart.total_price | money`.
3.  **Checkout Button:**
    *   Add the "Proceed to Checkout" `<button>`.
    *   Ensure it has `type="submit"` and `name="checkout"` so it routes to the Shopify checkout flow when the form is submitted.
4.  **Shipping Notice:**
    *   Add the "Complimentary Shipping" informational block with the truck icon below the checkout button, matching the wireframe design.

## Reference

*   Wireframe: `docs/cart/wireframe.html` (Lines 88-124 for order summary structure)
*   Shopify Cart Object: https://shopify.dev/docs/api/liquid/objects/cart

