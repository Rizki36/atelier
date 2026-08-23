# Step 1 — Header

## What was built

- The Atelier design system from `DESIGN.md` was wired into the theme's settings and CSS-variable system for the first time (nothing from it existed in the theme before this step — see "Design tokens introduced" below). All later steps will consume these same tokens without needing to add more.
- `sections/header.liquid` was rebuilt from the bare Skeleton default into the full Atelier header: logo + shop-name wordmark, primary navigation (theme-editor `link_list`, default `main-menu`, active-link styling via Shopify's built-in `link.active`), and a utility icon cluster (search, cart with item-count badge, account).
- A mobile navigation pattern was designed and built from scratch — the wireframe has no mobile nav at all (`nav` is just `hidden lg:flex`). Below 1024px, a hamburger button opens a full-screen `<dialog scroll-lock>` with the same links/icons, closed via a close button, the Escape key, or a backdrop click (all native `<dialog>` behavior), with an opacity-only fade transition.

## Files changed

New:
- `assets/icon-search.svg`, `assets/icon-menu.svg`, `assets/icon-close.svg` — new inline icons, following the existing `icon-cart.svg`/`icon-account.svg` convention (20×20 viewBox, `stroke="currentColor"`, `stroke-width="var(--icon-stroke-width)"`).
- `docs/landing-page/STEP-1.md` — this file.

Modified:
- `config/settings_schema.json` — added 4 new color settings (`primary_color`, `secondary_color`, `secondary_container_color`, `outline_color`); repointed `background_color`/`foreground_color` defaults to Atelier values; `min_page_margin` default `20` → `64`; `input_corner_radius` default `4` → `0`.
- `snippets/css-variables.liquid` — added the full Atelier `--color-*` token set, `--font-headline`/`--font-body`, the spacing scale, `--radius`, `--icon-stroke-width`, `--motion-fade`, `--header-height`, and a mobile `--page-margin` override.
- `layout/theme.liquid` — added a Google Fonts `<link>` for Playfair Display + DM Sans (fixed brand fonts, not merchant-editable).
- `assets/critical.css` — added `.text-display-lg`/`.text-headline-md`/`.text-headline-sm`/`.text-body-lg`/`.text-body-md`/`.text-label-caps`/`.text-label-sm` typography utilities, `body { padding-top: var(--header-height) }` (needed the moment the header goes `position: fixed`), and a breakpoint-convention comment.
- `locales/en.default.schema.json` — added labels for the 5 new settings.
- `locales/en.default.json` — added a `general.accessibility` block (`search`, `cart`, `account`, `menu`, `close`) for icon-button `aria-label`s.
- `sections/header.liquid` — full rebuild (see above).

## Design decisions & rationale

- **Merchant-editable vs. fixed color tokens.** DESIGN.md defines ~35 color tokens. Exposing all of them as theme settings would produce an unusable editor panel for a single-brand landing page. Only the 6 tones a merchant would plausibly want to adjust (primary, secondary, secondary-container, background, foreground/on-surface, outline) are settings; the ~29 supporting tones (surface variants, on-* pairs, fixed/dim variants, etc.) are hardcoded CSS variables taken directly from DESIGN.md's front matter, since they're part of one coherent tonal system a merchant shouldn't be expected to keep in sync by hand.
- **Rounded account avatar kept, despite DESIGN.md's sharp-corners mandate.** DESIGN.md requires 0px radius on all buttons/inputs/cards, but the wireframe's account icon is a circular avatar. Per explicit user decision, the circle is kept as a deliberate one-off exception — avatars are conventionally circular even in otherwise-sharp systems. Every other interactive element (nav, buttons, mobile-nav dialog) is sharp.
- **Nav uses `link_list`, not hardcoded links.** The wireframe hardcodes 4 static links; the header instead reuses (and fixes) the existing `section.settings.menu` `link_list` pattern, giving it `"default": "main-menu"` (it previously had no default and rendered empty). This is the correct Shopify convention — nav should be merchant-editable, not baked into the section.
- **Mobile nav uses native `<dialog scroll-lock>`, not a custom drawer.** `assets/critical.css` already had an unused rule (`html:has(dialog[scroll-lock][open]...) { overflow: hidden; }`) suggesting the theme was designed around this primitive. Reusing it means scroll-lock, Escape-to-close, and top-layer stacking all come free from the browser, and the fade transition is achieved with `@starting-style` + `transition-behavior: allow-discrete` rather than a hand-rolled open/close animation.
- **Search is a link to `routes.search_url`, not a predictive-search flyout.** The wireframe's search button has no defined behavior. A full predictive-search UI is out of scope for a landing-page header; linking to the search page is the minimal correct behavior.
- **Google Fonts loaded via CDN `<link>`, not self-hosted.** Self-hosting Playfair Display/DM Sans as `@font-face` would be more performant and avoid the theme-check `RemoteAsset` warning, but requires binary font files that can't be added in this pass. The 3 `RemoteAsset` warnings from `shopify theme check` on `layout/theme.liquid` are expected and accepted for now.

## Responsive behavior

Two breakpoints, used consistently (documented in `assets/critical.css`):
- **768px** — general content reflow for later sections (not yet exercised by the header itself).
- **1024px** — the header's own breakpoint: desktop horizontal nav is hidden below it and replaced by the hamburger button; the hamburger is hidden at/above it. The mobile-nav `<dialog>` is force-hidden above 1024px as a safety net (e.g. resizing the window while it's open).

