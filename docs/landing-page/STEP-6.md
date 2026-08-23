# Step 6 — Footer

## What was built

- A full rebuild of the stock `sections/footer.liquid` (previously a bare copyright line + flat link list + payment-icon toggle) into the wireframe's 4-column footer: a Newsletter signup, two merchant-editable link columns ("Information" / "Follow Us"), and a Logo + Copyright column.
- The Newsletter column uses Shopify's native customer-tag newsletter form (`{% form 'customer' %}`, `contact[tags]=newsletter`, `contact[email]`) with proper `form.posted_successfully?` / `form.errors` handling — real functionality, not a decorative button. This is new territory for the theme; no prior step needed a form.
- The link columns are **block-based**, like Steps 3 and 5's repeatable content, via a new `blocks/footer-link-column.liquid`: a heading text field + a `link_list` field pointing at a Shopify navigation menu — mirroring `header.liquid`'s own `menu` (`link_list`) setting rather than inventing a custom hardcoded-link mechanism.
- Logo + copyright column: a local `image_picker` (`logo`) setting, matching the pattern (not the value) of `header.liquid`'s own local logo setting, since there is no theme-wide shared logo setting; copyright text is dynamic (`{{ 'now' | date: '%Y' }}`, `shop.name`) via a new `general.footer.copyright_html` locale string.
- Full-bleed background + border with a constrained-width inner grid, following `.header`/`.header__inner`'s exact pattern (`full-width` class + `.footer__inner { max-width: var(--page-width); margin-inline: auto; padding-inline: var(--page-margin); }`).

## Files changed

New:
- `blocks/footer-link-column.liquid`
- `docs/landing-page/STEP-6.md` — this file.

Modified:
- `sections/footer.liquid` — full rebuild (see above).
- `sections/footer-group.json` — pre-seeded with two `footer-link-column` blocks ("Information" pointing at the `footer` menu handle Shopify creates by default; "Follow Us" left with a blank menu, matching Step 5's blank-image precedent — it renders as an empty column, not broken, until a merchant assigns a menu). `logo` left blank.
- `locales/en.default.schema.json` — added `general.footer_link_column` ("Link column"); reused existing `labels.heading`, `labels.logo`, `labels.menu`. Removed the now-orphaned `labels.show_payment_icons` (no remaining references after the payment-icons feature was dropped — see below).
- `locales/en.default.json` — added `general.footer.copyright_html` and a `general.newsletter` object (`email_label`, `email_placeholder`, `submit`, `success`) for the new form. Field-level validation errors reuse Shopify core's own translated `form.errors.translated_fields` / `form.errors.messages` — no new keys needed for those.
- `docs/landing-page/PROGRESS.md` — checked off "Footer".

No changes to `templates/index.json`, `config/settings_schema.json`, or the header/hero/categories/trending/story sections.

## Design decisions & rationale

- **Link columns reference Shopify navigation menus, not hardcoded link fields.** Confirmed with the project owner: mirrors `header.liquid`'s existing `menu` (`link_list`) pattern for native, familiar content management, at the cost of a column rendering empty until a merchant creates/assigns a matching menu — the same tradeoff Step 5 already documented for its own blank-by-default image.
- **Dropped the stock footer's "Powered by Shopify" text and `show_payment_icons` toggle entirely.** Confirmed with the project owner: matches the wireframe's clean "© 2024 Atelier Studio. All rights reserved." copyright-only design rather than preserving unrelated stock functionality the wireframe never called for.
- **Blocks wrapped in their own `.footer__links` subgrid** (2 columns at desktop) rather than targeting `.shopify-block` wrapper elements by position within the outer 12-column grid — mixing non-block siblings (newsletter, brand columns) with `content_for 'blocks'` output in the same grid made position-based selectors (`:nth-child`/`:nth-of-type`) unreliable, since Shopify wraps each block in its own `.shopify-block` div. A nested subgrid sidesteps this entirely: blocks just flow into 2 columns naturally, no positional CSS hacks needed.
- **Newsletter placeholder/label text uses normal sentence case** ("Email address"), not the wireframe's literal all-caps "EMAIL ADDRESS" — the wireframe's caps styling is handled by CSS classes (`.text-label-caps`) elsewhere in the theme, and locale strings consistently stay in normal case so any future class-level typography changes aren't baked into hardcoded shouty text.
- **No new CSS custom properties.** Reuses `--spacing-section-gap`, `--spacing-gutter`, `--page-width`, `--page-margin`, `--color-surface-container-low`, `--color-outline`/`-variant`, `--color-on-surface`/`-variant`, `--color-primary`, `--radius`, `--motion-fade`, and the existing `.text-label-caps`/`.text-label-sm` utilities.

## Responsive behavior

- **<768px**: single column, stacked in source order — Newsletter, then the link-column blocks in their configured order, then Logo/Copyright. Full-bleed background/border still spans the viewport edge-to-edge; padding adjusts via the existing mobile `--page-margin` override.
- **≥768px**: 12-column grid — Newsletter (cols 1–4), link columns (cols 6–9, in their own 2-column subgrid), Logo/Copyright (cols 10–12, right-aligned, logo pinned top and copyright pinned bottom via `justify-content: space-between` on a full-height flex column).

Verified visually at ~1440px (desktop 4-column split) and ~390px (mobile single-column stack) via `shopify theme dev`.

## Design tokens introduced

None — reuses tokens listed above from prior steps.

## How to review/preview

1. `shopify theme dev` and open the homepage, scroll to the footer.
2. **Desktop (~1440px)**: confirm the 4-column layout; click into the email input and confirm the bottom border turns black on focus; submit the newsletter form with a valid email and confirm the success message appears (check Shopify admin → Customers for the test signup), then submit an invalid email and confirm the inline error message renders.
3. **Mobile (~390px)**: confirm single-column stacking in the order Newsletter → Information → Follow Us → Logo/Copyright, with the hairline border-top still spanning full width.
4. In the theme editor: confirm the Footer section exposes a Logo image picker and Newsletter heading field, and that the two "Information"/"Follow Us" blocks each expose an editable heading + menu picker; add/remove/reorder blocks up to `max_blocks: 4` and confirm the columns reflow; assign an empty menu and confirm the column renders with just its heading (not broken).
5. `shopify theme check` — should report only the same pre-existing warning baseline as Step 5 (the Google Fonts `RemoteAsset` warnings), no new errors.

## Checklist

- [x] PROGRESS.md updated
- [x] Committed
