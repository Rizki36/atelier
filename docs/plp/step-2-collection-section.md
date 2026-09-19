# Step 2 — `sections/collection.liquid`

## Goal

Rebuild the bare-bones `sections/collection.liquid` stub into a full PLP layout matching the wireframe: sticky sidebar filters, sort dropdown, responsive product grid, and Load More pagination.

## Output File

**[`sections/collection.liquid`](file:///Users/shiro/Documents/GitHub/atelier/sections/collection.liquid)** ✅ Done

## Layout Structure

```
<section.collection-section>
  <div.container>
    <!-- Header: title + sort + mobile filter toggle -->
    <div.collection-section__header>

    <div.collection-section__body>
      <!-- Sidebar (desktop sticky, mobile drawer) -->
      <aside.collection-section__sidebar>
        <form.filter-form>
          <!-- Dynamic filters from collection.filters -->
          <!-- list → checkboxes -->
          <!-- presentation=swatch → color swatches -->
          <!-- price_range → min/max inputs -->

      <!-- Product grid -->
      <div.collection-section__products>
        <ul.product-grid>
          {% render 'product-card' %}
        <!-- Load More link -->
```

## Schema Settings

| ID | Type | Default | Description |
|---|---|---|---|
| `products_per_page` | range (6–48, step 6) | 12 | Products shown per page |
| `show_filters` | checkbox | true | Toggle sidebar |
| `enable_color_swatches` | checkbox | true | Render swatches vs. checkboxes for color |
| `show_sort` | checkbox | true | Toggle sort-by dropdown |
| `show_count` | checkbox | false | Show "Showing X of Y" text |

## Filter System

Filters use **Shopify Storefront Filtering** (`collection.filters` object). The form posts using native `<input type="checkbox">` auto-submit via JS — no full page reload library needed. Checking/unchecking any filter checkbox auto-submits the form.

- **List/boolean filters**: checkbox list or color swatches (when `filter.presentation == 'swatch'`)
- **Price range**: min/max number inputs with Apply button
- **Active filter count**: shown on the mobile filter button badge
- **Clear all**: link to `collection.url` (strips all filter params)

## JavaScript

- **Sort**: `change` event on `<select>` → sets `?sort_by=` URL param and navigates
- **Mobile filter toggle**: `click` on mobile btn → toggles `is-open` class on sidebar + `aria-expanded`
- **Filter auto-submit**: `change` on each checkbox → `form.submit()`

## Testing Checklist

- [ ] Grid renders 3 cols desktop / 2 cols tablet / 1 col mobile
- [ ] Sort dropdown changes URL param and reloads with sorted products
- [ ] Filter checkboxes auto-submit and update URL
- [ ] Price range filter works with Apply button
- [ ] Mobile filter button toggles sidebar visibility
- [ ] Load More button links to `paginate.next.url`
- [ ] Empty state message shows when no products match filters
- [ ] Schema settings are visible in theme editor

