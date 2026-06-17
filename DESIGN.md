---
version: "alpha"
name: "Client Name"
description: "A clean small-business starter template. Replace the content, tune the variables, and deploy."
colors:
  primary: "#1a1a1a"
  on-primary: "#ffffff"
  secondary: "#f7f5f0"
  accent: "#1a1a1a"
  neutral: "#ffffff"
  surface: "#f7f5f0"
  text: "#1a1a1a"
  text-muted: "#6b6258"
  border: "#d4ccc4"
typography:
  h1:
    fontFamily: "Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, Segoe UI, sans-serif"
    fontSize: "clamp(3rem, 8vw, 5.5rem)"
    fontWeight: 800
    lineHeight: 0.92
    letterSpacing: "-0.015em"
  h2:
    fontFamily: "Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, Segoe UI, sans-serif"
    fontSize: "2.25rem"
    fontWeight: 700
    lineHeight: 1.12
    letterSpacing: "0em"
  body:
    fontFamily: "Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, Segoe UI, sans-serif"
    fontSize: "1rem"
    fontWeight: 400
    lineHeight: 1.65
    letterSpacing: "0em"
  eyebrow:
    fontFamily: "Inter, ui-sans-serif, system-ui, -apple-system, BlinkMacSystemFont, Segoe UI, sans-serif"
    fontSize: "0.78rem"
    fontWeight: 700
    lineHeight: 1.2
    letterSpacing: "0.08em"
rounded:
  xs: "2px"
  sm: "4px"
  md: "8px"
  pill: "999px"
spacing:
  xs: "0.25rem"
  sm: "0.5rem"
  md: "1rem"
  lg: "1.5rem"
  xl: "2rem"
  section: "clamp(4rem, 9vw, 7rem)"
components:
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.on-primary}"
    rounded: "{rounded.sm}"
    padding: "0.75rem 1.1rem"
  button-secondary:
    backgroundColor: "{colors.neutral}"
    textColor: "{colors.text}"
    rounded: "{rounded.sm}"
    padding: "0.75rem 1.1rem"
  link:
    textColor: "{colors.accent}"
  muted-text:
    textColor: "{colors.text-muted}"
  divider:
    backgroundColor: "{colors.border}"
  section-surface:
    backgroundColor: "{colors.surface}"
    textColor: "{colors.text}"
  card:
    backgroundColor: "{colors.neutral}"
    textColor: "{colors.text}"
    rounded: "{rounded.md}"
---

## Overview

A clean Astro + Decap CMS starter for small business websites. One homepage, five inner pages, blog, CMS-driven collections, and a component library with live previews at `/blocks/`. Deploy to GitHub Pages with one push.

The design language is warm, material-conscious, and professional. Photography carries the emotional weight; the layout stays out of the way. Replace the content, tune the brand variables, and ship.

## Colors

The default palette is neutral and clean. It ships with a charcoal primary — understated, professional, works for any trade. Adjust for each client.

- **Primary:** Default action color — button fills, links, accent elements.
- **On Primary:** Text on primary backgrounds. Preserves WCAG AA contrast.
- **Accent:** Strong near-black for headings and brand marks.
- **Surface:** Warm off-white section background for separating content bands.
- **Text:** Main readable foreground.
- **Text Muted:** Paragraphs, captions, metadata, and secondary labels.
- **Border:** Warm-toned border. Avoids sterile blue-grey defaults.
- **Neutral:** Page background and card surfaces. Clean, open.

Update colors in this order: `--brand-primary`, `--brand-primary-contrast`, `--brand-name-color`, `--brand-secondary`, then `--brand-accent`. Component-specific color overrides should come only after semantic aliases are insufficient.

## Typography

The system uses Inter by default through `--font-sans`, with system fallbacks. Typography stays direct and trade-confident. No decorative flourishes.

Use one `h1` per page. Change `--font-body` and `--font-display` only if a clear brand direction emerges. Avoid adding web font dependencies unless they materially improve the finished result.

## Layout

Use `Section` for vertical rhythm and background variants. Use `.container` and `.container--narrow` for readable line lengths. Pages should be small and direct — visitors need clarity, proof of quality, and a clear contact path.

