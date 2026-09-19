# Step 2: Cart Line Items & Quantity Controls

**Goal:** Render the items currently in the user's cart, including their details, pricing, and controls to modify quantities or remove items.

## Tasks

1.  **Iterate Over Items:** Use a `{% for item in cart.items %}` loop within the left column of the layout created in Step 1.
2.  **Render Item Details:**
    *   **Image:** Display `item.image` using the `image_url` filter and `image_tag`. Follow the wireframe's aspect ratio and styling.
    *   **Title:** Output `item.product.title`.
    *   **Variant Options:** Loop through `item.options_with_values` to display chosen variants (e.g., Color, Size).
    *   **Price:** Output `item.final_line_price` using the `money` filter.
3.  **Quantity Selector:**
    *   Build the custom -/+ quantity selector as shown in the wireframe.
    *   Ensure the input has `name="updates[]"` and `value="{{ item.quantity }}"` (or similar depending on JS approach) so the form works.
4.  **Remove Action:**
    *   Add a "Remove" button/link pointing to `item.url_to_remove` (which is typically `/cart/change?line={{ forloop.index }}&quantity=0`).
5.  **Save for Later:**
    *   Implement the "Save for Later" button from the wireframe as a UI placeholder (Shopify doesn't natively support this without an app, but we need the UI).
6.  **Separators:**
    *   Add border dividers between items as indicated in the wireframe.

## Reference

*   Wireframe: `docs/cart/wireframe.html` (Lines 11-85 for item structure)
*   Shopify Line Item Object: https://shopify.dev/docs/api/liquid/objects/line_item

