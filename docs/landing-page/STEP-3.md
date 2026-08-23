# Step 3 — Curated Categories

## What was built

- A new `sections/curated-categories.liquid`: a constrained-width (not full-bleed) section with a header row (heading + "Explore All" link with an arrow icon that nudges on hover) above a hairline divider, followed by a responsive grid of category cards.
- A new dedicated block type, `blocks/category-card.liquid`: each card is a single link containing an image, a dark gradient scrim, and bottom-pinned title + "Shop Now" caption overlaid on the image. Merchants can add, remove, and reorder these blocks freely (up to 6) via the theme editor — this is a "curated" grid, not a fixed N-slot layout.
- A new `assets/icon-arrow-right.svg`, following the existing icon convention.

## Files changed

New:
- `sections/curated-categories.liquid`
- `blocks/category-card.liquid`
- `assets/icon-arrow-right.svg`
- `docs/landing-page/STEP-3.md` — this file.

Modified:
- `templates/index.json` — added a `categories` entry (type `curated-categories`) after `hero`, with 3 default category-card blocks (Outerwear/Essentials/Accessories) matching the wireframe, so the homepage isn't empty on first load. The existing `hero` entry's live-edited settings (image/eyebrow/heading/body/button, set via the theme editor) were left untouched.
- `locales/en.default.schema.json` — added `general.curated_categories`, `general.category_card`, and `labels.explore_all_label`/`explore_all_link`/`category_title`/`category_link`. Reused the existing `labels.heading` and `labels.image` from Step 2 rather than duplicating them.

## Design decisions & rationale

- **Category cards kept the wireframe's scale-zoom hover (1.05 over 700ms), not an opacity fade.** DESIGN.md's literal motion rule is "opacity fades... rather than lifting elements off the page," but a same-plane scale-zoom adds no shadow or elevation change — per explicit user decision, the wireframe's treatment was kept as-is.
- **Category cards kept the wireframe's 3:4 aspect ratio**, even though DESIGN.md's Image Placeholders rule only formally sanctions 4:5 or 2:3. Per explicit user decision, the wireframe (the literal structural source for this section) was followed.
- **Dedicated `category-card` block type, not fixed section settings.** Category cards are a repeated, homogeneous, structured unit (image + name + link) — exactly what AGENTS.md's blocks system is for. Unlike `custom-section.liquid`'s generic `{"type": "@theme"}` (accepts any block), this needed a typed block with its own fixed shape so merchants get purpose-built fields (Image / Category name / Link) rather than a freeform block or a hard-capped fixed-slot settings array.
- **Plain `url` setting for the card link, not a native `type: "collection"` picker.** Matches Hero's `button_link` precedent; the theme has no existing collection-picker pattern, and the wireframe's cards are placeholder `href="#"` links, not bound to real collections.
- **Section is not `full-width`.** Unlike Hero, the wireframe keeps this section inside the normal constrained container, so it renders as an ordinary child of `.shopify-section` and is centered automatically via the existing grid rule — no need to reimplement `--content-grid` internally.
- **Bug found and fixed during verification**: Shopify wraps each rendered block in its own `.shopify-block` wrapper `<div>`, so a `.category-card:nth-child(2)` selector inside the block's own stylesheet never matched (each card is always the only child of its wrapper). The asymmetrical offset for the second card was moved to the section's stylesheet instead, targeting `.categories__grid > :nth-child(2) .category-card` — the section, which owns the grid layout, is the correct place for this rule rather than the block, which only owns its own internal styling.

## Responsive behavior

- **<768px**: single-column grid, cards keep their 3:4 ratio, header row wraps if needed.
- **≥768px**: 3-column grid; the second card (`Essentials` by default) is offset down 48px for the asymmetrical layout DESIGN.md calls for ("use asymmetrical layouts for imagery while keeping typography strictly aligned").

Verified visually at 1440px (desktop, 3 columns + offset confirmed) and 500px (mobile, single column confirmed) via `shopify theme dev`.

## Design tokens introduced

None — this step consumes `.text-headline-md`/`.text-headline-sm`/`.text-label-caps`, `--spacing-gutter`/`--spacing-section-gap`, `--color-outline-variant`/`--color-surface-container`/`--color-on-tertiary`/`--color-primary`/`--color-on-surface-variant`, and `--motion-fade` entirely as built in Steps 1–2.

## How to review/preview

1. `shopify theme dev` and open the homepage.
2. Below the Hero, confirm "Curated Categories" heading + "Explore All →" link above a hairline divider, then 3 cards (Outerwear/Essentials/Accessories) in a grid.
3. Desktop (≥768px): confirm 3 columns and that the middle card sits lower than the other two.
4. Hover a card: the image should zoom slightly (1.05, 700ms) — text stays readable via the gradient scrim even with no image set.
5. Hover "Explore All": text shifts toward primary color, arrow nudges right.
6. Resize to mobile (<768px): grid collapses to 1 column.
7. In the theme editor, select Curated Categories, expand it, and confirm its 3 category-card blocks are individually selectable/editable (Image, Category name, Link) and can be added/removed/reordered.
8. `shopify theme check` — should report only the same 3 expected `RemoteAsset` warnings from Step 1, no errors.

## Checklist

- [x] PROGRESS.md updated
- [x] Committed
