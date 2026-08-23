# Step 2 — Hero

## What was built

- A new `sections/hero.liquid`: a full-bleed, viewport-height hero with an optional darkened/desaturated background image behind a left-aligned translucent "glass" content panel (eyebrow label, serif display headline, body copy, primary CTA button). Replaces the placeholder `hello-world` section as the homepage's first (and currently only) section.
- A new reusable `snippets/button.liquid` (primary/secondary variants per DESIGN.md's button system), introduced now rather than hand-rolled inline, since Steps 3–6 will likely need CTAs again.
- Two small gaps in Step 1's token system were fixed as part of this step (see below).

## Files changed

New:
- `sections/hero.liquid`
- `snippets/button.liquid`
- `docs/landing-page/STEP-2.md` — this file.

Modified:
- `snippets/css-variables.liquid` — added `--color-tertiary-container`/`--color-on-tertiary-container` (defined by DESIGN.md but missed in Step 1) and two new alpha-blended tokens for the glass panel, `--color-surface-glass`/`--color-outline-variant-glass`, computed via the same `color_modify` filter pattern already used for `--color-header-scrim`.
- `templates/index.json` — now renders `hero` (was `hello-world` under a generic `"main"` key).
- `locales/en.default.schema.json` — added `general.hero` and `labels.eyebrow`/`heading`/`body`/`button_label`/`button_link`/`image`.

Removed:
- `sections/hello-world.liquid` — dead placeholder, confirmed unreferenced anywhere else in the theme before deleting.

## Design decisions & rationale

- **Translucent glass panel kept, despite tension with DESIGN.md's flat/no-blur aesthetic.** DESIGN.md avoids shadows and lifting but doesn't explicitly rule on blur/translucency. The wireframe's hero panel uses a semi-transparent background + `backdrop-filter: blur` + faint border. Per explicit user decision (parallel to Step 1's rounded-avatar exception), the glass panel is kept exactly as the wireframe shows it — the same `blur(8px)` value is already precedented by the header's own scrim, so it's not a new pattern in the theme.
- **New `snippets/button.liquid` instead of inline button markup.** DESIGN.md defines a real two-variant button system (primary solid-black, secondary black-outline), and Steps 3–6 will very likely need CTAs again. Unlike header.liquid's single-use icon-buttons, this is an explicitly named, reusable system component — worth extracting now rather than duplicating across four more sections.
- **`--color-tertiary-container`/`--color-on-tertiary-container` added.** These are defined in DESIGN.md's token front-matter and needed for the button's hover state, but were missed when Step 1 wired the token system. Added as fixed CSS vars, following Step 1's own precedent that supporting tones aren't merchant settings.
- **Heading uses `textarea` + `newline_to_br`, not a hardcoded `<br>`.** Keeps the wireframe's explicit line break after "of" as a merchant-editable default rather than baking wireframe-specific copy into markup — any heading text works correctly, with or without a line break.
- **`templates/index.json` key renamed from generic `"main"` to `"hero"`.** More legible once Steps 3–6 accumulate more section entries in this file.
- **Image still rendered via `snippets/image.liquid`, not bare `image_tag`** (unlike `custom-section.liquid`'s reference pattern, which uses bare `image_url | image_tag`). Kept a single canonical image-rendering path across the theme rather than introducing a second convention.

## Responsive behavior

Height (`85vh`, `min-height: 600px`), left-alignment, and panel max-width stay constant across breakpoints, matching the wireframe (it never restructures the Hero at any screen size). Only the panel padding and the headline size respond:
- **<768px**: panel padding `32px`; headline at 40px (`.text-display-lg`'s existing mobile size from Step 1).
- **≥768px**: panel padding `48px`; headline at 64px.

Verified visually at 1440px (desktop) and 500px (mobile) via `shopify theme dev` — panel stays readable and doesn't overflow the viewport at either width.

## Design tokens introduced

- `--color-tertiary-container`, `--color-on-tertiary-container` (fixed, from DESIGN.md's front-matter — gap fix, not new to the system).
- `--color-surface-glass`, `--color-outline-variant-glass` (derived via `color_modify`, alpha-blended for the glass panel).

No new spacing/typography tokens were needed — the Hero consumes `.text-display-lg`/`.text-label-caps`/`.text-body-lg` and the `--content-grid`/`full-width` idiom entirely as built in Step 1.

## How to review/preview

1. `shopify theme dev` and open the homepage.
2. Confirm the Hero renders directly below the fixed header: eyebrow "New Collection", headline "The Art of Simplicity" (line break after "of"), body copy, and a solid black "Shop the Collection" button.
3. Hover the button — background should fade to the dark tertiary-container tone, no shadow/lift.
4. Resize to mobile width (<768px) — panel padding shrinks, headline drops to 40px, layout stays left-aligned and doesn't overflow.
5. In the theme editor, select the Hero section and confirm Image, Eyebrow, Heading, Body text, Button label, and Button link all appear and are editable; try picking a background image and confirm it renders full-bleed with a darkened/desaturated (multiply-blend, high-contrast, desaturated) treatment behind the panel.
6. `shopify theme check` — should report only the same 3 expected `RemoteAsset` warnings from Step 1 (Google Fonts), no errors.

## Checklist

- [x] PROGRESS.md updated
- [x] Committed