**Default homepage order:**
1. Hero — headline, intro, primary CTA
2. Introduction — two-column editorial statement
3. Featured work — full-width photography
4. Services — collection-driven grid
5. Process — simple step sequence
6. Testimonial — single editorial quote
7. CTA band — contact prompt

For a new client, configure in this order:
1. `src/content/settings/site.json`: name, description, URL, contact details, CTA.
2. Brand color primitives in `src/styles/tokens.css`.
3. Font variables if the brand has a clear type direction.
4. Section rhythm and radii only if the client needs a different visual style.
5. Component-specific CSS only after the global tokens are insufficient.

Visit `/blocks/` during local preview to compare available component variants before creating new components.

## Elevation & Depth

The system favors borders, spacing, and surface changes over heavy shadows. Use `--shadow-subtle` only for small lifts where a component needs separation. Avoid gradient-heavy backgrounds, decorative blobs, nested card layouts, and default stock-like visual effects.

Real client photography should replace all placeholders before launch. The images are the decoration.

## Shapes

Radii are restrained: `2px`, `4px`, and `8px` cover most UI needs. Buttons default to `4px`. Cards and repeated content items may use `8px`. Gallery images have no radius — let them bleed full. Pill radius is reserved for compact tags or controls.

## Components

Compose pages from existing components before creating new ones. Prefer collection-driven components for services, posts, testimonials, and FAQs.

Use `Hero` only on the home page. Use `PageIntro` for inner pages. Browse available variants at `/blocks/`.

Client-editable homepage variant controls live in `src/content/pages/home.json` and the Decap CMS Pages collection. Keep client-editable variant controls limited to safe layout choices. Do not expose raw spacing, color, typography, or arbitrary component controls to clients.

## Themes

The template ships with a `data-theme` attribute system for colouring sections within the brand palette. Four themes are defined in `src/styles/tokens.css`:

| Theme | `data-theme` | Background | Purpose |
|---|---|---|---|
| Default | _(none)_ | White | Standard page sections |
| Surface | `surface` | Warm off-white (`--color-soft`) | Alternating content bands — rhythm without contrast |
| Dark | `dark` | Near-black (`#111111`) | Footers, trust bars, hero overlays — structural zones |
| Primary | `primary` | Brand primary (`--brand-primary`) | High-impact CTAs, feature panels — one per page max |

**How it works:** Each theme block overrides 10 semantic tokens (`--color-page`, `--color-text`, `--color-text-muted`, `--color-heading`, `--color-link`, `--color-section`, `--color-rule`, `--color-button`, `--color-button-text`, `--color-button-outline`). Components reference these tokens — they don't know which theme they're in, the `data-theme` ancestor handles everything. Put `data-theme="dark"` on a container and every child (text, links, buttons, borders, headings) shifts automatically.

**Using themes in pages:**

```astro
<!-- Via Section component -->
<Section theme="primary" title="Get started today">

<!-- Via data-theme attribute on any element -->
<div data-theme="dark">
  <h2>This heading is white</h2>
  <a class="button" href="/contact/">This button is light-filled</a>
</div>
```

**Guidelines:**

- Use `surface` to break up long default-theme pages without introducing a colour change.
- Use `dark` only for structural zones: footer, hero overlay, trust bar. These components already ship with `data-theme="dark"`.
- Use `primary` once per page at most — it carries the highest visual weight.
- Do not nest themes. A `data-theme` container resets the context for everything inside it.
- When a brand swap changes `--brand-primary`, every theme that references it recalculates automatically.

**Adding a theme:** Copy a `[data-theme="..."]` block, rename the key, adjust the values. All components inherit the new colours with zero changes.

## Do's and Don'ts

Do keep copy specific to each client: real service details, real testimonials, real locations, real contact info.

Do lead with a clear quote or contact path.

Do use real client photography — it is the strongest brand asset most small businesses have.

Do write copy that sounds like a person in that trade talking to a customer. Direct, benefit-led, specific.

Don't use stock imagery if real photography is available.

Don't add decorative CSS effects — no parallax, no particle backgrounds, no animated counters.

Don't over-design the contact form. Make it functional and obvious.

Don't introduce a second accent colour until the primary palette is settled.

Don't let the template's neutral defaults become the final brand direction.
