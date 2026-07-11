# Link Relationships

## Description

The `<link>` element defines relationships between the current document and external resources. Link relationships control stylesheets, preloading, prefetching, icon configuration, and alternative versions of the page.

## Prerequisites

- [Document Structure](document-structure.md) — the `<head>` element and document anatomy
- [Meta Tags & Social Sharing](meta-tags.md) — metadata patterns

## Table of Contents

- [The `<link>` Element](#the-link-element)
- [Stylesheets](#stylesheets)
- [Icons and Favicons](#icons-and-favicons)
- [Preloading Resources](#preloading-resources)
- [Prefetching and Prerendering](#prefetching-and-prerendering)
- [Alternate Versions](#alternate-versions)
- [Canonical URL](#canonical-url)
- [PWA and App Manifests](#pwa-and-app-manifests)
- [DNS Prefetching](#dns-prefetching)
- [Link Types Reference](#link-types-reference)
- [Performance Impact](#performance-impact)
- [Common Mistakes](#common-mistakes)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The `<link>` Element

The `<link>` element specifies a relationship between the current document and an external resource. It is a void element (no closing tag) and lives in the `<head>`.

```html
<link rel="stylesheet" href="styles.css">
```

Required attributes:

- `rel` — the relationship type (e.g., `stylesheet`, `preload`, `icon`)
- `href` — the URL of the linked resource

Common attributes:

| Attribute | Purpose |
|---|---|
| `rel` | Relationship type (required) |
| `href` | Resource URL (required for most relationships) |
| `type` | MIME type of the linked resource |
| `media` | Media query for conditional linking |
| `sizes` | Icon sizes (for `rel="icon"`) |
| `as` | Type of content being preloaded (for `rel="preload"`) |
| `crossorigin` | CORS setting for the resource |
| `integrity` | Subresource Integrity hash for security |
| `hreflang` | Language of the linked resource |

### Stylesheets

The most common use of `<link>` is loading CSS:

```html
<link rel="stylesheet" href="styles.css">
<link rel="stylesheet" href="print.css" media="print">
<link rel="stylesheet" href="mobile.css" media="(max-width: 768px)">
```

**Multiple stylesheets** — each `<link>` loads independently:

```html
<link rel="stylesheet" href="reset.css">
<link rel="stylesheet" href="base.css">
<link rel="stylesheet" href="components.css">
```

**Media queries** — apply styles conditionally:

```html
<!-- Print only -->
<link rel="stylesheet" href="print.css" media="print">

<!-- Screen only -->
<link rel="stylesheet" href="screen.css" media="screen">

<!-- Responsive breakpoint -->
<link rel="stylesheet" href="desktop.css" media="(min-width: 1024px)">
<link rel="stylesheet" href="tablet.css" media="(min-width: 768px) and (max-width: 1023px)">
<link rel="stylesheet" href="mobile.css" media="(max-width: 767px)">
```

**Alternate stylesheets** — user-selectable themes (rarely used):

```html
<link rel="stylesheet" href="default.css" title="Default Theme">
<link rel="alternate stylesheet" href="dark.css" title="Dark Theme">
<link rel="alternate stylesheet" href="high-contrast.css" title="High Contrast">
```

The browser provides a menu to switch between stylesheets with a `title` attribute.

**Subresource Integrity (SRI)** — verify that the loaded CSS has not been tampered with:

```html
<link rel="stylesheet" href="https://cdn.example.com/styles.css"
      integrity="sha384-oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC"
      crossorigin="anonymous">
```

### Icons and Favicons

**Basic favicon:**

```html
<link rel="icon" href="/favicon.ico" sizes="any">
```

**Modern favicon setup:**

```html
<!-- Classic fallback -->
<link rel="icon" href="/favicon.ico" sizes="any">

<!-- SVG favicon (scales infinitely) -->
<link rel="icon" href="/favicon.svg" type="image/svg+xml">

<!-- PNG favicons for different sizes -->
<link rel="icon" href="/favicon-16x16.png" sizes="16x16" type="image/png">
<link rel="icon" href="/favicon-32x32.png" sizes="32x32" type="image/png">

<!-- Apple touch icon (iOS home screen) -->
<link rel="apple-touch-icon" href="/apple-touch-icon.png">

<!-- Apple touch icon with specific size -->
<link rel="apple-touch-icon" sizes="180x180" href="/apple-touch-icon-180x180.png">
```

**The `sizes` attribute** tells the browser the icon dimensions:

```html
<link rel="icon" sizes="192x192" href="/icon-192.png">
```

**Shortcut icon** — the older, deprecated syntax:

```html
<link rel="shortcut icon" href="/favicon.ico">
```

Modern browsers treat `shortcut icon` the same as `icon`.

**Maskable icons** for Android adaptive icons:

```html
<link rel="icon" href="/maskable-icon.png" sizes="192x192" type="image/png">
```

### Preloading Resources

`rel="preload"` tells the browser to fetch a resource early, before it is discovered in the HTML:

```html
<!-- Preload critical CSS -->
<link rel="preload" href="critical.css" as="style">

<!-- Preload hero image -->
<link rel="preload" href="hero.webp" as="image" type="image/webp">

<!-- Preload font -->
<link rel="preload" href="/fonts/inter-var.woff2" as="font" type="font/woff2" crossorigin>

<!-- Preload script -->
<link rel="preload" href="app.js" as="script">

<!-- Preload with media query (responsive preload) -->
<link rel="preload" href="bg-desktop.webp" as="image" media="(min-width: 768px)">
<link rel="preload" href="bg-mobile.webp" as="image" media="(max-width: 767px)">
```

The `as` attribute is required for `rel="preload"`. It tells the browser what type of resource is being preloaded:

| `as` value | Resource type |
|---|---|
| `style` | CSS stylesheet |
| `script` | JavaScript file |
| `image` | Image file |
| `font` | Web font (requires `crossorigin`) |
| `fetch` | XHR or fetch response |
| `document` | HTML document (for iframes) |
| `video` | Video file |
| `audio` | Audio file |

**Preload vs stylesheet** — when you want a CSS file to both preload and apply:

```html
<link rel="preload" href="critical.css" as="style" onload="this.rel='stylesheet'">
<noscript><link rel="stylesheet" href="critical.css"></noscript>
```

This preloads the CSS but does not apply it immediately. The `onload` handler changes it to a stylesheet. This pattern is used for CSS that is not render-blocking.

**Preload as early as possible** — place preload links at the beginning of `<head>`, before other resource declarations.

### Prefetching and Prerendering

`rel="prefetch"` hints that a resource will be needed for the next navigation:

```html
<!-- Prefetch the next page -->
<link rel="prefetch" href="/next-page.html">

<!-- Prefetch an image on the next page -->
<link rel="prefetch" href="/images/next-hero.webp" as="image">
```

Browsers fetch prefetched resources at the lowest priority, during idle time. They are cached for future use.

**`rel="prerender"`** (deprecated) — renders the entire page in the background:

```html
<!-- Deprecated — use speculation rules instead -->
<link rel="prerender" href="/next-page.html">
```

`prerender` is deprecated in favor of the Speculation Rules API:

```html
<script type="speculationrules">
{
    "prerender": [
        { "source": "list", "urls": ["/next-page.html", "/next-next-page.html"] }
    ],
    "prefetch": [
        { "source": "list", "urls": ["/about.html", "/contact.html"] }
    ]
}
</script>
```

Speculation rules provide more control than prefetch/prerender link elements.

**`rel="preconnect"`** — establish an early connection to an origin:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://cdn.example.com">
```

This performs DNS resolution, TCP handshake, and TLS negotiation before the browser encounters resources from that origin. Use it for critical third-party origins.

**`rel="dns-prefetch"`** — a lighter version that only resolves DNS:

```html
<link rel="dns-prefetch" href="https://analytics.example.com">
```

`preconnect` is more powerful but uses more browser resources. Use `preconnect` for critical origins and `dns-prefetch` for less important ones.

### Alternate Versions

**Alternate language versions:**

```html
<link rel="alternate" href="https://example.com/" hreflang="en">
<link rel="alternate" href="https://example.com/fr/" hreflang="fr">
<link rel="alternate" href="https://example.com/es/" hreflang="es">
<link rel="alternate" href="https://example.com/" hreflang="x-default">
```

`x-default` specifies the default language version. Search engines use these to serve the correct language in search results.

**Alternate formats — RSS feeds:**

```html
<link rel="alternate" type="application/rss+xml" title="DevBook Blog" href="/blog/feed.xml">

<link rel="alternate" type="application/atom+xml" title="DevBook Blog" href="/blog/feed.atom">
```

**Alternate formats — JSON:**

```html
<link rel="alternate" type="application/json" title="DevBook API" href="/api/posts.json">
```

**Alternate mobile version (deprecated):**

```html
<link rel="alternate" media="only screen and (max-width: 640px)" href="https://m.example.com/">
```

Modern responsive design makes this unnecessary.

### Canonical URL

```html
<link rel="canonical" href="https://example.com/original-page">
```

The canonical URL tells search engines which URL is the authoritative version of the page. Use it when:

- The same content is available at multiple URLs (e.g., `/article` and `/article?ref=home`)
- HTTP and HTTPS versions both exist
- `www` and non-`www` versions both exist
- Pages have URL parameters that do not change the content

The canonical URL should be absolute (not relative) to avoid ambiguity.

### PWA and App Manifests

**Web app manifest** — a JSON file that defines how the app behaves when installed:

```html
<link rel="manifest" href="/site.webmanifest">
```

Example manifest:

```json
{
    "name": "DevBook",
    "short_name": "DevBook",
    "description": "Developer learning resources",
    "start_url": "/",
    "display": "standalone",
    "background_color": "#ffffff",
    "theme_color": "#0056b3",
    "icons": [
        {
            "src": "/icon-192.png",
            "sizes": "192x192",
            "type": "image/png",
            "purpose": "any maskable"
        },
        {
            "src": "/icon-512.png",
            "sizes": "512x512",
            "type": "image/png"
        }
    ]
}
```

**Service worker registration** (via JavaScript, not a link element):

```javascript
if ('serviceWorker' in navigator) {
    navigator.serviceWorker.register('/sw.js');
}
```

Service workers enable offline support, push notifications, and background sync.

### DNS Prefetching

DNS prefetching resolves domain names before the browser needs them:

```html
<link rel="dns-prefetch" href="//fonts.googleapis.com">
<link rel="dns-prefetch" href="//cdn.example.com">
<link rel="dns-prefetch" href="//analytics.google.com">
```

This saves the DNS lookup time (typically 20-120ms) when the browser later encounters a request to that domain.

Use `dns-prefetch` for:
- Third-party domains that host resources
- CDN origins
- Analytics and tracking endpoints

### Link Types Reference

| `rel` value | Purpose |
|---|---|
| `stylesheet` | CSS file to style the page |
| `icon` | Favicon or icon for the site |
| `apple-touch-icon` | Icon for iOS home screen |
| `preload` | Fetch a critical resource early |
| `prefetch` | Fetch a resource for the next page |
| `preconnect` | Establish early connection to an origin |
| `dns-prefetch` | Resolve a domain name early |
| `canonical` | Authoritative URL for SEO |
| `alternate` | Alternative version (language, format) |
| `manifest` | PWA web app manifest |
| `next` | Next page in a series (rarely used) |
| `prev` | Previous page in a series (rarely used) |
| `help` | Link to help documentation |
| `license` | Link to license information |
| `author` | Link to the author's page |
| `search` | Link to search page (OpenSearch) |
| `modulepreload` | Preload an ES module |
| `expect` | Expectation for the document (experimental) |

**`rel="modulepreload"`** — preload JavaScript modules:

```html
<link rel="modulepreload" href="app.mjs">
<link rel="modulepreload" href="utils.mjs">
```

Similar to `preload` but specifically for ES modules. The browser also parses and compiles the module, not just downloads it.

### Performance Impact

Link relationships significantly affect page load performance when used correctly:

**`preload`** is the most impactful. Use it for:
- Above-the-fold images (LCP element)
- Critical CSS files
- Hero fonts
- Key scripts needed for first paint

Overuse of `preload` competes for bandwidth. Only preload 1-3 critical resources.

**`preconnect`** helps with third-party origins but uses browser resources. Limit to 3-6 origins.

**`prefetch`** should be used sparingly. Prefetching too many resources wastes bandwidth and may cause unnecessary server load.

**`dns-prefetch`** is low-cost and can be used freely for all third-party origins.

**Optimal ordering in `<head>`:**

```html
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Page Title</title>

    <!-- Critical preloads first -->
    <link rel="preload" href="critical.css" as="style">
    <link rel="preload" href="hero.webp" as="image">
    <link rel="preload" href="/fonts/inter.woff2" as="font" crossorigin>

    <!-- Preconnect to third parties -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="dns-prefetch" href="https://analytics.example.com">

    <!-- Stylesheets -->
    <link rel="stylesheet" href="styles.css">

    <!-- Icons -->
    <link rel="icon" href="/favicon.ico" sizes="any">

    <!-- Prefetch for next page -->
    <link rel="prefetch" href="/next-page.html">
</head>
```

### Common Mistakes

**Over-preloading.** Preloading too many resources defeats the purpose — the browser cannot prioritize them properly. Only preload 1-3 critical resources.

**Preloading with missing `as` attribute.** Without `as`, the browser cannot prioritize the preload correctly. The `as` attribute is required.

**Preloading fonts without `crossorigin`.** Font preloads require the `crossorigin` attribute, even if the font is same-origin:

```html
<link rel="preload" href="/fonts/custom.woff2" as="font" type="font/woff2" crossorigin>
```

**Using `preconnect` for too many origins.** Each preconnect uses browser resources (TCP connections, TLS). Limit to the most critical origins.

**Confusing `preload` and `prefetch`.** `preload` is for resources needed now; `prefetch` is for resources needed later. Using the wrong one can hurt performance.

**Missing `rel="canonical"` on pages with URL parameters.** Without a canonical URL, search engines may index multiple versions of the same page.

**Not specifying `sizes` on icons.** Without `sizes`, the browser does not know which icon to use for different contexts.

## Glossary

| Term | Definition |
|------|------------|
| `<link>` | HTML element for linking external resources |
| `rel` | Relationship type between current document and linked resource |
| `href` | URL of the linked resource |
| `as` | Resource type for preload (style, script, image, font, fetch) |
| `sizes` | Icon dimensions for favicons |
| `crossorigin` | CORS setting for the linked resource |
| `integrity` | Subresource Integrity hash for security verification |
| `hreflang` | Language of the linked alternate resource |
| Preload | Fetch a resource needed for the current page |
| Prefetch | Fetch a resource needed for the next page |
| Preconnect | Establish early connection to a third-party origin |
| DNS-prefetch | Resolve a domain name before it is needed |
| Canonical | Authoritative URL for SEO duplicate content handling |
| Manifest | JSON file defining PWA installation properties |
| SRI | Subresource Integrity — cryptographic hash for resource verification |
| Service Worker | JavaScript that enables offline caching and push notifications |
| PWA | Progressive Web App — installable web application |
| FOIT | Flash of Invisible Text — text hidden while font loads |
| FOUT | Flash of Unstyled Text — text shown in fallback font before swap |
| LCP | Largest Contentful Paint — Core Web Vital metric |

## Quick References

- [MDN: Link Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link) — complete reference
- [MDN: Link Types](https://developer.mozilla.org/en-US/docs/Web/HTML/Link_types) — all `rel` values
- [MDN: Preloading Content](https://developer.mozilla.org/en-US/docs/Web/HTML/Link_types/preload) — preload guide
- [MDN: Prefetch](https://developer.mozilla.org/en-US/docs/Web/HTTP/Link_prefetching_FAQ) — prefetch FAQ
- [MDN: Subresource Integrity](https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity) — SRI guide
- [HTML Spec: Links](https://html.spec.whatwg.org/multipage/links.html) — official specification
- [web.dev: Preload and Prefetch](https://web.dev/preload-and-prefetch/) — performance guide
- [Google: Resource Prioritization](https://developers.google.com/web/fundamentals/performance/resource-prioritization) — prioritization guide

## Next Steps

- [Script Loading & Integration](scripts-and-defer.md) — JavaScript loading strategies
- [IFrames & Embeds](iframes-and-embeds.md) — embedding external content
- [Performance & SEO Basics](performance-seo.md) — optimizing page performance
