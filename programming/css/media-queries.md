# Media Queries

## Description

Media queries apply CSS based on device characteristics — viewport width, resolution, color scheme, motion preferences, and more. They are the foundation of responsive design.

## Prerequisites

- [Values & Units](values-and-units.md) — viewport units and responsive sizing
- [Flexbox](flexbox.md) — responsive layout patterns

## Table of Contents

- [Media Query Syntax](#media-query-syntax)
- [Media Types](#media-types)
- [Media Features](#media-features)
- [Logical Operators](#logical-operators)
- [Viewport Breakpoints](#viewport-breakpoints)
- [Resolution Queries](#resolution-queries)
- [Color Scheme Queries](#color-scheme-queries)
- [User Preference Queries](#user-preference-queries)
- [Interaction Queries](#interaction-queries)
- [Scripting Queries](#scripting-queries)
- [Range Syntax](#range-syntax)
- [Using @media in HTML and CSS](#using-media-in-html-and-css)
- [Mobile-First vs Desktop-First](#mobile-first-vs-desktop-first)
- [Media Query Best Practices](#media-query-best-practices)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Media Query Syntax

A media query consists of an optional media type and one or more media features:

```css
/* @media [type] and ([feature]: [value]) */
@media screen and (min-width: 768px) {
    .sidebar {
        display: block;
    }
}
```

Multiple features:

```css
@media screen and (min-width: 768px) and (max-width: 1200px) {
    .layout {
        grid-template-columns: 1fr 1fr;
    }
}
```

### Media Types

```css
@media screen { }          /* Computer screens, tablets, phones */
@media print { }           /* Print preview and printed pages */
@media all { }             /* All devices (default) */
@media speech { }          /* Screen readers */
```

`all` is the default and can be omitted. Most responsive queries use `screen` or omit the type.

### Media Features

**Width-based:**

```css
@media (min-width: 768px) { }    /* Width ≥ 768px */
@media (max-width: 767px) { }    /* Width ≤ 767px */
@media (width: 1024px) { }       /* Exact width (rare) */
```

**Height-based:**

```css
@media (min-height: 600px) { }
@media (max-height: 800px) { }
```

**Aspect ratio:**

```css
@media (aspect-ratio: 16/9) { }
@media (min-aspect-ratio: 4/3) { }
@media (max-aspect-ratio: 3/2) { }
```

**Orientation:**

```css
@media (orientation: portrait) { }     /* Height > width */
@media (orientation: landscape) { }    /* Width > height */
```

**Resolution:**

```css
@media (min-resolution: 2dppx) { }     /* Retina displays */
@media (min-resolution: 96dpi) { }
@media (min-resolution: 150dpcm) { }
```

**Color scheme:**

```css
@media (prefers-color-scheme: light) { }
@media (prefers-color-scheme: dark) { }
@media (prefers-color-scheme: no-preference) { }
```

**User preferences:**

```css
@media (prefers-reduced-motion: reduce) { }   /* Disable animations */
@media (prefers-reduced-motion: no-preference) { }
@media (prefers-reduced-transparency: reduce) { }
@media (prefers-contrast: more) { }
@media (prefers-contrast: less) { }
@media (prefers-contrast: no-preference) { }
@media (forced-colors: active) { }              /* Windows high contrast */
@media (inverted-colors: inverted) { }
```

**Interaction:**

```css
@media (pointer: fine) { }          /* Mouse/stylus — precise pointer */
@media (pointer: coarse) { }        /* Touch — imprecise pointer */
@media (pointer: none) { }          /* No pointing device */
@media (hover: hover) { }           /* Can hover (desktop) */
@media (hover: none) { }            /* Cannot hover (touch) */
@media (any-pointer: fine) { }      /* Any input has fine pointer */
@media (any-hover: hover) { }       /* Any input can hover */
```

**Display:**

```css
@media (display-mode: fullscreen) { }   /* Fullscreen API */
@media (display-mode: standalone) { }    /* PWA standalone */
@media (display-mode: minimal-ui) { }
@media (display-mode: browser) { }
```

**Scripting:**

```css
@media (scripting: enabled) { }     /* JavaScript enabled */
@media (scripting: none) { }        /* JavaScript disabled */
@media (scripting: initial-only) { } /* JS enabled during page load only */
```

**Viewport segments (foldable devices):**

```css
@media (horizontal-viewport-segments: 2) { }
@media (vertical-viewport-segments: 2) { }
```

### Logical Operators

**`and`** — all conditions must match:

```css
@media screen and (min-width: 768px) and (orientation: landscape) { }
```

**`or` (comma-separated)** — any condition matches:

```css
@media screen and (max-width: 600px), screen and (orientation: portrait) { }
```

**`not`** — negate the query:

```css
@media not screen { }                        /* Everything except screens */
@media not (hover: hover) { }                /* Devices that cannot hover */
```

**`only`** — prevent older browsers from applying the styles:

```css
@media only screen and (min-width: 768px) { }
```

Older browsers that do not understand media features apply the styles if `only` is omitted. `only` hides the rule from old browsers.

### Viewport Breakpoints

Common breakpoints (for reference — use content to determine your own):

```css
/* Mobile-first */
/* Base: < 640px — mobile */
@media (min-width: 640px) { }    /* sm — tablet */
@media (min-width: 768px) { }    /* md — small desktop */
@media (min-width: 1024px) { }   /* lg — desktop */
@media (min-width: 1280px) { }   /* xl — large desktop */
@media (min-width: 1536px) { }   /* 2xl — extra large */
```

**Do not target specific devices.** Use breakpoints based on your content:

```css
/* Break when the sidebar no longer fits */
@media (min-width: 800px) {
    .layout { display: grid; grid-template-columns: 250px 1fr; }
}

/* Break when the card grid looks cramped */
@media (max-width: 700px) {
    .card-grid { grid-template-columns: 1fr; }
}
```

### Resolution Queries

Target high-DPI ("Retina") displays:

```css
.logo {
    background-image: url('logo.png');
}

@media (min-resolution: 2dppx) {
    .logo {
        background-image: url('logo@2x.png');
        background-size: 200px 100px;    /* Half natural size */
    }
}
```

More complete approach with `image-set()`:

```css
.logo {
    background-image: image-set(
        url('logo.png') 1x,
        url('logo@2x.png') 2x
    );
}
```

### Color Scheme Queries

Adapt to user preference:

```css
:root {
    --bg: white;
    --text: #1a1a1a;
}

@media (prefers-color-scheme: dark) {
    :root {
        --bg: #1a1a1a;
        --text: #e0e0e0;
    }
}

body {
    background: var(--bg);
    color: var(--text);
}
```

Or use `light-dark()` (CSS Color Level 5):

```css
body {
    color: light-dark(#1a1a1a, #e0e0e0);
    background: light-dark(white, #1a1a1a);
}

/* Requires color-scheme on :root */
:root {
    color-scheme: light dark;
}
```

### User Preference Queries

**Reduced motion:**

```css
/* Disable animations for vestibular disorders */
@media (prefers-reduced-motion: reduce) {
    *, *::before, *::after {
        animation-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.01ms !important;
    }
}
```

**Reduced transparency:**

```css
@media (prefers-reduced-transparency: reduce) {
    .glass-panel {
        backdrop-filter: none;          /* Remove frosted glass */
        background: rgba(255, 255, 255, 0.95);
    }
}
```

**Increased contrast:**

```css
@media (prefers-contrast: more) {
    .card {
        border-width: 2px;             /* Thicker borders */
    }
    .text-muted {
        color: #555;                    /* Higher contrast gray */
    }
}
```

### Interaction Queries

Adapt interfaces for touch vs mouse:

```css
/* Increase touch targets on mobile */
@media (pointer: coarse) {
    button, a, .clickable {
        min-height: 44px;               /* Apple recommendation */
        min-width: 44px;
    }
}

/* Increase spacing on touch devices */
@media (hover: none) {
    .nav-item {
        padding: 0.75rem 1rem;          /* Larger tap targets */
    }
}

/* Show hover-only styles only on devices that can hover */
@media (hover: hover) {
    .card:hover {
        transform: translateY(-2px);
        box-shadow: 0 4px 12px rgba(0,0,0,0.15);
    }
}
```

### Range Syntax

Modern CSS supports range syntax for readability (2023+):

```css
/* Old way */
@media (min-width: 768px) and (max-width: 1200px) { }

/* New range syntax */
@media (768px <= width <= 1200px) { }

/* Other ranges */
@media (width >= 768px) { }            /* min-width: 768px */
@media (width < 1024px) { }            /* max-width: 1023.99px */
@media (600px < width < 1200px) { }   /* Exclusive bounds */
```

The range syntax works in all modern browsers (Chrome 104+, Firefox 102+, Safari 16+).

### Using @media in HTML and CSS

**In CSS:**

```css
/* In stylesheet or <style> block */
@media (min-width: 768px) {
    .grid { grid-template-columns: 1fr 1fr; }
}
```

**In HTML `<link>`:**

```html
<link rel="stylesheet" href="mobile.css" media="(max-width: 767px)">
<link rel="stylesheet" href="desktop.css" media="(min-width: 768px)">
```

The browser downloads both files regardless but applies only the matching one.

**In `<source>` for `<picture>`:**

```html
<picture>
    <source srcset="wide.webp" media="(min-width: 768px)">
    <source srcset="narrow.webp" media="(max-width: 767px)">
    <img src="fallback.jpg" alt="">
</picture>
```

### Mobile-First vs Desktop-First

**Mobile-first** (base styles are mobile, add complexity with `min-width`):

```css
/* Base: mobile */
.sidebar { display: none; }
.content { width: 100%; }

/* Tablet */
@media (min-width: 768px) {
    .sidebar { display: block; width: 250px; }
    .layout { display: flex; }
}

/* Desktop */
@media (min-width: 1024px) {
    .layout { max-width: 1200px; margin: 0 auto; }
}
```

**Desktop-first** (base styles are desktop, simplify with `max-width`):

```css
/* Base: desktop */
.layout { display: grid; grid-template-columns: 250px 1fr; }
.sidebar { display: block; }

/* Tablet */
@media (max-width: 1023px) {
    .layout { grid-template-columns: 200px 1fr; }
}

/* Mobile */
@media (max-width: 767px) {
    .layout { grid-template-columns: 1fr; }
    .sidebar { display: none; }
}
```

**Mobile-first is recommended** — it is simpler to add than remove, and base styles are the minimal version.

### Media Query Best Practices

1. **Use logical breakpoints**, not device-specific ones. Let your content determine when it needs a breakpoint.

2. **Group media queries by component** rather than at the end of the file:

```css
/* Component-based organization */
.card { ... }
@media (min-width: 768px) { .card { ... } }

.sidebar { ... }
@media (min-width: 768px) { .sidebar { ... } }
```

3. **Do not duplicate selectors** in every media query — only override what changes.

4. **Use custom properties inside media queries** for centralized breakpoint values:

```css
:root {
    --columns: 1;
}

@media (min-width: 768px) {
    :root { --columns: 2; }
}

@media (min-width: 1024px) {
    :root { --columns: 3; }
}

.grid {
    grid-template-columns: repeat(var(--columns), 1fr);
}
```

5. **Test at actual breakpoints.** Do not rely on device emulators alone.

## Glossary

| Term | Definition |
|---|---|
| Media query | CSS feature detecting device characteristics |
| Breakpoint | Threshold where layout changes |
| dppx | Dots per pixel (resolution unit) |
| prefers-color-scheme | User preference for light/dark |
| prefers-reduced-motion | User preference to minimize animation |
| Mobile-first | Base styles for mobile, add with min-width |
| Range syntax | Modern `min-width <= width <= max-width` |

## Quick References

- [MDN: Media Queries](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_media_queries) — complete reference
- [MDN: prefers-color-scheme](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-color-scheme) — dark mode
- [MDN: prefers-reduced-motion](https://developer.mozilla.org/en-US/docs/Web/CSS/@media/prefers-reduced-motion) — motion preferences
- [CSS Media Queries Level 5](https://www.w3.org/TR/mediaqueries-5/) — specification
- [Responsive Design Patterns](https://responsivepattern.design/) — collection of patterns

## Next Steps

- [Container Queries](container-queries.md) — element-based responsive design