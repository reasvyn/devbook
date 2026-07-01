# Meta Tags & Social Sharing

## Description

Meta tags provide metadata about an HTML document — instructions for browsers, search engines, and social media platforms. They control how a page appears in search results, how it is shared on social networks, and how browsers handle the document.

## Prerequisites

- [Document Structure](document-structure.md) — the `<head>` element and document anatomy

## Table of Contents

- [The `<meta>` Element](#the-meta-element)
- [Character Encoding](#character-encoding)
- [Viewport Configuration](#viewport-configuration)
- [Search Engine Optimization](#search-engine-optimization)
- [Social Media — Open Graph](#social-media--open-graph)
- [Social Media — Twitter Cards](#social-media--twitter-cards)
- [Browser and Platform Tags](#browser-and-platform-tags)
- [The `<title>` Element](#the-title-element)
- [The `<base>` Element](#the-base-element)
- [SEO Best Practices](#seo-best-practices)
- [Common Mistakes](#common-mistakes)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-pages)

## Content / Material

### The `<meta>` Element

`<meta>` elements provide structured metadata. They are void elements (no closing tag) and live in the `<head>`.

Common attributes:

| Attribute | Purpose |
|---|---|
| `charset` | Character encoding declaration |
| `name` | Metadata name (e.g., `description`, `viewport`) |
| `content` | Metadata value paired with `name` |
| `http-equiv` | Pragma directive (e.g., refresh, content-type) |
| `property` | Used by Open Graph for social media metadata |

### Character Encoding

```html
<meta charset="UTF-8">
```

This must be the first `<meta>` element in `<head>`. It declares the document's character encoding. UTF-8 supports all Unicode characters and is the standard for the web.

Without this declaration, the browser may misinterpret special characters (emojis, accented letters, non-Latin scripts).

### Viewport Configuration

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

This tells mobile browsers to set the viewport width to the device width and use 1:1 initial zoom. Without it, mobile browsers emulate a 980px desktop viewport and zoom out, making text tiny.

Additional viewport options:

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=3.0, user-scalable=yes">
```

| Directive | Purpose |
|---|---|
| `width=device-width` | Set width to device width |
| `initial-scale=1.0` | Initial zoom level |
| `minimum-scale` | Minimum allowed zoom |
| `maximum-scale` | Maximum allowed zoom |
| `user-scalable` | Allow or prevent pinch zoom |

Do not disable zooming (`user-scalable=no`) unless there is a strong accessibility reason. Preventing zoom violates WCAG guidelines.

### Search Engine Optimization

**Title tag** — the most important SEO element:

```html
<title>Page Title | Site Name</title>
```

The title appears in search results, browser tabs, and bookmarks. Keep it under 60 characters and include primary keywords early.

**Meta description** — the snippet shown in search results:

```html
<meta name="description" content="Learn HTML5 from scratch with practical examples. Covers elements, forms, multimedia, accessibility, and SEO best practices.">
```

Keep descriptions between 150-160 characters. Write for humans, not search engines. Each page should have a unique description.

**Robots meta tag** — controls search engine crawling:

```html
<meta name="robots" content="index, follow">
```

| Content | Meaning |
|---|---|
| `index, follow` | Index this page and follow links (default) |
| `noindex` | Do not include in search results |
| `nofollow` | Do not follow links on this page |
| `noindex, nofollow` | Neither index nor follow |
| `nosnippet` | Do not show a snippet in search results |

**Canonical URL** — prevents duplicate content issues:

```html
<link rel="canonical" href="https://example.com/page">
```

Tells search engines which URL is the authoritative version when the same content is available at multiple URLs.

### Social Media — Open Graph

Open Graph (OG) meta tags control how content appears when shared on Facebook, LinkedIn, Discord, and other platforms:

```html
<meta property="og:title" content="HTML5 Complete Guide">
<meta property="og:description" content="Learn HTML5 from scratch with practical examples.">
<meta property="og:url" content="https://example.com/html-guide">
<meta property="og:image" content="https://example.com/images/html-guide-og.png">
<meta property="og:type" content="article">
<meta property="og:site_name" content="DevBook">
<meta property="og:locale" content="en_US">
```

Required OG tags:

| Tag | Purpose |
|---|---|
| `og:title` | Title of the content |
| `og:type` | Type of content (`website`, `article`, `video.movie`, etc.) |
| `og:image` | Image URL (minimum 1200x630px recommended) |
| `og:url` | Canonical URL of the page |

Optional OG tags:

| Tag | Purpose |
|---|---|
| `og:description` | Description for the shared link |
| `og:site_name` | Name of the website |
| `og:locale` | Language/locale (e.g., `en_US`, `fr_FR`) |
| `og:image:width` | Image width in pixels |
| `og:image:height` | Image height in pixels |
| `og:image:alt` | Alt text for the image |

Best practices for OG images:
- Use 1200x630px (1.91:1 aspect ratio)
- Keep text within a safe zone (center 600x600px on Facebook)
- Use high contrast text on a solid or gradient background
- Include the page title and site logo
- Avoid using the same default image for every page

**Facebook / LinkedIn specific:**

```html
<meta property="fb:app_id" content="your-app-id">
```

### Social Media — Twitter Cards

Twitter uses its own meta tags for card display:

```html
<meta name="twitter:card" content="summary_large_image">
<meta name="twitter:site" content="@devbook">
<meta name="twitter:creator" content="@authorhandle">
<meta name="twitter:title" content="HTML5 Complete Guide">
<meta name="twitter:description" content="Learn HTML5 from scratch with practical examples.">
<meta name="twitter:image" content="https://example.com/images/html-guide-twitter.png">
```

Twitter card types:

| Card type | Description |
|---|---|
| `summary` | Title, description, small thumbnail |
| `summary_large_image` | Large image with title and description |
| `app` | Mobile app download card |
| `player` | Card with video/audio player |

If Open Graph tags exist and Twitter tags are absent, Twitter falls back to OG tags for `title`, `description`, and `image`. However, specifying Twitter tags explicitly gives you control over the Twitter card format.

### Browser and Platform Tags

**Theme color** — customize the browser UI color:

```html
<meta name="theme-color" content="#0056b3">
```

Supported in Chrome on Android and some other browsers. Changes the address bar color.

**Color scheme** — indicate dark/light mode support:

```html
<meta name="color-scheme" content="light dark">
```

Tells the browser the page supports both light and dark color schemes, enabling native form controls to adapt.

**Apple-specific:**

```html
<!-- Status bar style (iOS) -->
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent">

<!-- Web app capable (iOS full-screen mode) -->
<meta name="apple-mobile-web-app-capable" content="yes">

<!-- Smart App Banner (iOS) -->
<meta name="apple-itunes-app" content="app-id=123456789">
```

**Microsoft-specific:**

```html
<!-- Windows tile color -->
<meta name="msapplication-TileColor" content="#0056b3">

<!-- Windows tile image -->
<meta name="msapplication-TileImage" content="/mstile-144x144.png">
```

### The `<title>` Element

The `<title>` element sets the document title:

```html
<title>Meta Tags & Social Sharing — DevBook</title>
```

The title appears in:
- Browser tab and title bar
- Search engine results (as the clickable headline)
- Social media shares (when OG title is not set)
- Bookmarks and browser history

Best practices:
- Include the page name first, then the site name
- Keep under 60 characters (Google truncates at ~60)
- Make each page title unique
- Include primary keywords near the beginning
- Use a separator like `—` or `|`

### The `<base>` Element

The `<base>` element sets the base URL for all relative URLs in the document:

```html
<base href="https://example.com/docs/" target="_blank">
```

- `href` — base URL for relative links
- `target` — default browsing context for all links

Use with caution. A single `<base>` element affects every relative URL in the document, including links in images, scripts, and stylesheets. It can cause unexpected behavior when pages are moved or served from different paths.

```html
<head>
    <base href="/">
</head>
```

This is the safest usage — it ensures all relative URLs resolve from the root.

### SEO Best Practices

**Use one `<h1>` per page.** The `<h1>` should describe the page content and include primary keywords.

**Write unique meta descriptions for every page.** Duplicate descriptions hurt click-through rates.

**Use descriptive, keyword-rich titles.** The title tag is the strongest on-page SEO signal.

**Set the canonical URL.** Prevent duplicate content issues by pointing to the authoritative URL.

**Use semantic HTML elements.** Search engines favor well-structured content with `<article>`, `<section>`, `<nav>`, and `<aside>`.

**Optimize images with descriptive `alt` text.** Alt text helps search engines understand image content.

**Create an XML sitemap** and submit it to Google Search Console.

**Use structured data (Schema.org)** for rich search results:

```html
<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "Article",
    "headline": "Meta Tags & Social Sharing",
    "description": "Complete guide to HTML meta tags for SEO and social media.",
    "author": {
        "@type": "Person",
        "name": "DevBook"
    }
}
</script>
```

**Ensure fast page load times.** Search engines prioritize fast-loading pages.

### Common Mistakes

**Missing viewport meta tag.** Mobile pages without a viewport tag render at desktop width and require pinch-zoom, creating a poor user experience.

**Duplicate or missing meta descriptions.** Every page needs a unique meta description. Missing descriptions result in auto-generated snippets that may not reflect the page content.

**Generic OG image.** Using the same OG image for every page reduces the visual context shared on social media. Create unique images for important pages.

**Missing OG tags for key pages.** Pages that are frequently shared (articles, product pages) must have complete OG tags.

**Using `<base>` without understanding its scope.** A `<base>` element changes all relative URLs, which can break scripts, stylesheets, images, and links.

**Blocking CSS and JS in robots.txt.** Search engines need CSS and JavaScript to render pages for indexing. Blocking them can hurt search rankings.

**Overstuffing keywords in meta tags.** Keyword stuffing is a spam signal and can result in search engine penalties.

**Forgetting the charset declaration.** Without `<meta charset="UTF-8">`, special characters may render incorrectly.

## Glossary

| Term | Definition |
|------|------------|
| `<meta>` | HTML element for structured metadata |
| `charset` | Character encoding declaration |
| `viewport` | Meta tag for mobile viewport configuration |
| `description` | Meta tag for search result snippets |
| `robots` | Meta tag controlling search engine crawling |
| `canonical` | Link tag pointing to the authoritative URL |
| Open Graph | Protocol for social media sharing metadata (Facebook) |
| Twitter Card | Twitter's meta tag format for shared links |
| `og:image` | Image displayed when content is shared |
| `og:title` | Title displayed when content is shared |
| `og:description` | Description displayed when content is shared |
| `theme-color` | Browser UI color meta tag |
| `color-scheme` | Indicates light/dark mode support |
| Structured data | JSON-LD or Microdata for rich search results (Schema.org) |
| `<title>` | Document title shown in tabs and search results |
| `<base>` | Base URL for all relative URLs in the document |
| Sitemap | XML file listing all pages for search engines |

## Quick References

- [MDN: Meta Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta) — complete reference
- [MDN: Viewport Meta Tag](https://developer.mozilla.org/en-US/docs/Web/HTTP/Viewport-fit) — viewport documentation
- [OG Protocol](https://ogp.me/) — Open Graph specification
- [Twitter Card Documentation](https://developer.twitter.com/en/docs/twitter-for-websites/cards/guides/getting-started) — Twitter cards guide
- [Google Search Central](https://developers.google.com/search/docs/fundamentals/seo-starter-guide) — SEO starter guide
- [Schema.org](https://schema.org/) — structured data vocabulary
- [Facebook Sharing Debugger](https://developers.facebook.com/tools/debug/) — test OG tags
- [Twitter Card Validator](https://cards-dev.twitter.com/validator) — test Twitter cards

## Next Steps

- [Link Relationships](link-relationships.md) — `<link>` relationships and uses
- [Semantic HTML & Document Outline](semantic-html.md) — structural meaning in markup
- [Performance & SEO Basics](performance-seo.md) — optimizing page performance
