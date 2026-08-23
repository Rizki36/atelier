---
name: Atelier Minimalist
colors:
  surface: '#f9f9f9'
  surface-dim: '#dadada'
  surface-bright: '#f9f9f9'
  surface-container-lowest: '#ffffff'
  surface-container-low: '#f3f3f4'
  surface-container: '#eeeeee'
  surface-container-high: '#e8e8e8'
  surface-container-highest: '#e2e2e2'
  on-surface: '#1a1c1c'
  on-surface-variant: '#4c4546'
  inverse-surface: '#2f3131'
  inverse-on-surface: '#f0f1f1'
  outline: '#7e7576'
  outline-variant: '#cfc4c5'
  surface-tint: '#5e5e5e'
  primary: '#000000'
  on-primary: '#ffffff'
  primary-container: '#1b1b1b'
  on-primary-container: '#848484'
  inverse-primary: '#c6c6c6'
  secondary: '#5f5e5b'
  on-secondary: '#ffffff'
  secondary-container: '#e5e2dd'
  on-secondary-container: '#656461'
  tertiary: '#000000'
  on-tertiary: '#ffffff'
  tertiary-container: '#1b1c1c'
  on-tertiary-container: '#848484'
  error: '#ba1a1a'
  on-error: '#ffffff'
  error-container: '#ffdad6'
  on-error-container: '#93000a'
  primary-fixed: '#e2e2e2'
  primary-fixed-dim: '#c6c6c6'
  on-primary-fixed: '#1b1b1b'
  on-primary-fixed-variant: '#474747'
  secondary-fixed: '#e5e2dd'
  secondary-fixed-dim: '#c9c6c2'
  on-secondary-fixed: '#1c1c19'
  on-secondary-fixed-variant: '#474743'
  tertiary-fixed: '#e4e2e2'
  tertiary-fixed-dim: '#c7c6c6'
  on-tertiary-fixed: '#1b1c1c'
  on-tertiary-fixed-variant: '#464747'
  background: '#f9f9f9'
  on-background: '#1a1c1c'
  surface-variant: '#e2e2e2'
typography:
  display-lg:
    fontFamily: Playfair Display
    fontSize: 64px
    fontWeight: '400'
    lineHeight: '1.1'
    letterSpacing: -0.02em
  display-lg-mobile:
    fontFamily: Playfair Display
    fontSize: 40px
    fontWeight: '400'
    lineHeight: '1.1'
  headline-md:
    fontFamily: Playfair Display
    fontSize: 32px
    fontWeight: '400'
    lineHeight: '1.2'
  headline-sm:
    fontFamily: Playfair Display
    fontSize: 24px
    fontWeight: '400'
    lineHeight: '1.3'
  body-lg:
    fontFamily: DM Sans
    fontSize: 18px
    fontWeight: '400'
    lineHeight: '1.6'
    letterSpacing: 0.01em
  body-md:
    fontFamily: DM Sans
    fontSize: 16px
    fontWeight: '400'
    lineHeight: '1.6'
  label-caps:
    fontFamily: DM Sans
    fontSize: 12px
    fontWeight: '500'
    lineHeight: '1.0'
    letterSpacing: 0.1em
  label-sm:
    fontFamily: DM Sans
    fontSize: 13px
    fontWeight: '400'
    lineHeight: '1.4'
spacing:
  unit: 8px
  container-max: 1440px
  gutter: 24px
  margin-desktop: 64px
  margin-mobile: 20px
  section-gap: 120px
---

## Brand & Style
The brand personality is architectural, silent, and intentional. This design system targets a sophisticated audience that values "quiet luxury" and structural clarity. The emotional response should be one of calm, confidence, and exclusivity.

The style is a fusion of **High-End Minimalism** and **Modern Editorial**. It relies on expansive whitespace to create a sense of breathing room, treating the interface as a gallery where the product photography remains the focal point. Elements are stripped of unnecessary ornamentation, utilizing thin hair-lines and precise alignment to convey quality.

## Colors
The palette is rooted in a monochromatic foundation to maintain a high-contrast, editorial feel. 

- **Primary (Deep Black):** Used for primary typography and structural borders.
- **Secondary (Warm Beige):** A subtle, sophisticated accent used for highlight areas, soft backgrounds, or call-to-action surfaces to prevent the design from feeling sterile.
- **Tertiary (Soft Gray):** Reserved for secondary text, metadata, and subtle dividers.
- **Neutral (Pure White):** The core canvas color to ensure maximum clarity and light.

## Typography
The typographic hierarchy relies on the tension between the classic, high-contrast **Playfair Display** (Serif) and the functional, understated **DM Sans** (Sans-Serif). 

- **Headlines:** Use serif fonts to establish an editorial rhythm. Large displays should use negative letter-spacing for a tighter, more modern look.
- **Body:** DM Sans provides an airy, legible experience. Increased line-height (1.6) is mandatory to support the minimalist aesthetic.
- **Labels:** Use uppercase styling with generous letter-spacing (0.1em) for category headers, buttons, and navigation items to evoke luxury branding.

## Layout & Spacing
The layout follows a **Fixed Grid** philosophy on desktop to maintain a curated, boutique-like frame.

- **Desktop:** 12-column grid with a 1440px max-width. Use wide 64px outer margins to push content toward the center, mimicking a fashion lookbook.
- **Section Gaps:** Use aggressive vertical spacing (120px+) between major sections to emphasize the "less is more" brand value.
- **Alignment:** Use asymmetrical layouts for imagery while keeping typography strictly aligned to the grid's vertical axes.

## Elevation & Depth
This design system avoids shadows to maintain its architectural, flat aesthetic. Depth is achieved through **Tonal Layering** and **Line Work**.

- **Surfaces:** Use the Warm Beige (Secondary) as a base for container elements against a White (Neutral) background to create soft separation.
- **Outlines:** Use 1px solid Deep Black or Soft Gray borders to define structures. These should feel like technical drawings—thin and precise.
- **Transitions:** Use simple opacity fades (0ms to 300ms) for hover states rather than lifting elements off the page.

## Shapes
The shape language is strictly **Sharp (0px)**. 

All buttons, inputs, image containers, and cards must have square corners. This reinforces the architectural and high-end fashion feel, distinguishing it from mass-market consumer apps that favor rounded, "friendly" corners.

## Components

- **Buttons:** Primary buttons are solid Deep Black with White label-caps typography. Secondary buttons use a 1px Black border with a transparent background. No icons unless strictly necessary for utility.
- **Input Fields:** Bottom-border only (1px Soft Gray), transforming to Deep Black on focus. Labels should use the `label-caps` style positioned above the line.
- **Cards:** Borderless by default. The depth is created by the image within the card. Titles are placed below the image in `headline-sm` or `body-lg`.
- **Chips/Tags:** Minimalist rectangles with 1px Soft Gray borders and `label-sm` text. Use for sizes, colors, or categories.
- **Lists:** Clean rows separated by 1px Soft Gray hair-lines. High vertical padding (24px) between items.
- **Image Placeholders:** Use a 4:5 or 2:3 aspect ratio. Images should always be center-cropped and utilize a desaturated or high-contrast filter to match the brand.