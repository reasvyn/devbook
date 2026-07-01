# CSS Tooling & Performance

## Description

CSS tooling — linters, autoprefixers, minifiers, and build tools — improves code quality and performance. Understanding how CSS affects rendering performance helps you build fast, smooth pages.

## Prerequisites

- [BEM & CSS Architecture](bem-and-architecture.md) — scalable CSS organization
- [Transitions & Animations](transitions-and-animations.md) — animation performance

## Table of Contents

- [Linting with Stylelint](#linting-with-stylelint)
- [Autoprefixing](#autoprefixing)
- [PostCSS](#postcss)
- [CSS Minification](#css-minification)
- [Critical CSS](#critical-css)
- [CSS Bundle Size](#css-bundle-size)
- [CSS Performance Metrics](#css-performance-metrics)
- [Layout Thrashing](#layout-thrashing)
- [Selector Performance](#selector-performance)
- [Paint, Layout, Composite](#paint-layout-composite)
- [Content-Visibility](#content-visibility)
- [Containment](#containment)
- [Debouncing with will-change](#debouncing-with-will-change)
- [CSS Reset vs Normalize](#css-reset-vs-normalize)
- [Debugging CSS](#debugging-css)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Linting with Stylelint

Stylelint catches errors, enforces conventions, and prevents problematic patterns:

```json
{
    "extends": "stylelint-config-standard",
    "rules": {
        "color-hex-length": "long",
        "declaration-no-important": true,
        "selector-class-pattern": "^[a-z][a-zA-Z0-9]+$",
        "max-nesting-depth": 3,
        "unit-allowed-list": ["px", "rem", "em", "%", "vw", "vh", "deg", "s"]
    }
}
```

Common Stylelint rules:

```css
/* Before linting */
.card {
    color: #FFF;            /* ❌ Hex should be lowercase */
    font-size: 16px;        /* ❌ Use rem instead of px */
    background: red;        /* ❌ Named colors are not allowed */
}

/* After linting */
.card {
    color: #ffffff;
    font-size: 1rem;
    background: #ff0000;
}
```

Usage:

```bash
npx stylelint "**/*.css"
# or integrate with build: "lint:css": "stylelint \"**/*.css\""
```

**Prettier for CSS formatting:**

```bash
npm install --save-dev prettier
```

```json
// .prettierrc
{
    "printWidth": 100,
    "tabWidth": 2,
    "singleQuote": true
}
```

### Autoprefixing

Autoprefixer adds vendor prefixes based on browser targets:

```css
/* Input */
.grid {
    display: grid;
}

/* Output (with default targets) */
.grid {
    display: -ms-grid;
    display: grid;
}
```

Configure via `browserslist` in `package.json`:

```json
{
    "browserslist": [
        "last 2 versions",
        "> 1%",
        "not dead"
    ]
}
```

Or in `.browserslistrc`:

```
last 2 versions
> 1%
not dead
```

Modern CSS properties (like `display: grid`, `display: flex`, `gap`) rarely need prefixes in 2024+. Autoprefixer is most useful for older browser support.

### PostCSS

PostCSS transforms CSS via plugins in a build pipeline:

```javascript
// postcss.config.js
module.exports = {
    plugins: [
        require('autoprefixer'),
        require('cssnano'),                // Minification
        require('postcss-preset-env'),      // Future CSS features
        require('postcss-import'),          // CSS @import inlining
    ],
};
```

Common PostCSS plugins:
- `autoprefixer` — vendor prefixes
- `cssnano` — minification
- `postcss-preset-env` — future CSS (stages 0–4)
- `postcss-import` — inline `@import` statements
- `postcss-nesting` — native nesting support (even for browsers without it)
- `postcss-custom-properties` — custom properties fallbacks for IE

### CSS Minification

cssnano (PostCSS plugin) or other minifiers:

```css
/* Before */
.card {
    padding: 1rem;
    margin: 1rem 0;
    color: #333333;
}

/* After minification */
.card{padding:1rem;margin:1rem 0;color:#333}
```

Typical savings: 15–30% size reduction. Combine with Gzip/Brotli on the server for maximum compression.

### Critical CSS

Critical CSS inlines above-the-fold styles in the `<head>`:

```html
<head>
    <style>
        /* Critical CSS — everything needed for above-the-fold content */
        body { margin: 0; font-family: 'Inter', sans-serif; }
        .header { position: fixed; top: 0; width: 100%; background: white; }
        .hero { min-height: 60vh; display: flex; align-items: center; }
        /* ... */
    </style>
    <link rel="stylesheet" href="/styles.css" media="print" onload="this.media='all'">
</head>
```

The full stylesheet is loaded asynchronously (`media="print"` trick).

Tools to generate critical CSS:
- **Critical** (Node.js) — `npx critical index.html > critical.css`
- **Penthouse** — headless Chromium-based extraction
- **Webpack plugin** — `html-critical-webpack-plugin`

### CSS Bundle Size

Monitor and optimize CSS bundle size:

```bash
# Analyze CSS with Source Map
npx source-map-explorer dist/*.css

# Find duplicate styles
npx stylelint --config-basedir . --syntax css "**/*.css"

# Check unused CSS
npx uncss index.html > styles.clean.css
```

**Strategies to reduce CSS size:**

1. **Remove dead code** — use PurgeCSS/UnCSS with your HTML or JSX templates:

```javascript
// Tailwind CSS purge (built-in)
module.exports = {
    purge: ['./src/**/*.{html,js,jsx}'],
    // ...
};
```

2. **Code-split CSS** — load CSS per route/page:

```javascript
// In Next.js or similar frameworks
// CSS is automatically code-split per component
```

3. **Avoid large frameworks** — use modern CSS instead of Bootstrap for simple needs.

4. **Limit fonts** — each `@font-face` is a download.

### CSS Performance Metrics

Key metrics affected by CSS:

| Metric | CSS Impact |
|---|---|
| First Contentful Paint (FCP) | CSS blocks rendering — critical CSS reduces this |
| Largest Contentful Paint (LCP) | Font loading, image loading, layout shifts |
| Cumulative Layout Shift (CLS) | Font swap, images without dimensions, late-loading CSS |
| First Input Delay (FID) | Not directly affected by CSS |
| Interaction to Next Paint (INP) | Animation performance, layout thrashing |

### Layout Thrashing

Layout thrashing happens when JavaScript forces repeated layout calculations:

```javascript
// ❌ Bad — causes multiple layout recalculations
const elements = document.querySelectorAll('.card');
elements.forEach(el => {
    el.style.width = '50%';               // Triggers layout
    console.log(el.offsetHeight);         // Triggers layout again
    el.style.marginLeft = '10px';         // Triggers layout
});

// ✅ Good — batch reads and writes
const elements = document.querySelectorAll('.card');
const heights = [];
elements.forEach(el => {
    heights.push(el.offsetHeight);        // Read all first
});
elements.forEach((el, i) => {
    el.style.width = '50%';              // Then write all
    el.style.marginLeft = '10px';
});
```

From CSS side, avoid properties that trigger layout:

| Triggers layout | Triggers paint only | Compositor only |
|---|---|---|
| `width`, `height` | `color` | `transform` |
| `top`, `left`, `right`, `bottom` | `background-color` | `opacity` |
| `margin`, `padding` | `border-color` | `filter` (some cases) |
| `display`, `position` | `outline-color` | `will-change` |
| `float`, `clear` | `box-shadow` | |
| `font-size`, `line-height` | `background` | |

### Selector Performance

Selector matching is fast — the browser reads selectors right-to-left:

```css
/* The key selector is the rightmost part */
.content p { }           /* Key: p — fast */
div p { }                /* Key: p — fast */
* .highlight { }         /* Key: .highlight — universal key is slow */
ul li a span { }         /* Key: span — 4 levels, more work */
```

In practice, selector performance only matters with 10,000+ elements. For typical pages, write for readability.

### Paint, Layout, Composite

The browser rendering pipeline:

1. **JavaScript** → DOM/CSSOM changes
2. **Style** → matching selectors, computing values
3. **Layout** → calculating geometry (boxes, positions)
4. **Paint** → filling pixels (colors, shadows, images)
5. **Composite** → layering (GPU compositing)

```css
/* Layout → Paint → Composite */
.element {
    width: 200px;           /* Layout */
    background: blue;       /* Paint */
}

/* Composite only (best performance) */
.element {
    transform: scale(1.2);  /* Composite */
    opacity: 0.5;           /* Composite */
}
```

Using `transform` and `opacity` for animations keeps changes in the compositor, skipping Layout and Paint.

### Content-Visibility

Delay rendering of off-screen elements:

```css
.section {
    content-visibility: auto;
    contain-intrinsic-size: 500px;       /* Placeholder size during load */
}
```

- `content-visibility: auto` — skips rendering for off-screen sections
- `contain-intrinsic-size` — provides a placeholder size so scrollbar does not jump

This can significantly improve FCP and LCP for long pages.

### Containment

CSS containment isolates subtrees for rendering optimization:

```css
.element {
    contain: none;           /* No containment */
    contain: layout;         /* Layout inside does not affect outside */
    contain: paint;          /* Paint clipped to element's box */
    contain: size;           /* Size must be specified explicitly */
    contain: style;          /* Style changes do not affect outside */
    contain: content;        /* layout + paint + style */
    contain: strict;         /* layout + paint + style + size */
}
```

Consider using `contain: content` on widgets and components to improve rendering performance.

### CSS Reset vs Normalize

**CSS Reset** removes all browser defaults:

```css
/* Example: reset */
*, *::before, *::after {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

h1, h2, h3, h4, h5, h6, p {
    font-size: inherit;
    font-weight: inherit;
}
```

**Normalize.css** preserves useful defaults while fixing inconsistencies:

```css
/* Normalize keeps h1 font-size, heading margins */
/* but normalizes form element inconsistencies across browsers */
```

**Modern CSS reset (by Andy Bell / Josh Comeau):**

```css
*,
*::before,
*::after {
    box-sizing: border-box;
    margin: 0;
    padding: 0;
}

html:focus-within {
    scroll-behavior: smooth;
}

body {
    min-height: 100vh;
    text-rendering: optimizeSpeed;
    line-height: 1.5;
    font-family: system-ui, -apple-system, sans-serif;
}

img, picture, video, canvas, svg {
    display: block;
    max-width: 100%;
}

input, button, textarea, select {
    font: inherit;
}
```

### Debugging CSS

**Browser DevTools:**

- **Computed panel** — see which rules apply and which are overridden
- **Styles panel** — toggle rules, add inline styles, force states
- **Layout panel** — box model visualization, grid/flex overlays
- **Coverage tab** — find unused CSS
- **Rendering tab** — paint flashing (identify over-painting)

**CSS debugging techniques:**

```css
/* Highlight all elements to visualize boxes */
* {
    outline: 1px solid red !important;
}

/* Debug specific issues */
.problematic-element {
    outline: 2px dashed blue !important;
    background: rgba(0, 0, 255, 0.1) !important;
}
```

**Checking for specificity issues:**

```css
/* Which rule wins? Compare selectors */
#sidebar .widget a { }      /* 0,1,0,2 */
.widget__link { }           /* 0,0,1,0 */
```

**Performance profiling:** Use the Performance tab in DevTools to record interactions and identify forced layouts and long paint times.

## Glossary

| Term | Definition |
|---|---|
| Stylelint | CSS linter for error detection and convention enforcement |
| Autoprefixer | PostCSS plugin adding vendor prefixes |
| PostCSS | CSS transformation tool via plugin pipeline |
| cssnano | CSS minifier |
| Critical CSS | Above-the-fold styles inlined for fast FCP |
| Layout thrashing | Repeated forced layout calculations causing jank |
| Compositor | GPU layer rendering |
| content-visibility | CSS property deferring off-screen rendering |
| Containment | Rendering isolation of subtrees |
| PurgeCSS | Tool removing unused CSS selectors |
| FCP | First Contentful Paint — when first content renders |

## Quick References

- [Stylelint Documentation](https://stylelint.io/) — CSS linter
- [PostCSS Documentation](https://postcss.org/) — CSS transformer
- [CSS Tricks: Critical CSS](https://css-tricks.com/authoring-critical-css/) — critical CSS guide
- [Web.dev: CSS Performance](https://web.dev/learn/performance/) — performance guide
- [Can I Use](https://caniuse.com/) — browser support data
- [CSS Stats](https://cssstats.com/) — analyze stylesheet
- [PageSpeed Insights](https://pagespeed.web.dev/) — performance measurement

## Next Steps

- Review the [CSS Index](index.md) for all available topics