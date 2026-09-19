# Step 3 — `locales/en.default.json`

## Goal

Add a `"collection"` namespace to the English locale file to keep all user-facing PLP strings translatable.

## Output File

**[`locales/en.default.json`](file:///Users/shiro/Documents/GitHub/atelier/locales/en.default.json)** ✅ Done

## Added Keys

```json
"collection": {
  "sort_by": "Sort by",
  "filters": "Filters",
  "active_filters": "active filters",
  "clear_filters": "Clear all filters",
  "apply": "Apply",
  "load_more": "Load More",
  "no_products": "No products found.",
  "showing_x_of_y": "Showing {{ x }} of {{ y }} products",
  "price_min": "Min",
  "price_max": "Max",
  "products_per_page": "Products per page",
  "show_filters": "Show filters",
  "enable_color_swatches": "Enable color swatches",
  "show_sort": "Show sort by",
  "show_count": "Show product count"
}
```

## Key Map

| Locale Key | Used In | Notes |
|---|---|---|
| `collection.sort_by` | Sort `<label>` (sr-only) | Screen readers |
| `collection.filters` | Mobile filter button + sidebar `aria-label` | |
| `collection.active_filters` | Filter count badge `aria-label` | |
| `collection.clear_filters` | "Clear all" link | Only shown when filters are active |
| `collection.apply` | Price range apply button | |
| `collection.load_more` | Load More link text | |
| `collection.no_products` | Empty state paragraph | |
| `collection.showing_x_of_y` | Product count text | Uses `x` and `y` variables |
| `collection.price_min/max` | Price range labels | |
| `collection.products_per_page` — `show_count` | Schema labels in theme editor | |

## Testing Checklist

- [ ] All `{{ 'collection.*' | t }}` keys resolve without error in Shopify theme preview
- [ ] Schema setting labels show correct English text in theme editor
- [ ] Add matching keys to any other locale files (e.g. `fr.json`) if the store is multilingual

