# Section Recipes

Most page sections don't need a dedicated component — they're combinations of the `Section` wrapper plus some structured content. This guide covers the patterns you'll reach for most often and when to use them.

## When to use a component vs. a recipe

| Situation | Approach |
|---|---|
| The pattern is identical across projects (hero, CTA, FAQ) | Use the component |
| The pattern is common but varies per project (stats, team, pricing) | Use a recipe — Section + inline HTML/CSS |
| The pattern is unique to this project | Build from Section + custom content |

All recipes assume you're working inside `<Section>` or `<Section theme="...">`. The Section component provides the container, heading block (`eyebrow`/`title`/`intro`), theme, and spacing. You just supply the content.

---

## Stats bar

Big numbers, small labels. Works on any theme. Use for credibility at the top of a services page or just after the intro on a homepage.

```astro
<Section theme="surface" eyebrow="By the numbers" title="Trusted by clients across Melbourne">
  <StatsBar
    items={[
      { value: '20+', label: 'Years experience' },
      { value: '500+', label: 'Projects delivered' },
      { value: '98%', label: 'Client satisfaction' },
      { value: '15', label: 'Team members' }
    ]}
  />
</Section>
```

**Variations:**
- Dark theme + full-width numbers for a high-impact trust section
- Fewer items (3) centred for a tighter feel
- Use as a standalone trust signal without the Section header block

---

## Two-column media

Image + text, flippable. Use for service detail pages, about page features, or anywhere you need to show and tell. Distinct from `Hero split-media` (which is hero-scale) and `Introduction` (which is editorial with no image).

```astro
<Section eyebrow="Our approach" title="Built off-site, installed in days">
  <TwoColumnMedia
    imageUrl="/uploads/workshop.jpg"
    imageAlt="Our workshop"
    title="Everything happens under one roof"
    body="We design, build, and finish every piece in our workshop before installation. No weather delays, no dust in your home, no subcontractors working to a different standard."
    ctaLabel="See how we work"
    ctaUrl="/blog/how-we-build/"
    mediaPosition="right"
  />
</Section>
```

**When to use:**
- **Service pages** — one per service, image on right for consistency
- **About page** — alternate left/right for rhythm
- **Homepage** — as a deeper dive after the intro, before the CTA

---

## Team grid

Headshots + names + roles. Use on about pages or as a trust section.

```astro
<Section eyebrow="Our team" title="The people behind the work">
  <div class="card-grid">
    <div class="card-grid__item">
      <img src="/uploads/team-1.jpg" alt="Name" loading="lazy" />
      <h3>Name Here</h3>
      <p>Role or short bio line</p>
    </div>
    <!-- repeat -->
  </div>
</Section>

<style>
  .card-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(14rem, 1fr));
    gap: var(--space-5);
  }

  .card-grid__item {
    text-align: center;
  }

  .card-grid__item img {
    width: 100%;
    aspect-ratio: 3 / 4;
    object-fit: cover;
    border-radius: var(--radius-sm);
    margin-bottom: var(--space-4);
  }

  .card-grid__item h3 {
    margin-bottom: var(--space-1);
  }

  .card-grid__item p {
    font-size: var(--text-sm);
    margin-bottom: 0;
  }
</style>
```

**Variations:**
- Replace images with initials in coloured circles for a lighter look
- Use `grid-template-columns: repeat(4, 1fr)` for exactly 4 team members
- Combine with `theme="surface"` for an alternate background

---

## Feature list with icons

Bulleted benefits with icons. Use on service pages or as a "why choose us" section.

```astro
<Section title="What's included">
  <ul class="feature-list">
    <li>
      <strong>Feature heading</strong>
      <p>One sentence explaining the benefit to the client. Keep it concrete.</p>
    </li>
    <li>
      <strong>Feature heading</strong>
      <p>One sentence explaining the benefit to the client. Keep it concrete.</p>
    </li>
    <!-- repeat -->
  </ul>
</Section>

<style>
  .feature-list {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(18rem, 1fr));
    gap: var(--space-5);
    list-style: none;
    padding: 0;
    margin: 0;
  }

  .feature-list li {
    padding-left: var(--space-7);
    position: relative;
  }

  .feature-list li::before {
    content: '';
    position: absolute;
    left: 0;
    top: 0.35em;
    width: 1.25rem;
    height: 1.25rem;
    border-radius: 50%;
    background: var(--color-section);
    border: 2px solid var(--brand-primary);
  }

  .feature-list strong {
    display: block;
    margin-bottom: var(--space-2);
    color: var(--color-heading);
  }

  .feature-list p {
    font-size: var(--text-sm);
    margin-bottom: 0;
  }
</style>
```

---

## Pricing comparison

Simple table or card grid. Use on service pages when the client wants transparent pricing tiers.

