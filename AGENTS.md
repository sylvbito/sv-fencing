# Agent Guide

This repository is designed for AI-assisted client build-outs. Preserve the template’s simplicity unless the client brief clearly requires more.

## Default Workflow

1. Read `CLIENT_BRIEF.example.md` and the actual client brief supplied for the build.
2. Update `src/content/settings/site.json`.
3. Replace content in `src/content/services`, `src/content/posts`, `src/content/testimonials`, and `src/content/faqs`.
4. Adjust `src/styles/tokens.css` for brand primitives.
5. Visit `/blocks/` during local preview to compare available component variants.
6. Compose pages from existing components before creating new ones.
7. **Take a full-page screenshot and visually review each section before handing work back.** Do not ship blind.
8. Run `npm run build` before handing work back.

## Rules

- Keep brand changes concentrated in `src/styles/tokens.css`, `src/styles/theme.css`, and site settings.
- Do not add UI libraries unless explicitly requested.
- Do not add client-side JavaScript for static presentation sections.
- Prefer plain Astro components with clear props.
- Keep copy specific to the client. Remove placeholder explanations from public pages before launch.
- Keep pages small and direct. Small-business visitors usually need clarity, proof, and contact paths.
- Preserve accessibility basics: one `h1`, labelled controls, useful alt text, visible focus states, and semantic landmarks.

## Component Guidance

- Use `Hero` only on the home page unless the client needs a campaign-style landing page.
- Use `PageIntro` for inner pages.
- Use `Section` for layout rhythm and background variants.
- Use `TwoColumnMedia` for image + text layouts on service and about pages (distinct from Hero split-media).
- Use `StatsBar` for credibility strips — big numbers, small labels, works on any theme.
- Use collection-driven components for services, posts, testimonials, and FAQs.
- Create new components only when a repeated pattern appears or a page becomes hard to scan.
- Prefer existing variants before building new components: hero variants, service layouts, testimonial layouts, gallery layouts, and CTA layouts are documented on `/blocks/`.
- For patterns that vary per project (team grids, pricing, timelines, logo walls), use a recipe from `SECTION_RECIPES.md` instead of building a new component.
- Keep client-editable variant controls limited to safe layout choices in `src/content/pages/home.json`.

## Decap Guidance

- Any field added to a content schema should also be added to `public/admin/config.yml`.
- Any new collection should be documented in `README.md`.
- Keep filenames slug-friendly and content portable.

## Stock Photography

Source photos without a browser. Workflow:

1. **Search** — find an Unsplash photo page via web_search
   ```
   web_search("unsplash.com interior wood floor living room")
   ```
2. **Extract the photo ID** — photo page URLs contain the ID, e.g. `Kq8uV6tZM20` from `.../photo-Kq8uV6tZM20`
3. **Download direct** — build the download URL and curl it:
   ```
   curl -sL -o public/uploads/filename.jpg \
     "https://images.unsplash.com/photo-{ID}?w=2000&q=85"
   ```
   Add `&fit=crop&h={height}` for specific aspect ratios.
4. **Wire it in** — reference `/uploads/filename.jpg` in the component or page.
5. **Keep it lean** — hero images ~500-700KB fine; resize gallery images to `w=800&h=1100`.

No browser, no API key, no page rendering.

## Visual Review Protocol

Before sharing any build back to the user:

1. **Build the site** — `npm run build`
2. **Serve locally** — `python3 -m http.server 8888` from `dist/`
3. **Take a full-page screenshot** — using Playwright or similar
4. **Review each section** — check headings, spacing, image loading, button styles, colours, mobile layout
5. **Fix issues found** before reporting back

Do not rely on code correctness alone. The rendered page is what matters.

## Button Convention

Use a clear three-tier hierarchy:

| Level | Style | Class | Usage |
|---|---|---|---|
| Primary | Filled background | `.button` | Main CTAs, conversion actions |
| Secondary | Ghost/outlined | `.button--secondary` | Alternative actions on same surface |
| Light (solid) | Light fill for dark backgrounds | `.button--solid-light` | Primary action on dark sections |
| Light (outline) | Light outline for dark backgrounds | `.button--outline-light` | Secondary action on dark sections |

Buttons are theme-aware — `.button` and `.button--secondary` use semantic tokens that change per `data-theme` context. On a dark theme, `.button` becomes a light-filled button automatically; `.button--secondary` becomes a light outline. The `--light` variants are explicit overrides for cases where you need specific behaviour regardless of theme context.

## Brand Theme System

The template uses a `data-theme` attribute system so any container can declare its colour treatment. Four themes are defined in `src/styles/tokens.css`:

| Theme | `data-theme` | Background | Use for |
|---|---|---|---|
| Default | _(none)_ | White | Standard page sections |
| Surface | `surface` | Warm off-white | Alternating content bands |
| Dark | `dark` | Near-black | Footers, trust bars, hero overlays, contrast panels |
| Primary | `primary` | Brand primary colour | High-impact CTAs, feature panels, campaign sections |

Each theme overrides 9 semantic colour tokens (`--color-page`, `--color-text`, `--color-text-muted`, `--color-heading`, `--color-link`, `--color-section`, `--color-rule`, `--color-button`, `--color-button-text`). Components use these tokens without knowing which theme they're in — the `data-theme` ancestor handles everything.

**Using themes:**

1. **`Section` component** — `<Section theme="dark">` sets `data-theme="dark"` on the section and applies `background: var(--color-page)`.
2. **`data-theme` attribute** — put it directly on any element: `<div data-theme="primary">`. TrustBar, Footer, ServiceGrid icon-grid, and Hero image-full use this.
3. **Default** — no attribute means the default light theme.

**When to use themes:**

- Alternate adjacent sections with default and surface to create visual rhythm.
- Use dark for the footer, trust bar, and any full-bleed hero — these components already have `data-theme="dark"` built in.
- Use primary sparingly for high-impact CTAs or feature panels — one per page maximum.
- Do NOT use a theme just to make something "look different." Themes carry semantic weight — dark means "this is a distinct zone" (footer, hero overlay), primary means "this is the most important thing on the page."

**Adding a new theme:**

Copy a `[data-theme="..."]` block in `tokens.css`, change the values. All components pick it up automatically — no component changes needed. Keep the 9 semantic tokens consistent: `--color-page`, `--color-text`, `--color-text-muted`, `--color-heading`, `--color-link`, `--color-section`, `--color-rule`, `--color-button`, `--color-button-text`, `--color-button-outline`.

**Brand swap:** Change the 5 brand primitives and the neutral palette in `tokens.css`. Every theme recalculates from those values. The `primary` theme uses `var(--brand-primary)` — swap that one hex and the whole primary theme shifts.

## Section Spacing

Adjacent sections with the same background colour should compress their gap (remove the second section's top padding). Sections with different background colours keep full spacing so the colour change has room to breathe. This is handled via CSS:

```css
.section:not(.section--surface) + .section:not(.section--surface),
.section--surface + .section--surface {
  padding-top: 0;
}
```

## FAQ Accordion Pattern

For smooth open/close transitions on FAQ items, use `grid-template-rows` with `overflow: hidden`:

```css
.faq-answer {
  display: grid;
  grid-template-rows: 0fr;
  overflow: hidden;
  transition: grid-template-rows 0.3s ease;
}
details[open] .faq-answer {
  grid-template-rows: 1fr;
}
```

Key rules:
- `overflow: hidden` goes on the grid container, not a child
- Inner `<p>` must have `margin: 0`
- Use a spacer `<div>` for bottom gap (no padding/margin on animated elements)
- The inner track collapses to zero because `overflow: hidden` clips the content
