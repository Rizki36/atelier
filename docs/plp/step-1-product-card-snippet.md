# Step 1 — `snippets/product-card.liquid`

## Goal

Create a reusable product card snippet matching the wireframe's 3:4-ratio image card design with hover zoom, product title, color variant label, and price.

## Output File

**[`snippets/product-card.liquid`](file:///Users/shiro/Documents/GitHub/atelier/snippets/product-card.liquid)** ✅ Done

## Parameters

| Param | Type | Required | Description |
|---|---|---|---|
| `product` | `product` | ✅ | The Shopify product object |
| `lazy` | `boolean` | ❌ | Whether to lazy-load the image (default: `true`) |

## Usage

```liquid
{% render 'product-card', product: product %}
{% render 'product-card', product: product, lazy: false %}
```

## Key Details

- **Image**: 3:4 aspect ratio (`aspect-ratio: 3 / 4`), `object-fit: cover`, CSS `scale(1.05)` hover transition
- **Color label**: reads the `Color` variant option via `product.options_with_values | where: 'name', 'Color' | first`
- **Price**: `money_without_trailing_zeros` filter
- **Accessibility**: `<h2>` title, `aria-label` on the `<a>` link, placeholder SVG for missing images
- **Performance**: First 3 cards use `loading="eager"` (above the fold), rest use `loading="lazy"`

## Testing Checklist

- [ ] Card renders on `/collections/*` page
- [ ] Hover zoom is smooth
- [ ] Color label shows correct variant option
- [ ] Price formatted correctly
- [ ] Placeholder SVG shows when no image