```astro
<Section eyebrow="Pricing" title="Straightforward, no surprises">
  <div class="pricing-grid">
    <div class="pricing-card">
      <h3>Package name</h3>
      <p class="pricing-card__price">From $X,XXX</p>
      <p>Short description of what's included and who it suits.</p>
      <a class="button button--secondary" href="/contact/">Enquire</a>
    </div>
    <!-- repeat for 2-3 tiers -->
  </div>
</Section>

<style>
  .pricing-grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(16rem, 1fr));
    gap: var(--space-5);
  }

  .pricing-card {
    padding: var(--space-6);
    border: 1px solid var(--color-rule);
    border-radius: var(--radius-md);
    display: flex;
    flex-direction: column;
    gap: var(--space-4);
  }

  .pricing-card__price {
    font-family: var(--font-condensed);
    font-size: var(--text-xl);
    font-weight: 700;
    color: var(--color-heading);
  }

  .pricing-card .button {
    margin-top: auto;
    align-self: flex-start;
  }
</style>
```

**When to use this vs. embedding a third-party tool:**
- Use this recipe for simple tiered pricing (2-3 options)
- Use the Embeds collection for interactive booking/quoting tools (Calendly, etc.)

---

## Timeline / process

Numbered steps. Use on about pages or as a "how it works" section. For a simple process (3-5 steps), this is lighter than importing ProcessStrip. For homepage, prefer the ProcessStrip component.

```astro
<Section eyebrow="How it works" title="From first call to final walk-through">
  <ol class="timeline">
    <li>
      <strong>Step one</strong>
      <p>What happens in this phase. One sentence.</p>
    </li>
    <!-- repeat -->
  </ol>
</Section>

<style>
  .timeline {
    display: grid;
    gap: var(--space-5);
    list-style: none;
    padding: 0;
    margin: 0;
    counter-reset: step;
  }

  .timeline li {
    display: grid;
    grid-template-columns: auto 1fr;
    gap: var(--space-4);
    counter-increment: step;
  }

  .timeline li::before {
    content: counter(step, decimal-leading-zero);
    font-family: var(--font-condensed);
    font-size: var(--text-lg);
    font-weight: 700;
    color: var(--color-heading);
    opacity: 0.35;
    min-width: 2.5rem;
  }

  .timeline strong {
    color: var(--color-heading);
  }

  .timeline p {
    font-size: var(--text-sm);
    margin: var(--space-1) 0 0;
  }
</style>
```

---

## Logo wall / clients

A strip of client logos or brands. Use on about pages, service pages, or as a trust section. The LogoStrip component handles simple cases; this recipe gives you more control over layout and sizing.

```astro
<Section theme="surface" title="Brands we've worked with">
  <div class="logo-wall">
    <img src="/uploads/logo-1.svg" alt="Company name" loading="lazy" />
    <img src="/uploads/logo-2.svg" alt="Company name" loading="lazy" />
    <!-- repeat -->
  </div>
</Section>

<style>
  .logo-wall {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    gap: var(--space-7) var(--space-8);
  }

  .logo-wall img {
    height: 2.25rem;
    width: auto;
    opacity: 0.55;
    filter: grayscale(1);
    transition: opacity 0.2s;
  }

  .logo-wall img:hover {
    opacity: 0.85;
  }
</style>
```

---

## Contact split

Map embed + contact details side by side. Use on the contact page.

```astro
<Section title="Find us">
  <div class="contact-split">
    <div class="contact-split__map">
      <!-- Paste Google Maps iframe embed -->
      <iframe src="https://www.google.com/maps/embed?..." ... />
    </div>
    <div class="contact-split__details">
      <ContactBlock email={site.data.email} phone={site.data.phone} address={site.data.address} />
    </div>
  </div>
</Section>

<style>
  .contact-split {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: var(--space-7);
    align-items: start;
  }

  .contact-split__map iframe {
    width: 100%;
    aspect-ratio: 4 / 3;
    border: none;
    border-radius: var(--radius-md);
  }

  @media (max-width: 720px) {
    .contact-split {
      grid-template-columns: 1fr;
    }
  }
</style>
```

---

## Section rhythm — composing a page

How sections flow together matters as much as the individual components. A good page rhythm:

```
┌──────────────────────────────────────┐
│ Hero (image-full)         ← dark     │  Big statement, full bleed
├──────────────────────────────────────┤
│ Introduction              ← default  │  Who you are, two-column editorial
├──────────────────────────────────────┤
│ SelectedWork              ← default  │  Photography carries the proof
├──────────────────────────────────────┤
│ Section + ServiceGrid     ← surface  │  What you do, alternate background
├──────────────────────────────────────┤
│ Section + StatsBar        ← dark     │  Credibility, contrast break
├──────────────────────────────────────┤
│ Section + Testimonials    ← default  │  Social proof
├──────────────────────────────────────┤
│ CTABand (band)            ← default  │  Conversion moment
├──────────────────────────────────────┤
│ Footer                    ← dark     │  Structural close
└──────────────────────────────────────┘
```

**The rhythm rule:** alternate default ↔ surface for adjacent light sections. Use dark or primary as deliberate contrast breaks — never two dark sections in a row, and never more than two default sections in a row without a surface or dark break.

For a shorter page (landing page, campaign):
```
Hero (image-full) → Section + ServiceGrid (surface) → Section + Testimonial (default) → CTABand → Footer
```

For a detail page (service, about):
```
PageIntro → Section + content (default) → Section + TwoColumnMedia (surface) → Section + StatsBar (dark) → CTABand → Footer
```
