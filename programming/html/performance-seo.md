# Performance and SEO

## Description

HTML performance and search engine optimization (SEO) are tightly linked — fast-loading, well-structured pages rank higher and provide better user experiences. This document covers HTML-level techniques for optimizing rendering and search visibility.

## Prerequisites

- [Meta Tags & SEO](meta-tags.md) — meta elements and social graph
- [Document Structure](document-structure.md) — HTML document shape
- [Scripts, Defer & Async](scripts-and-defer.md) — script loading strategies

## Table of Contents

- [Core Web Vitals](#core-web-vitals)
- [HTML Rendering Pipeline](#html-rendering-pipeline)
- [Critical Rendering Path](#critical-rendering-path)
- [Link Priority Hints](#link-priority-hints)
- [Preconnect, Prefetch, and Preload](#preconnect-prefetch-and-preload)
- [Lazy Loading](#lazy-loading)
- [Responsive Images](#responsive-images)
- [Video Performance](#video-performance)
- [Font Loading](#font-loading)
- [Semantic HTML for SEO](#semantic-html-for-seo)
- [Structured Data](#structured-data)
- [Canonical URLs](#canonical-urls)
- [Pagination](#pagination)
- [Crawl Budget](#crawl-budget)
- [Page Speed Metrics](#page-speed-metrics)
- [Measuring Performance in HTML](#measuring-performance-in-html)
- [Performance Budgets](#performance-budgets)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Core Web Vitals

Google's Core Web Vitals are three metrics that measure user experience:

| Metric | Measures | Good | Poor | HTML impact |
|---|---|---|---|---|
| LCP | Largest Contentful Paint (loading) | ≤ 2.5s | > 4.0s | Image optimization, preloading, server timing |
| INP | Interaction to Next Paint (responsiveness) | ≤ 200ms | > 500ms | Minimal blocking JS, efficient event handlers |
| CLS | Cumulative Layout Shift (stability) | ≤ 0.1 | > 0.25 | Explicit dimensions, font-display, avoided late inserts |

**LCP optimization:**
- Preload the LCP image or resource
- Use responsive images with `srcset`
- Minimize render-blocking resources
- Enable compression (Brotli, Gzip)
- Use a CDN

```html
<link rel="preload" href="/hero.webp" as="image" fetchpriority="high">
```

**CLS optimization:**
- Set explicit `width` and `height` on images and videos
- Reserve space for ads and embeds
- Use `font-display: swap` or `optional` for web fonts
- Avoid inserting content above existing content (above the fold)

```html
<img src="hero.jpg" width="1200" height="600" alt="Hero image">
```

**INP optimization:**
- Break long tasks into smaller chunks
- Defer non-critical JavaScript
- Use `content-visibility` for off-screen content
- Minimize main-thread work during interaction

### HTML Rendering Pipeline

Understanding how the browser renders HTML helps identify performance bottlenecks:

```
HTML → DOM → Render Tree → Layout → Paint → Composite
```

**Parse HTML** — The browser parses HTML and builds the DOM. Blocking resources (synchronous scripts, CSS) pause parsing.

**Build render tree** — The DOM and CSSOM combine into the render tree, containing only visible elements.

**Layout** — The browser calculates the position and size of each element. This is one of the most expensive operations.

**Paint** — Pixels are drawn to the screen. Layers are painted independently.

**Composite** — Layers are combined in the correct order. Compositing is GPU-accelerated.

**Optimization principles:**
- Minimize HTML size (fewer bytes to parse)
- Stream the `<head>` first (include CSS and preloads before body)
- Defer non-critical CSS and JS
- Avoid deep DOM nesting (more than ~30 levels)

### Critical Rendering Path

The critical rendering path is the sequence of steps the browser must complete to render the page for the first time. Every byte of HTML, CSS, and JavaScript in the critical path delays rendering.

**Optimize the critical path:**

1. **Inline critical CSS** (styles needed for above-the-fold content):

```html
<style>
    /* Critical above-the-fold styles */
    header { display: flex; padding: 1rem; }
    .hero { font-size: 2rem; }
</style>
<link rel="stylesheet" href="/styles.css" media="print" onload="this.media='all'">
```

2. **Defer non-critical JavaScript:**

```html
<script src="/analytics.js" defer></script>
<script src="/app.js" type="module"></script>
```

3. **Preload critical resources:**

```html
<link rel="preload" href="/hero.webp" as="image">
<link rel="preload" href="/critical.css" as="style">
```

4. **Minimize render-blocking external resources** — only CSS and synchronous scripts in `<head>` block rendering.

### Link Priority Hints

The `fetchpriority` attribute hints to the browser about resource priority:

```html
<!-- High priority (LCP element) -->
<img src="hero.webp" fetchpriority="high" alt="Hero">

<!-- Low priority (below-the-fold) -->
<img src="footer-bg.webp" fetchpriority="low" alt="">

<!-- Default priority -->
<img src="gallery-1.webp" alt="Gallery image">
```

**`fetchpriority` values:**

| Value | Meaning | Use case |
|---|---|---|
| `high` | High priority | LCP image, critical CSS |
| `low` | Low priority | Below-the-fold images, analytics scripts |
| `auto` | Default (browser decides) | Most resources |

`fetchpriority` works on `<img>`, `<link>`, `<script>`, and `<iframe>` elements.

### Preconnect, Prefetch, and Preload

**`<link rel="preconnect">`** — establishes early connections to third-party origins:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://analytics.example.com">
```

Saves DNS lookup, TCP handshake, and TLS negotiation time. Use for critical third-party origins.

**`<link rel="dns-prefetch">** — resolves DNS early (lighter than preconnect):

```html
<link rel="dns-prefetch" href="https://cdn.example.com">
```

Use for non-critical origins where preconnect overhead is not justified.

**`<link rel="prefetch">** — fetches a resource for the next navigation:

```html
<link rel="prefetch" href="/next-page.html" as="document">
```

The browser fetches this during idle time, but may not use it. Only prefetch resources the user is likely to visit next.

**`<link rel="preload">** — fetches a resource for the current page urgently:

```html
<link rel="preload" href="/fonts/inter-var.woff2" as="font" crossorigin>
<link rel="preload" href="/hero.webp" as="image">
<link rel="preload" href="/critical.css" as="style">
```

Preload forces the browser to fetch the resource before it is discovered in the HTML. Use sparingly — only for resources discovered late in parsing.

**`<link rel="modulepreload">** — preloads ES modules:

```html
<link rel="modulepreload" href="/app.mjs">
```

Like `preload` but specifically for ES modules, fetching and compiling the module.

| Directive | Purpose | When to use |
|---|---|---|
| `preconnect` | Initiate early connection to origin | Third-party CDNs, font hosts |
| `dns-prefetch` | Resolve DNS early | Backup for preconnect |
| `prefetch` | Fetch for future navigation | Likely next page |
| `preload` | Fetch for current page | Critical late-discovered resources |
| `modulepreload` | Preload ES module | Critical JS modules |

### Lazy Loading

**Native lazy loading** defers loading off-screen images and iframes:

```html
<img src="gallery-photo.webp" loading="lazy" alt="Gallery photo">
<iframe src="widget.html" loading="lazy"></iframe>
```

**`loading` attribute values:**

| Value | Behavior |
|---|---|
| `lazy` | Defer loading until the element approaches the viewport |
| `eager` | Load immediately (default) |

**Distance threshold:** Browsers typically start loading lazy resources when they are approximately 1250px (Chrome) or 2500-3750px (Safari) from the viewport.

**Lazy loading best practices:**
- Always lazy-load below-the-fold images and iframes
- Do not lazy-load LCP candidates (above-the-fold images)
- Provide explicit dimensions to prevent layout shift:

```html
<img src="photo.webp" loading="lazy" width="800" height="600" alt="Photo">
```

**Lazy loading with Intersection Observer (for custom scenarios):**

```html
<img data-src="photo.webp" class="lazy" alt="Photo">
```

```javascript
const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            const img = entry.target;
            img.src = img.dataset.src;
            observer.unobserve(img);
        }
    });
});

document.querySelectorAll('img.lazy').forEach(img => observer.observe(img));
```

Native `loading="lazy"` is preferred over custom Intersection Observer implementations for most cases.

### Responsive Images

**`srcset` and `sizes`** serve appropriately sized images for the device:

```html
<img
    src="hero-1200.jpg"
    srcset="hero-600.jpg 600w, hero-900.jpg 900w, hero-1200.jpg 1200w, hero-1800.jpg 1800w"
    sizes="(max-width: 600px) 100vw, (max-width: 1200px) 80vw, 1200px"
    width="1200" height="600"
    alt="Hero image"
    fetchpriority="high"
>
```

**How it works:**
1. The browser checks `sizes` to determine the display width
2. It selects the smallest image from `srcset` that is >= the display width times the device pixel ratio
3. The selected image is downloaded

**`<picture>` for art direction (different crops per breakpoint):**

```html
<picture>
    <source srcset="hero-wide.webp" media="(min-width: 1200px)"
            type="image/webp" width="1200" height="400">
    <source srcset="hero.webp" media="(min-width: 600px)"
            type="image/webp" width="800" height="600">
    <source srcset="hero-narrow.webp" type="image/webp" width="400" height="400">
    <img src="hero.jpg" width="800" height="600" alt="Hero image"
         fetchpriority="high">
</picture>
```

**`<picture>` for image format fallbacks:**

```html
<picture>
    <source srcset="image.avif" type="image/avif">
    <source srcset="image.webp" type="image/webp">
    <img src="image.jpg" alt="Photo">
</picture>
```

The browser selects the first supported format. AVIF and WebP offer better compression than JPEG.

### Video Performance

**Use `<video>` instead of GIFs for animation — videos are much smaller:**

```html
<!-- Animated GIF: ~5MB -->
<img src="animation.gif" alt="Animation">

<!-- Same animation as video: ~200KB -->
<video autoplay loop muted playsinline width="400" height="300">
    <source src="animation.webm" type="video/webm">
    <source src="animation.mp4" type="video/mp4">
</video>
```

**Preload video attributes:**

```html
<video controls preload="metadata" poster="thumbnail.jpg">
    <source src="intro.mp4" type="video/mp4">
</video>
```

**`preload` values:**

| Value | Behavior |
|---|---|
| `none` | Do not preload any video data |
| `metadata` | Preload only metadata (duration, dimensions) — recommended |
| `auto` | Preload the entire video |

**Lazy loading videos:**

```html
<video controls preload="none" poster="thumbnail.jpg" width="640" height="360">
    <source data-src="intro.mp4" type="video/mp4">
</video>
```

```javascript
const observer = new IntersectionObserver((entries) => {
    entries.forEach(entry => {
        if (entry.isIntersecting) {
            const video = entry.target;
            video.querySelectorAll('source').forEach(s => {
                s.src = s.dataset.src;
            });
            video.load();
            observer.unobserve(video);
        }
    });
});
document.querySelectorAll('video[preload="none"]').forEach(v => observer.observe(v));
```

### Font Loading

**Fonts block rendering or cause layout shifts.** Optimize with `font-display`:

```css
@font-face {
    font-family: 'Inter';
    src: url('/fonts/inter-var.woff2') format('woff2');
    font-display: swap;  /* Show fallback font, swap when loaded */
}
```

**`font-display` values:**

| Value | Behavior | Use case |
|---|---|---|
| `auto` | Browser default (usually block) | Default |
| `block` | Invisible text for ~3s, then swap | Icon fonts |
| `swap` | Fallback text immediately, swap when loaded | Body text |
| `fallback` | Invisible text for ~100ms, fallback for ~3s, swap | Headings |
| `optional` | Invisible text for ~100ms, fallback if font not loaded | Decorative fonts |

**Preloading fonts:**

```html
<link rel="preload" href="/fonts/inter-var.woff2" as="font" crossorigin>
```

Always add `crossorigin` for font preloads (fonts are fetched with anonymous CORS mode).

**Font subsetting** — serve only the characters you need:

```css
/* Latin subset only — much smaller than full font */
@font-face {
    font-family: 'Inter';
    src: url('/fonts/inter-latin.woff2') format('woff2');
    unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}
```

### Semantic HTML for SEO

Search engines use semantic HTML to understand page structure and content.

**Heading hierarchy:**

```html
<!-- Good: clear hierarchy -->
<h1>Page Title</h1>
<h2>Section</h2>
<h3>Subsection</h3>

<!-- Bad: skipped levels or all h1s -->
<h1>Page Title</h1>
<h3>Subsection</h3>  <!-- Skipped h2 -->
```

Search engines use headings for content understanding and featured snippets. One `<h1>` per page, hierarchical heading levels.

**Landmark elements:**

```html
<header>, <nav>, <main>, <article>, <section>, <aside>, <footer>
```

These help search engines identify page regions. The `<main>` element marks the primary content — critical for Google's search results.

**`<article>` for self-contained content:**

```html
<article>
    <h2>Blog Post Title</h2>
    <p>Published: <time datetime="2026-06-28">June 28, 2026</time></p>
    <p>Content...</p>
</article>
```

Google uses `<article>` for rich results and the "Articles" carousel.

**Link text matters:**

```html
<!-- Bad: generic link text -->
<a href="/docs/html">Click here</a>

<!-- Good: descriptive link text -->
<a href="/docs/html">HTML documentation</a>
```

Descriptive link text improves both accessibility and SEO (Google uses link text as a relevance signal).

### Structured Data

Structured data helps search engines understand content and enables rich results:

```html
<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "Article",
    "headline": "HTML Performance Optimization",
    "description": "Techniques for optimizing HTML performance and SEO.",
    "author": {
        "@type": "Person",
        "name": "DevBook"
    },
    "datePublished": "2026-06-28",
    "image": "https://example.com/hero.jpg"
}
</script>
```

**Common structured data types for developers:**

| Type | Purpose | Fields |
|---|---|---|
| `Article` | Blog posts, news | `headline`, `author`, `datePublished` |
| `TechArticle` | Technical documentation | `proficiencyLevel`, `dependencies` |
| `BreadcrumbList` | Navigation path | `itemListElement` with position and name |
| `FAQPage` | FAQ content | `mainEntity` with Question/Answer |
| `Course` | Educational content | `name`, `description`, `provider` |
| `VideoObject` | Video content | `name`, `description`, `thumbnailUrl`, `contentUrl` |

**Breadcrumb structured data:**

```html
<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "BreadcrumbList",
    "itemListElement": [
        { "@type": "ListItem", "position": 1, "name": "Home", "item": "https://example.com/" },
        { "@type": "ListItem", "position": 2, "name": "HTML", "item": "https://example.com/html/" },
        { "@type": "ListItem", "position": 3, "name": "Performance", "item": "https://example.com/html/performance" }
    ]
}
</script>
```

**Testing structured data:**
- Google Rich Results Test
- Schema.org Validator
- Google Search Console (for live data)

### Canonical URLs

The canonical URL tells search engines which URL is the authoritative version:

```html
<link rel="canonical" href="https://example.com/docs/html/performance">
```

**When to use canonical:**
- Duplicate content (same page at multiple URLs)
- www vs non-www
- HTTP vs HTTPS
- Trailing slash vs no trailing slash
- URL parameters that do not change content (sort, filter, utm_*)

**Self-referencing canonicals** — each page should link to itself as canonical:

```html
<link rel="canonical" href="https://example.com/current-page">
```

**Canonical vs redirect:**
- `301` redirect: permanently moved, search engines transfer all ranking signals
- `rel="canonical"`: weaker signal, search engines may ignore it

Use redirects when possible; use canonical when redirects are impractical.

### Pagination

`rel="next"` and `rel="prev"` indicate paginated series:

```html
<link rel="prev" href="/docs/html/page-2">
<link rel="next" href="/docs/html/page-4">
```

**Pagination best practices:**
- Each page in the series has `prev` and `next` links (except first/last)
- Use `rel="canonical"` pointing to the same page (not the first in the series)
- Consider `view-all` as a single page containing all content for search engines
- Avoid indexing every paginated page (use `noindex` on low-value pages)

### Crawl Budget

Crawl budget is the number of pages a search engine crawls on your site within a time period. Optimize with:

- `noindex` on thin content (filter pages, tag pages)
- `nofollow` on login links and admin areas
- Fix broken links (404s waste budget)
- Use `robots.txt` to block non-crawlable paths:

```
User-agent: *
Disallow: /admin/
Disallow: /search/
Allow: /
```

**`robots` meta values:** `noindex` prevents indexing, `nofollow` prevents link following, `noarchive` prevents caching, `nosnippet` prevents snippet display.

### Page Speed Metrics

**Key HTML elements that report performance:**

```html
<!-- Server timing API -->
<meta http-equiv="Server-Timing" content="cache;desc=Cache Hit;dur=1">

<!-- Viewport for mobile performance -->
<meta name="viewport" content="width=device-width, initial-scale=1">
```

**Resource timing via the Performance API:**

```javascript
// Measure specific resources
const resources = performance.getEntriesByType('resource');
resources.forEach(r => {
    console.log(`${r.name}: ${r.duration}ms`);
});

// Largest Contentful Paint
const observer = new PerformanceObserver((list) => {
    const entries = list.getEntries();
    const lastEntry = entries[entries.length - 1];
    console.log('LCP:', lastEntry.renderTime || lastEntry.loadTime);
});
observer.observe({ type: 'largest-contentful-paint', buffered: true });
```

### Measuring Performance in HTML

**Server Timing header:**

```http
Server-Timing: db;desc="Database query";dur=123, cache;desc="Cache hit";dur=2
```

**`<meta http-equiv>` for HTTP-equivalent headers (limited):**

```html
<meta http-equiv="Cache-Control" content="no-cache">
```

**User timing API for custom marks:**

```javascript
performance.mark('start-render');
// ... render ...
performance.mark('end-render');
performance.measure('render-time', 'start-render', 'end-render');
```

### Performance Budgets

A performance budget sets limits on HTML metrics:

| Metric | Budget (good) | Budget (poor) |
|---|---|---|
| HTML document size | < 50 KB | > 150 KB |
| Total page weight | < 500 KB | > 2 MB |
| Number of requests | < 30 | > 80 |
| Render-blocking resources | 0 | > 3 |
| DOM nodes | < 1500 | > 3000 |
| Deepest DOM level | < 32 | > 64 |

**Enforcing budgets in CI:**

```html
<!-- Add a lightweight beacon for monitoring -->
<img src="/pixel.gif?page=docs-html" alt="">
```

## Study Cases

### Optimizing a Blog Post Page

Before optimization:
```html
<!DOCTYPE html>
<html>
<head>
    <title>Blog Post</title>
    <link rel="stylesheet" href="/styles.css">
    <script src="/analytics.js"></script>
    <script src="/social.js"></script>
</head>
<body>
    <img src="/hero.jpg" alt="Hero">
    <!-- ... -->
</body>
</html>
```

After optimization:
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <title>HTML Performance Optimization | DevBook</title>
    <meta name="description" content="Techniques for optimizing HTML rendering and search visibility.">

    <!-- Preload critical resources -->
    <link rel="preload" href="/fonts/inter-var.woff2" as="font" crossorigin>
    <link rel="preload" href="/hero.webp" as="image" fetchpriority="high">

    <!-- Preconnect to third-party origins -->
    <link rel="preconnect" href="https://cdn.example.com">

    <!-- Critical CSS inlined -->
    <style>
        header { display: flex; padding: 1rem; }
        .hero { max-width: 1200px; }
    </style>

    <!-- Deferred non-critical CSS -->
    <link rel="stylesheet" href="/styles.css" media="print" onload="this.media='all'">

    <!-- Deferred scripts -->
    <script src="/analytics.js" defer></script>
</head>
<body>
    <!-- Explicit dimensions on all images -->
    <img src="/hero.webp" width="1200" height="600"
         alt="Hero image" fetchpriority="high">

    <article>
        <h1>HTML Performance Optimization</h1>
        <p>Published on <time datetime="2026-06-28">June 28, 2026</time></p>

        <!-- Lazy-loaded images below the fold -->
        <img src="/example.webp" loading="lazy" width="800" height="400"
             alt="Example diagram">
    </article>

    <!-- Structured data -->
    <script type="application/ld+json">
    {
        "@context": "https://schema.org",
        "@type": "Article",
        "headline": "HTML Performance Optimization",
        "datePublished": "2026-06-28"
    }
    </script>

    <!-- Canonical URL -->
    <link rel="canonical" href="https://example.com/docs/html/performance">
</body>
</html>
```

Changes: inlined critical CSS, deferred JS and non-critical CSS, preloaded hero image and fonts, added explicit dimensions, lazy-loaded below-fold images, added structured data.

### Optimizing a Product Listing Page

E-commerce listing with many product images — use `fetchpriority="high"` on the first (LCP) product and `loading="lazy"` on remaining products. Set explicit `width` and `height` on all images to prevent CLS. Preconnect to the images CDN origin.

## Examples

### Example 1: Video with poster and lazy load

```html
<video controls preload="metadata" poster="/thumbnail.webp"
       width="640" height="360">
    <source data-src="intro.mp4" type="video/mp4">
    <source data-src="intro.webm" type="video/webm">
    <p>Your browser does not support video.</p>
</video>
```

Using `preload="metadata"` prevents downloading the full video until the user clicks play. The poster image provides visual context.

### Example 2: Font loading optimization

```html
<link rel="preload" href="/fonts/inter-var.woff2" as="font" crossorigin>
<style>
    @font-face {
        font-family: 'Inter';
        src: url('/fonts/inter-var.woff2') format('woff2');
        font-display: swap;
    }
    body { font-family: 'Inter', system-ui, sans-serif; }
</style>
```

Preloading the font ensures it is discovered before the CSS rule. `font-display: swap` shows system text immediately, then swaps to Inter when loaded.

### Example 3: Responsive images with art direction

```html
<picture>
    <source srcset="hero-mobile.webp" media="(max-width: 600px)"
            type="image/webp" width="400" height="400">
    <source srcset="hero-desktop.webp" type="image/webp"
            width="1200" height="600">
    <img src="hero.jpg" width="1200" height="600"
         alt="Hero" fetchpriority="high">
</picture>
```

Mobile users download a smaller image (400×400) while desktop users get the full-size version. The `type` attribute enables WebP format for supported browsers.

## Glossary

| Term | Definition |
|---|---|
| LCP | Largest Contentful Paint — time to render the largest visible element |
| INP | Interaction to Next Paint — responsiveness to user interactions |
| CLS | Cumulative Layout Shift — visual stability metric |
| Critical path | Minimum resources needed for initial page render |
| Preload | Force early fetch of a resource for the current page |
| Prefetch | Speculative fetch for a future navigation |
| Preconnect | Early connection establishment to an origin |
| `fetchpriority` | Hint for resource loading priority |
| `loading` | Attribute for native lazy loading of images/iframes |
| `srcset` | Set of image sources for responsive selection |
| `sizes` | Display width conditions for `srcset` selection |
| `font-display` | CSS descriptor for font loading behavior |
| Structured data | JSON-LD markup for search engine understanding |
| Canonical URL | Preferred URL for duplicate content |
| Crawl budget | Number of pages a search engine crawls per time period |

## Quick References

- [Core Web Vitals](https://web.dev/articles/vitals) — Google's user experience metrics
- [web.dev Performance](https://web.dev/learn-core-web-vitals/) — optimization guides
- [Lighthouse](https://developer.chrome.com/docs/lighthouse/overview/) — automated performance auditing
- [Google Rich Results Test](https://search.google.com/test/rich-results) — structured data testing
- [PageSpeed Insights](https://pagespeed.web.dev/) — lab and field performance data
- [HTML Spec: Lazy Loading](https://html.spec.whatwg.org/multipage/urls-and-fetching.html#lazy-loading-attributes) — `loading` attribute specification

## Next Steps

- [Meta Tags & SEO](meta-tags.md) — meta elements and social graph
- [Link Relationships](link-relationships.md) — link types and SEO signals
- [Responsive Images](images.md) — image optimization techniques