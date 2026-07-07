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
- [Structured Data with JSON-LD](#structured-data-with-json-ld)
- [Meta Refresh and Redirects](#meta-refresh-and-redirects)
- [Browser and Platform Tags](#browser-and-platform-tags)
- [The `<title>` Element](#the-title-element)
- [The `<base>` Element](#the-base-element)
- [SEO Best Practices](#seo-best-practices)
- [Common Mistakes](#common-mistakes)
- [Learning Tips](#learning-tips)
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

**Why charset matters:**

The browser uses the charset declaration to decode the byte stream into characters. If the declaration is missing or incorrect:

- The browser may fall back to a legacy encoding (Windows-1252 in some browsers)
- Characters above U+007F may render as mojibake (garbled text)
- Security vulnerabilities like UTF-7-based XSS attacks become possible

**UTF-8 as the default:**

Every modern web page should use UTF-8. It encodes all Unicode code points, is backward-compatible with ASCII, and is the most space-efficient encoding for Latin text among full Unicode encodings.

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

**Viewport and responsive design:**

The viewport meta tag is required for responsive design to work. It tells the browser to lay out the page at the actual device width rather than an emulated desktop width. Combined with CSS media queries and flexible grid layouts, it enables a single HTML document to render well on phones, tablets, and desktops.

**Viewport fit:**

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
```

The `viewport-fit` property controls how the page interacts with display notches and rounded corners (iPhone X and later). Setting it to `cover` allows the page to extend into the safe areas, while `safe-area-inset-*` CSS environment variables provide padding values.

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

**Additional SEO meta tags:**

```html
<!-- Language and region targeting -->
<meta name="language" content="en">

<!-- Revisit after (ignored by most modern search engines) -->
<meta name="revisit-after" content="7 days">

<!-- Rating (for adult content flagging) -->
<meta name="rating" content="general">

<!-- Google-specific: prevent email/phone auto-detection -->
<meta name="format-detection" content="telephone=no">
```

**Google-specific meta tags:**

```html
<!-- Enable Google's site search box in results -->
<meta name="google" content="nositelinkssearchbox">

<!-- Prevent translation in Google search results -->
<meta name="google" content="notranslate">

<!-- Google News verification -->
<meta name="googlebot-news" content="noindex">
```

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

**Open Graph objects and types:**

The `og:type` property specifies the nature of the content. Common values include `website` (default), `article`, `book`, `video.movie`, `video.episode`, `music.song`, `music.album`, `profile`, and `product`. Each type has additional required properties:

```html
<!-- Article type with additional properties -->
<meta property="og:type" content="article">
<meta property="article:published_time" content="2026-01-15T10:00:00Z">
<meta property="article:modified_time" content="2026-03-20T14:30:00Z">
<meta property="article:author" content="https://example.com/authors/jane-doe">
<meta property="article:section" content="Technology">
<meta property="article:tag" content="HTML">
<meta property="article:tag" content="SEO">

<!-- Product type -->
<meta property="og:type" content="product">
<meta property="product:price:amount" content="29.99">
<meta property="product:price:currency" content="USD">
<meta property="product:availability" content="in stock">
```

**Facebook / LinkedIn specific:**

```html
<meta property="fb:app_id" content="your-app-id">
```

The `fb:app_id` associates the page with a Facebook app, enabling insights about how people share and engage with the content.

**Debugging Open Graph:**

Facebook provides the [Sharing Debugger](https://developers.facebook.com/tools/debug/) to test how a URL will appear when shared. LinkedIn has a similar [Post Inspector](https://www.linkedin.com/post-inspector/). Both tools show scraped OG data and flag missing or problematic tags.

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

**Twitter player card:**

For video and audio content:

```html
<meta name="twitter:card" content="player">
<meta name="twitter:player" content="https://example.com/video-player">
<meta name="twitter:player:width" content="640">
<meta name="twitter:player:height" content="360">
<meta name="twitter:player:stream" content="https://example.com/video.mp4">
```

The player card allows users to play video or audio directly in the Twitter timeline. The player URL must be an HTTPS page that includes the Twitter player JavaScript library.

**Card validator:**

Use the [Twitter Card Validator](https://cards-dev.twitter.com/validator) to preview how a URL will render as a Twitter card. It shows the raw meta tags Twitter finds and any errors in the card configuration.

### Structured Data with JSON-LD

Structured data helps search engines understand the content and context of a page, enabling rich search results like review stars, recipe cards, FAQ dropdowns, and event listings.

**JSON-LD format (recommended):**

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
    },
    "datePublished": "2026-01-15",
    "dateModified": "2026-03-20",
    "publisher": {
        "@type": "Organization",
        "name": "DevBook",
        "logo": {
            "@type": "ImageObject",
            "url": "https://example.com/logo.png"
        }
    }
}
</script>
```

**Common schema types:**

| Schema type | Use case | Key properties |
|---|---|---|
| `Article` | Blog posts, news | headline, author, datePublished, image |
| `Product` | E-commerce | name, offers, brand, aggregateRating |
| `LocalBusiness` | Physical stores | name, address, telephone, openingHours |
| `Event` | Events, webinars | name, startDate, location, offers |
| `Recipe` | Cooking content | name, cookTime, recipeIngredient, nutrition |
| `FAQPage` | FAQ sections | mainEntity (list of Question) |
| `HowTo` | Tutorials | step, tool, supply, totalTime |
| `BreadcrumbList` | Navigation paths | itemListElement (list of ListItem) |
| `SoftwareApplication` | Apps, tools | name, applicationCategory, offers |

**FAQPage example:**

```html
<script type="application/ld+json">
{
    "@context": "https://schema.org",
    "@type": "FAQPage",
    "mainEntity": [{
        "@type": "Question",
        "name": "What are meta tags?",
        "acceptedAnswer": {
            "@type": "Answer",
            "text": "Meta tags provide metadata about an HTML document for browsers and search engines."
        }
    }]
}
</script>
```

**Testing structured data:**

Google provides the [Rich Results Test](https://search.google.com/test/rich-results) and [Schema Markup Validator](https://validator.schema.org/) to verify JSON-LD syntax and eligibility for rich results.

**JSON-LD vs. Microdata vs. RDFa:**

| Format | Embedding | Readability |
|--------|-----------|-------------|
| JSON-LD | Separate `<script>` block | Easy to write and maintain |
| Microdata | Inline HTML attributes | Hard to maintain, clutters markup |
| RDFa | Inline HTML attributes | Most verbose, rarely used |

JSON-LD is Google's recommended format. It does not touch the visible HTML and is easier to generate programmatically.

### Meta Refresh and Redirects

```html
<meta http-equiv="refresh" content="5; url=https://example.com/new-page">
```

The `http-equiv="refresh"` attribute instructs the browser to refresh the page after a specified delay, optionally navigating to a URL.

**Use cases:**

```html
<!-- Refresh page every 30 seconds (live dashboards) -->
<meta http-equiv="refresh" content="30">

<!-- Redirect after 0 seconds (immediate redirect) -->
<meta http-equiv="refresh" content="0; url=https://example.com/new-page">

<!-- Redirect with a delay and message -->
<meta http-equiv="refresh" content="5; url=https://example.com/new-page">
```

**When to use meta refresh:**

| Scenario | Recommendation |
|----------|---------------|
| Landing page redirect | Prefer server-side 301/302 redirects |
| Countdown redirect | Acceptable (e.g., "Redirecting in 5 seconds...") |
| Auto-refresh dashboard | Acceptable for internal tools, not public pages |
| Fallback for JS redirect | Acceptable as a `<noscript>` fallback |

**Accessibility concerns:**

Meta refresh redirects are problematic for screen readers and users with cognitive disabilities. The W3C recommends avoiding automatic redirects unless they are triggered by user action. The `content` value of 0 (immediate redirect) is even more disruptive because the user has no time to perceive the intermediate page.

**SEO impact:**

Google treats a 0-second meta refresh as a soft 301 redirect, passing most link equity. Delayed redirects (over a few seconds) may be treated differently. For permanent URL changes, always use server-side 301 redirects with proper HTTP status codes.

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

**Google-specific (Android):**

```html
<!-- Android Chrome theme color -->
<meta name="theme-color" content="#0056b3">

<!-- Android web app manifest -->
<link rel="manifest" href="/manifest.json">
```

The web app manifest (`manifest.json`) defines how the app appears when installed on a user's home screen, including icons, splash screen, and display mode.

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

**Title patterns by page type:**

| Page type | Pattern | Example |
|-----------|---------|---------|
| Homepage | Site Name | DevBook |
| Article | Article Title | Site Name | Meta Tags Guide | DevBook |
| Product | Product Name | Site Name | Pro Plan | DevBook |
| Category | Category | Site Name | HTML Guides | DevBook |
| Search results | Search Term | Site Name | meta tags | DevBook |

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

**Common base-related pitfalls:**

- Anchors (#section) are resolved relative to the base URL, not the current page
- The `<base>` element affects dynamically created links in JavaScript
- Only one `<base>` element is allowed per document
- The `<base>` element must be placed before any URL-dependent elements

### SEO Best Practices

**Use one `<h1>` per page.** The `<h1>` should describe the page content and include primary keywords.

**Write unique meta descriptions for every page.** Duplicate descriptions hurt click-through rates.

**Use descriptive, keyword-rich titles.** The title tag is the strongest on-page SEO signal.

**Set the canonical URL.** Prevent duplicate content issues by pointing to the authoritative URL.

**Use semantic HTML elements.** Search engines favor well-structured content with `<article>`, `<section>`, `<nav>`, and `<aside>`.

**Optimize images with descriptive `alt` text.** Alt text helps search engines understand image content.

**Create an XML sitemap** and submit it to Google Search Console.

**Use structured data (Schema.org)** for rich search results.

**Ensure fast page load times.** Search engines prioritize fast-loading pages.

**Optimize for Core Web Vitals:** Largest Contentful Paint (LCP), First Input Delay (FID), and Cumulative Layout Shift (CLS) are ranking signals.

### Common Mistakes

**Missing viewport meta tag.** Mobile pages without a viewport tag render at desktop width and require pinch-zoom, creating a poor user experience.

**Duplicate or missing meta descriptions.** Every page needs a unique meta description. Missing descriptions result in auto-generated snippets that may not reflect the page content.

**Generic OG image.** Using the same OG image for every page reduces the visual context shared on social media. Create unique images for important pages.

**Missing OG tags for key pages.** Pages that are frequently shared (articles, product pages) must have complete OG tags.

**Using `<base>` without understanding its scope.** A `<base>` element changes all relative URLs, which can break scripts, stylesheets, images, and links.

**Blocking CSS and JS in robots.txt.** Search engines need CSS and JavaScript to render pages for indexing. Blocking them can hurt search rankings.

**Overstuffing keywords in meta tags.** Keyword stuffing is a spam signal and can result in search engine penalties.

**Forgetting the charset declaration.** Without `<meta charset="UTF-8">`, special characters may render incorrectly.

**Using multiple `<h1>` elements.** Only one `<h1>` per page; multiple dilutes semantic structure.

**Missing alt text on images.** Images without alt text are invisible to search engines and inaccessible to screen readers.

**Not testing social share previews.** An OG image that looks good on Facebook may be cropped differently on LinkedIn or Twitter. Always test on multiple platforms.

## Learning Tips

- **Use a meta tag generator for complex pages.** Tools like Megatags.co and Metatags.io provide previews of how a page will look on Google, Facebook, Twitter, and LinkedIn simultaneously. This helps catch missing tags before deployment.
- **Test every page with the Facebook Sharing Debugger and Twitter Card Validator.** These tools show exactly which tags each platform reads and flag errors. Make this part of your deployment checklist.
- **Keep a template handy.** Start every new HTML document from a template that includes charset, viewport, title, description, and OG tags. This prevents forgetting critical tags.
- **Install a browser extension for debugging.** Extensions like "Meta SEO Inspector" or "Detailed SEO" show all meta tags on a page with a single click, making it easy to audit competitors and diagnose issues.
- **Learn to read search engine result previews.** Before publishing, use tools like Google's Rich Results Test to see how your title and description will appear in search results. Adjust phrasing to maximize click-through rate.
- **Audit structured data with Google's Schema Validator.** JSON-LD can have subtle syntax errors. The validator catches missing brackets, invalid types, and incorrect property names.
- **Generate OG images programmatically.** Services like Vercel OG or Puppeteer can generate Open Graph images on-the-fly with the page title and branding. This ensures every page has a unique, professional-looking OG image without manual design work.
- **Monitor performance impact of redirects.** Meta refresh redirects delay page transitions and affect user experience. Prefer server-side redirects and reserve meta refresh for true fallback scenarios.

## Glossary

| Term | Definition |
|------|------------|
| `<meta>` | HTML element for structured metadata in the document head |
| `charset` | Character encoding declaration for the HTML document |
| `viewport` | Meta tag for mobile viewport width and zoom configuration |
| `description` | Meta tag providing a summary shown in search result snippets |
| `robots` | Meta tag controlling search engine indexing and link following |
| `canonical` | Link tag pointing to the authoritative URL for duplicate content |
| Open Graph | Protocol by Facebook for rich social media sharing metadata |
| OG image | Image displayed when a URL is shared on social platforms |
| OG title | Title displayed when a URL is shared via Open Graph |
| OG description | Description displayed alongside the OG title in shares |
| Twitter Card | Twitter's meta tag format controlling tweet card display |
| `summary_large_image` | Twitter card type with large hero image |
| Structured data | Machine-readable content format (JSON-LD, Microdata, RDFa) |
| JSON-LD | JavaScript Object Notation for Linked Data — Google's preferred structured data format |
| Schema.org | Collaborative vocabulary for structured data on the web |
| Rich results | Enhanced search results with images, ratings, or other data |
| `theme-color` | Browser UI color meta tag for address bar theming |
| `color-scheme` | Indicates light/dark mode support for native form controls |
| `http-equiv` | Meta tag attribute for HTTP header simulations |
| Meta refresh | http-equiv directive to refresh or redirect a page |
| Core Web Vitals | Google's performance metrics (LCP, FID, CLS) used as ranking signals |
| Sitemap | XML file listing all pages of a site for search engine crawlers |
| `<title>` | Document title shown in browser tabs, bookmarks, and search results |
| `<base>` | Base URL element for resolving all relative URLs in a document |
| Viewport fit | CSS property controlling layout and rendering in relation to the device's physical boundaries |
| LCP | Largest Contentful Paint — measures perceived load speed |
| CLS | Cumulative Layout Shift — measures visual stability during page load |
| Link equity | SEO value passed from one page to another through hyperlinks |
| Mozjibake | Garbled text resulting from incorrect character encoding interpretation |

## Quick References

- [MDN: Meta Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/meta) — complete reference
- [MDN: Viewport Meta Tag](https://developer.mozilla.org/en-US/docs/Web/HTTP/Viewport-fit) — viewport documentation
- [OG Protocol](https://ogp.me/) — Open Graph specification
- [Twitter Card Documentation](https://developer.twitter.com/en/docs/twitter-for-websites/cards/guides/getting-started) — Twitter cards guide
- [Google Search Central](https://developers.google.com/search/docs/fundamentals/seo-starter-guide) — SEO starter guide
- [Schema.org](https://schema.org/) — structured data vocabulary
- [Facebook Sharing Debugger](https://developers.facebook.com/tools/debug/) — test OG tags
- [Twitter Card Validator](https://cards-dev.twitter.com/validator) — test Twitter cards
- [Google Rich Results Test](https://search.google.com/test/rich-results) — validate structured data
- [Schema Markup Validator](https://validator.schema.org/) — validate schema.org markup
- [JSON-LD Playground](https://json-ld.org/playground/) — test and debug JSON-LD documents
- [Metatags.io](https://metatags.io/) — preview social media and search previews
- [LinkedIn Post Inspector](https://www.linkedin.com/post-inspector/) — test LinkedIn share previews
- [Web App Manifest Docs](https://developer.mozilla.org/en-US/docs/Web/Manifest) — PWA manifest configuration
- [Google Search Console](https://search.google.com/search-console) — monitor search performance

## Next Steps

- [Link Relationships](link-relationships.md) — `<link>` relationships and uses
- [Semantic HTML & Document Outline](semantic-html.md) — structural meaning in markup
- [Performance & SEO Basics](performance-seo.md) — optimizing page performance
