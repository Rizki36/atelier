# Step 1: Main Cart Section Layout & Empty State

**Goal:** Set up the foundational structure of the cart page, including the main grid layout, the cart header, and the empty state.

## Tasks

1.  **Create Section File:** Create (or modify if it exists) `sections/cart.liquid`.
2.  **HTML Structure:**
    *   Implement the main `<main>` container and padding/margins based on the wireframe (`docs/cart/wireframe.html`).
    *   Set up the CSS Grid layout (`grid-cols-1 lg:grid-cols-12`) that will hold the items (left column, span 8) and order summary (right column, span 4).
3.  **Cart Header:**
    *   Add the `<h1>` title "Your Cart" with the item count `{{ cart.item_count }}`.
    *   Add the "Clear All" button (functionality can be wired up later or point to `/cart/clear`).
4.  **Empty State:**
    *   Add an `{% if cart.item_count == 0 %}` conditional.
    *   If empty, display a message like "Your cart is currently empty" and a button to "Continue Shopping" linking to `routes.all_products_collection_url`.
5.  **Form Wrapper:**
    *   Wrap the non-empty state content in a `{% form 'cart', cart %}` tag to enable standard Shopify cart submission.

## Reference

*   Wireframe: `docs/cart/wireframe.html` (Lines 4-9 for header structure)
*   Shopify Cart Object: https://shopify.dev/docs/api/liquid/objects/cart

