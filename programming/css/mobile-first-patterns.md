# Mobile-First Design Patterns

## Description

Mobile-first is a design strategy where you build the smallest-screen experience first, then progressively enhance for larger screens. It ensures every user gets a functional baseline.

## Prerequisites

- [Media Queries](media-queries.md) — `min-width` breakpoints
- [Flexbox](flexbox.md) and [Grid](grid.md) — responsive layout tools

## Table of Contents

- [The Mobile-First Mindset](#the-mobile-first-mindset)
- [Content Prioritization](#content-prioritization)
- [Touch Targets](#touch-targets)
- [Responsive Navigation Patterns](#responsive-navigation-patterns)
- [Responsive Tables](#responsive-tables)
- [Responsive Images](#responsive-images)
- [Progressive Enhancement](#progressive-enhancement)
- [Performance on Mobile](#performance-on-mobile)
- [Testing Mobile First](#testing-mobile-first)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Mobile-First Mindset

Mobile-first means:
- **Content first** — decide what matters most, ensure it works in a narrow viewport
- **Performance first** — network is slow, CPU is limited
- **Touch first** — finger targets are larger than mouse targets
- **Then enhance** — add complexity for larger screens

```css
/* Mobile-first — base is the phone layout */
.layout {
    display: flex;
    flex-direction: column;
    gap: 1rem;
}

/* Tablet — add two columns */
@media (min-width: 768px) {
    .layout {
        flex-direction: row;
        flex-wrap: wrap;
    }
    .layout > * {
        flex: 1 1 45%;
    }
}

/* Desktop — three columns */
@media (min-width: 1024px) {
    .layout > * {
        flex: 1 1 30%;
    }
}
```

Desktop-first would have started with `flex-direction: row` and used `max-width` to simplify — more code, harder to maintain.

### Content Prioritization

Decide what to show at each breakpoint:

```html
<article>
    <h1>Article Title</h1>
    <p class="meta">By Author · 5 min read</p>
    <div class="hero-image">
        <img src="hero.jpg" alt="">
    </div>
    <p class="abstract">Key summary...</p>
    <!-- desktop only -->
    <aside class="sidebar-module">
        <h2>Related Articles</h2>
        <!-- ... -->
    </aside>
    <div class="content">
        <!-- Main article text -->
    </div>
</article>
```

```css
/* Mobile: show only essentials */
.sidebar-module {
    display: none;                  /* Too much clutter for mobile */
}

.hero-image {
    margin: 0 -1rem;               /* Full-bleed image */
    width: calc(100% + 2rem);
}

/* Desktop: add sidebar */
@media (min-width: 1024px) {
    article {
        display: grid;
        grid-template-columns: 1fr 300px;
        grid-template-areas:
            "title   sidebar"
            "meta    sidebar"
            "hero    sidebar"
            "abstract sidebar"
            "content sidebar";
    }
    .sidebar-module {
        display: block;
        grid-area: sidebar;
    }
}
```

### Touch Targets

Minimum touch target size: 44×44 pixels (Apple HIG, WCAG):

```css
/* Mobile-first: generous sizing */
button, a, .interactive {
    min-height: 44px;
    min-width: 44px;
    padding: 0.75rem 1rem;          /* Comfortable tap area */
}

/* Fine pointer: can be smaller */
@media (pointer: fine) {
    button, .interactive {
        min-height: auto;
        padding: 0.5rem 0.75rem;
    }
}
```

Spacing between touch targets:

```css
.nav-list {
    display: flex;
    gap: 0.5rem;                    /* Minimum gap for mobile */
}

@media (pointer: fine) {
    .nav-list {
        gap: 0.25rem;               /* Closer spacing on desktop */
    }
}
```

### Responsive Navigation Patterns

**Hamburger menu (mobile-first):**

```css
/* Mobile: hamburger */
.nav-list {
    display: none;
}

.nav-toggle {
    display: block;
    background: none;
    border: none;
    padding: 0.5rem;
    cursor: pointer;
}

.nav-list.is-open {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
}

/* Desktop: horizontal nav */
@media (min-width: 768px) {
    .nav-toggle {
        display: none;
    }
    .nav-list {
        display: flex;
        flex-direction: row;
        gap: 1.5rem;
    }
}
```

**Priority+ (show-more nav):**

```css
.nav {
    display: flex;
    overflow: hidden;
}

.nav-item {
    flex-shrink: 0;
}

.nav-more {
    margin-left: auto;              /* Pushed to the right */
    display: none;                  /* Hidden until needed */
}
```

### Responsive Tables

**Horizontal scroll:**

```css
.table-wrapper {
    overflow-x: auto;
    -webkit-overflow-scrolling: touch;
}

.data-table {
    min-width: 600px;               /* Forces scroll on mobile */
}
```

**Cards (convert rows to cards on mobile):**

```css
.data-table {
    width: 100%;
    border-collapse: collapse;
}

@media (max-width: 600px) {
    .data-table thead {
        display: none;              /* Hide header */
    }
    .data-table tr {
        display: block;
        margin-bottom: 1rem;
        border: 1px solid #ddd;
        border-radius: 8px;
        padding: 0.5rem;
    }
    .data-table td {
        display: flex;
        justify-content: space-between;
        padding: 0.25rem 0;
        border: none;
    }
    .data-table td::before {
        content: attr(data-label);
        font-weight: 600;
    }
}
```

### Responsive Images

**`<picture>` element:**

```html
<picture>
    <source srcset="hero-large.webp" media="(min-width: 1024px)">
    <source srcset="hero-medium.webp" media="(min-width: 768px)">
    <img src="hero-small.webp" alt="" loading="lazy" decoding="async">
</picture>
```

**`srcset` with sizes:**

```html
<img src="photo-400.webp"
     srcset="photo-400.webp 400w, photo-800.webp 800w, photo-1200.webp 1200w"
     sizes="(max-width: 600px) 100vw, (max-width: 1024px) 50vw, 33vw"
     alt="">
```

**CSS `image-set()` for background images:**

```css
.hero {
    background-image: image-set(
        url('hero-400.webp') 1x,
        url('hero-800.webp') 2x
    );
}

@media (min-width: 768px) {
    .hero {
        background-image: image-set(
            url('hero-1200.webp') 1x,
            url('hero-2400.webp') 2x
        );
    }
}
```

### Progressive Enhancement

Build the core experience first, then layer enhancements:

```css
/* Core: works everywhere */
.button {
    padding: 0.75rem 1.5rem;
    background: #0066cc;
    color: white;
    border: none;
    border-radius: 6px;
}

/* Enhancement: hover */
@media (hover: hover) {
    .button:hover {
        background: #0052a3;
        cursor: pointer;
    }
}

/* Enhancement: transition */
@media (prefers-reduced-motion: no-preference) {
    .button {
        transition: background 0.2s;
    }
}

/* Enhancement: backdrop-filter for glass effect */
@supports (backdrop-filter: blur(4px)) {
    .glass {
        background: rgba(255, 255, 255, 0.6);
        backdrop-filter: blur(8px);
    }
}
```

### Performance on Mobile

Mobile devices have slower CPUs, less memory, and slower networks.

**CSS performance tips for mobile:**
- Avoid expensive animations (blur, large transforms)
- Use `will-change` sparingly
- Minimize selector complexity
- Reduce unused CSS
- Use `content-visibility: auto` for off-screen sections

```css
.section {
    content-visibility: auto;   /* Delays rendering of off-screen sections */
    contain-intrinsic-size: 500px;  /* Placeholder size */
}
```

**Font loading:**
- Limit to 2–3 font weights
- Use `font-display: swap` or `optional`
- Subset fonts

### Testing Mobile First

1. Open the site on a phone-sized viewport (320–375px).
2. Ensure all content is accessible and usable.
3. Resize larger — add media queries when content breaks.
4. Test with touch emulation.
5. Test with throttled network (Slow 3G).

Common mobile-first breakpoints (content-driven):

```css
/* Base: 0–639px — mobile */
@media (min-width: 640px) { }  /* sm — large phone / tablet */
@media (min-width: 768px) { }  /* md — tablet landscape / small desktop */
@media (min-width: 1024px) { } /* lg — desktop */
@media (min-width: 1280px) { } /* xl — wide desktop */
```

### Container Queries for Mobile-First Components

Container queries (`@container`) complement media queries by allowing components to respond to their parent container's size rather than the viewport:

```css
/* Define a containment context */
.card-grid {
    container-type: inline-size;
    container-name: cards;
}

/* Mobile-first card: stacked by default */
.card {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
}

/* When container is wide enough, switch to row layout */
@container cards (min-width: 400px) {
    .card {
        flex-direction: row;
    }
    .card img {
        width: 200px;
        flex-shrink: 0;
    }
}

/* Even wider: add metadata sidebar */
@container cards (min-width: 700px) {
    .card {
        display: grid;
        grid-template-columns: auto 1fr 200px;
    }
}
```

Container queries are especially valuable for mobile-first design because they make components truly reusable. A card component can be used in a narrow sidebar, a medium-width content area, or a wide hero section — it adapts to each context independently. This eliminates the need for fragile width-based media queries tied to specific viewport sizes.

### Conditional CSS with prefers-* Media Features

Mobile-first design must respect user preferences and device capabilities:

```css
/* Base: works everywhere */
.button {
    padding: 0.75rem 1.5rem;
    font-size: 1rem;
}

/* Reduce motion for vestibular disorders */
@media (prefers-reduced-motion: reduce) {
    .button {
        transition: none;
        animation: none;
    }
}

/* High contrast mode */
@media (prefers-contrast: high) {
    .button {
        border: 2px solid currentColor;
        background: transparent;
    }
}

/* Dark mode */
@media (prefers-color-scheme: dark) {
    .button {
        background: #3399ff;
        color: #000;
    }
}

/* Reduced data — serve smaller assets */
@media (prefers-reduced-data: reduce) {
    .hero {
        background-image: none;
    }
    .hero img {
        display: none;
    }
}
```

These conditional styles ensure that the mobile-first baseline works for everyone, while enhancements tailor the experience to individual needs. Mobile-first is not just about screen size — it is about designing for a diverse range of users and contexts.

## Glossary

| Term | Definition |
|---|---|
| Mobile-first | Design starting from smallest screen, adding complexity upward |
| Progressive enhancement | Building core experience first, layering enhancements |
| Touch target | Interactive area a finger can tap |
| Hamburger menu | Three-line icon revealing navigation |
| content-visibility | CSS property deferring off-screen rendering |
| responsive image | Image that serves different resolutions per viewport |

## Quick References

- [MDN: Responsive Design](https://developer.mozilla.org/en-US/docs/Learn/CSS/CSS_layout/Responsive_Design) — guide
- [MDN: Responsive Images](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images) — image guide
- [Mobile First](https://www.lukew.com/ff/entry.asp?933) — Luke Wroblewski's original article
- [WCAG Touch Target](https://www.w3.org/WAI/WCAG21/Understanding/target-size.html) — accessibility guidelines

## Next Steps

- [Pseudo-Classes](pseudo-classes.md) — interactive and structural selectors