Below 1024px, tapping the hamburger opens a full-screen dialog listing the same nav links plus search/cart/account, sized and touch-friendly for mobile.

## Design tokens introduced

All new — this is the first step, so it also carries the foundational token work. Later steps consume these without adding more:
- Colors: `--color-primary`, `--color-on-primary`, `--color-primary-container`, `--color-on-primary-container`, `--color-secondary`, `--color-on-secondary`, `--color-secondary-container`, `--color-on-secondary-container`, `--color-tertiary`, `--color-on-tertiary`, `--color-surface`, `--color-surface-dim`, `--color-surface-bright`, `--color-surface-container-lowest/low/(none)/high/highest`, `--color-on-surface`, `--color-on-surface-variant`, `--color-outline`, `--color-outline-variant`, `--color-inverse-surface`, `--color-inverse-on-surface`, `--color-header-scrim`.
- Typography: `--font-headline`, `--font-body`, plus `.text-display-lg`/`.text-headline-md`/`.text-headline-sm`/`.text-body-lg`/`.text-body-md`/`.text-label-caps`/`.text-label-sm` utility classes in `critical.css`.
- Spacing: `--spacing-unit`, `--spacing-gutter`, `--spacing-margin-mobile`, `--spacing-section-gap` (existing `--page-width`/`--page-margin` now carry DESIGN.md's 1440px/64px-desktop/20px-mobile values).
- Other: `--radius`, `--icon-stroke-width`, `--motion-fade`, `--header-height`.

## How to review/preview

1. `shopify theme dev` and open the homepage.
2. **Desktop (≥1024px):** logo/wordmark on the left, horizontal nav in the center/right, search/cart/account icons on the right. Add an item to the cart and confirm the badge appears. Confirm the fixed header has a blurred translucent background and doesn't overlap page content below it.
3. **Mobile (<1024px):** nav is replaced by a hamburger icon. Tap it — a full-screen menu should fade in with the same links and icons. Confirm it closes via the × button, the Escape key, and tapping the dimmed backdrop, and that page scroll is locked while it's open.
4. **Theme editor:** confirm the new color settings (Primary, Secondary, Secondary container, Outline, plus updated Background/Foreground) appear under **Theme settings → Colors**, and that the Header section has a **Logo** image picker and a **Menu** link-list setting.
5. `shopify theme check` — should report only the 3 expected `RemoteAsset` warnings on `layout/theme.liquid` (Google Fonts), no errors.

## Checklist

- [x] PROGRESS.md updated
- [ ] Committed
