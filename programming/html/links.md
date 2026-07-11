# Links & Navigation

## Description

Links are the defining feature of the web — they connect documents, enable navigation, and turn isolated pages into a interconnected network. HTML provides the `<a>` element for creating hyperlinks, along with attributes and patterns for navigation menus, anchors, and downloadable resources.

## Prerequisites

- [Elements, Tags & Attributes](elements-and-attributes.md) — fundamental HTML building blocks
- [Text Content](text-content.md) — headings, paragraphs, lists

## Table of Contents

- [The Anchor Element](#the-anchor-element)
- [Absolute vs Relative URLs](#absolute-vs-relative-urls)
- [Fragment Links (Anchors)](#fragment-links-anchors)
- [The `target` Attribute](#the-target-attribute)
- [The `rel` Attribute](#the-rel-attribute)
- [Download Links](#download-links)
- [Email and Telephone Links](#email-and-telephone-links)
- [Navigation Menus](#navigation-menus)
- [Breadcrumb Navigation](#breadcrumb-navigation)
- [Skip Links](#skip-links)
- [Link States and Styling](#link-states-and-styling)
- [Accessible Links](#accessible-links)
- [Link Relationships](#link-relationships)
- [Common Mistakes](#common-mistakes)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Anchor Element

The `<a>` element (anchor) creates a hyperlink. Users can click it to navigate to another page, a section within the same page, or trigger a downloadable file.

```html
<a href="https://example.com">Visit Example</a>
```

The `href` attribute (hypertext reference) specifies the destination. Without `href`, the `<a>` element is a placeholder (not clickable) and is not focusable via keyboard navigation.

The `<a>` element is inline by default but can wrap block-level content in HTML5:

```html
<a href="/articles/first-article" class="card-link">
    <article>
        <h2>Article Title</h2>
        <p>Article summary goes here.</p>
    </article>
</a>
```

This is useful for card-based layouts where the entire card should be clickable. However, interactive content (like `<button>`, `<input>`, or another `<a>`) must not be nested inside an anchor.

### Absolute vs Relative URLs

**Absolute URLs** specify the full path including the protocol and domain:

```html
<a href="https://developer.mozilla.org/en-US/docs/Web/HTML">MDN HTML Docs</a>
<a href="https://example.com/images/photo.jpg">Photo</a>
```

Use absolute URLs when linking to resources on a different domain.

**Relative URLs** specify the path relative to the current document:

```html
<!-- Same directory -->
<a href="about.html">About</a>

<!-- Subdirectory -->
<a href="team/members.html">Team Members</a>

<!-- Parent directory -->
<a href="../index.html">Home</a>

<!-- Root-relative (from the domain root) -->
<a href="/products/index.html">Products</a>
```

Root-relative URLs start with `/` and resolve from the domain root regardless of the current page's depth. They are the most maintainable option within a single domain because they work from any page depth.

```html
<!-- From /products/details/item.html -->
<a href="/about/">About</a>              <!-- Goes to /about/ -->
<a href="../about/">About</a>            <!-- Also goes to /about/ but breaks if page is moved -->
```

**Protocol-relative URLs** omit the protocol and use the current page's protocol:

```html
<a href="//fonts.googleapis.com/css?family=Open+Sans">Google Fonts</a>
```

This pattern was used to avoid mixed content warnings (HTTP vs HTTPS). With HTTPS now being the norm, protocol-relative URLs are less common and generally unnecessary.

### Fragment Links (Anchors)

Fragment links navigate to a specific part of the same page by referencing an element's `id`:

```html
<!-- Link to the section -->
<a href="#section-2">Go to Section 2</a>

<!-- Target section -->
<h2 id="section-2">Section 2</h2>
```

The fragment is the part after `#` — it must match the `id` of the target element. When clicked, the browser scrolls the target element into view and updates the URL.

Fragment links can also target elements on other pages:

```html
<a href="document-structure.html#doctype">Learn about DOCTYPE</a>
```

The `id` attribute must be unique on the page. If multiple elements share the same `id`, the browser scrolls to the first one found.

JavaScript can also scroll to elements programmatically:

```javascript
document.getElementById('section-2').scrollIntoView({ behavior: 'smooth' });
```

### The `target` Attribute

The `target` attribute controls where the linked content opens:

```html
<a href="https://example.com" target="_self">Default (same tab)</a>
<a href="https://example.com" target="_blank">New tab or window</a>
<a href="https://example.com" target="_parent">Parent frame (iframes)</a>
<a href="https://example.com" target="_top">Full body of the window (iframes)</a>
```

**`_self`** — the default. Opens the link in the same browsing context.

**`_blank`** — opens in a new tab or window. Always include `rel="noopener"` for security when using `target="_blank"`:

```html
<a href="https://example.com" target="_blank" rel="noopener">External Link</a>
```

Without `rel="noopener"`, the new page has access to `window.opener`, which can manipulate the original page (tabnapping attack). Modern browsers treat `target="_blank"` as `rel="noopener"` by default in some contexts, but explicitly adding it is safer and more compatible.

Use `target="_blank"` sparingly. Opening new tabs can be disorienting for users, especially those using assistive technology. The general guideline: open external links (links to other domains) in a new tab, and keep internal links in the same tab.

**`_parent`** and **`_top`** — only relevant for pages displayed in `<iframe>` elements. `_parent` opens in the parent frame, `_top` opens in the top-level window (breaking out of all frames).

### The `rel` Attribute

The `rel` attribute defines the relationship between the current page and the linked resource. It is used for both `<a>` links and `<link>` elements in the `<head>`.

Common values for `<a>`:

| Value | Meaning |
|---|---|
| `noopener` | Prevents the new page from accessing `window.opener` |
| `noreferrer` | Prevents the browser from sending the `Referer` header |
| `nofollow` | Tells search engines not to pass link equity (SEO) |
| `ugc` | User-generated content (comments, forum posts) |
| `sponsored` | Paid or sponsored links |
| `external` | Indicates the link points to an external site |
| `tag` | The link points to a tag or keyword |
| `help` | The link points to a help resource |
| `prev` / `next` | Previous and next pages in a sequence (pagination) |
| `license` | The linked page is the license for the current content |
| `author` | The linked page is about the author |

Multiple values are separated by spaces:

```html
<a href="https://sponsored-post.example.com" rel="nofollow sponsored">Sponsored Post</a>
<a href="https://external-site.com" target="_blank" rel="noopener noreferrer">External</a>
```

Search engines use `rel` attributes to understand link relationships. Google treats `nofollow`, `ugc`, and `sponsored` as hints for crawling and indexing, not strict rules.

### Download Links

Adding the `download` attribute turns a link into a file download prompt instead of navigating to the resource:

```html
<a href="/files/manual.pdf" download>Download Manual (PDF)</a>
```

You can specify a custom filename:

```html
<a href="/files/manual.pdf" download="product-manual.pdf">Download Manual</a>
```

Without `download`, clicking the link navigates to the PDF (or opens it in the browser if the browser supports it). With `download`, the browser prompts the user to save the file.

The `download` attribute only works for same-origin URLs. Cross-origin downloads fall back to navigation unless the server sends the appropriate `Content-Disposition` header.

### Email and Telephone Links

**Email links** use the `mailto:` scheme:

```html
<a href="mailto:hello@example.com">Send Email</a>
```

Additional parameters (subject, body, cc, bcc) can be added:

```html
<a href="mailto:hello@example.com?subject=Hello&body=I%20have%20a%20question">
    Send Email with Subject
</a>
```

Parameters must be URL-encoded (spaces as `%20`, etc.). Multiple parameters use `&`:

```html
<a href="mailto:hello@example.com?cc=admin@example.com&subject=Feedback">
    Send Feedback
</a>
```

Email links open the user's default email client. On mobile devices, this works well. On desktop, it may open a separate email application. Some users prefer webmail — in that case, use a contact form instead.

**Telephone links** use the `tel:` scheme:

```html
<a href="tel:+1234567890">Call Us: +1 (234) 567-890</a>
```

On mobile devices, tel links initiate a phone call. On desktop, they may open a VoIP application (Skype, FaceTime) or do nothing.

**SMS links** use the `sms:` scheme:

```html
<a href="sms:+1234567890">Send Text Message</a>
```

These are less commonly used but available for mobile-focused interfaces.

### Navigation Menus

Navigation menus are lists of links wrapped in a `<nav>` element:

```html
<nav aria-label="Main navigation">
    <ul>
        <li><a href="/">Home</a></li>
        <li><a href="/about">About</a></li>
        <li><a href="/services">Services</a></li>
        <li><a href="/contact">Contact</a></li>
    </ul>
</nav>
```

Using `<ul>` for navigation is the standard pattern. Screen readers announce the list and its item count, giving users an overview of the navigation structure. The `<nav>` element identifies the region as navigation.

For single-page navigation with active state indication:

```html
<nav aria-label="Main navigation">
    <ul>
        <li><a href="/" aria-current="page">Home</a></li>
        <li><a href="/about">About</a></li>
        <li><a href="/contact">Contact</a></li>
    </ul>
</nav>
```

The `aria-current="page"` attribute tells screen readers that this link points to the current page. CSS can style it differently:

```css
[aria-current="page"] {
    font-weight: bold;
    color: #0056b3;
    border-bottom: 2px solid #0056b3;
}
```

### Breadcrumb Navigation

Breadcrumbs show the user's location in the site hierarchy:

```html
<nav aria-label="Breadcrumb">
    <ol>
        <li><a href="/">Home</a></li>
        <li><a href="/html">HTML</a></li>
        <li aria-current="page">Links & Navigation</li>
    </ol>
</nav>
```

Using `<ol>` (ordered list) is important because breadcrumbs represent a sequence — the order matters. The last item (current page) has `aria-current="page"` instead of a link.

Styling breadcrumb separators is typically done with CSS:

```css
nav[aria-label="Breadcrumb"] li + li::before {
    content: "›";
    padding: 0 8px;
    color: #666;
}
```

### Skip Links

Skip links allow keyboard users and screen reader users to bypass repetitive navigation and jump directly to the main content:

```html
<a href="#main-content" class="skip-link">Skip to main content</a>

<nav>
    <!-- navigation links -->
</nav>

<main id="main-content">
    <h1>Main Content</h1>
</main>
```

The skip link is typically hidden off-screen and shown on focus:

```css
.skip-link {
    position: absolute;
    top: -40px;
    left: 0;
    background: #000;
    color: #fff;
    padding: 8px 16px;
    z-index: 100;
}

.skip-link:focus {
    top: 0;
}
```

When the user presses Tab on page load, the skip link is the first focusable element. Pressing Enter jumps focus to `#main-content`, skipping the navigation.

### Link States and Styling

Browsers apply default styles to links based on their state:

| State | Selector | Default style | Meaning |
|---|---|---|---|
| Unvisited | `:link` | Blue, underlined | Link has not been clicked |
| Visited | `:visited` | Purple, underlined | Link has been clicked (in browser history) |
| Hover | `:hover` | Varies | Mouse pointer is over the link |
| Active | `:active` | Red, underlined | Link is being clicked |
| Focus | `:focus` | Outline or ring | Link is focused via keyboard |

A comprehensive link style:

```css
a {
    color: #0056b3;
    text-decoration: underline;
    text-underline-offset: 2px;
}

a:hover {
    color: #003d82;
    text-decoration-thickness: 2px;
}

a:active {
    color: #002244;
}

a:focus-visible {
    outline: 2px solid #0056b3;
    outline-offset: 2px;
    border-radius: 2px;
}

a:visited {
    color: #6b3fa0;
}
```

Best practices for link styling:

- **Underline links** (or provide another clear visual indicator like an icon). Color alone is not sufficient — users with color blindness may not distinguish link text from regular text.
- **Ensure sufficient contrast** against the background for all states.
- **Do not remove `:focus` outlines** without providing an accessible alternative. Removing `outline: none` without adding a focus style makes keyboard navigation impossible.
- **`text-underline-offset`** gives more control over underline positioning, improving readability.

### Accessible Links

Links must be accessible to all users, including those using screen readers, voice control, or keyboard navigation.

**Descriptive link text.** Links should describe their destination. "Click here" and "Read more" are not descriptive:

```html
<!-- Bad -->
<a href="/products/details/42">Click here</a> for product details.

<!-- Bad -->
<a href="/articles/123">Read more</a>

<!-- Good -->
<a href="/products/details/42">View product details for Wireless Headphones</a>

<!-- Good -->
<a href="/articles/123">Read the full article: Getting Started with HTML</a>
```

Screen reader users can pull up a list of all links on the page. If every link says "Click here," the list is useless. Each link should make sense when read out of context.

**Visually hidden text** can provide context without affecting the visual design:

```html
<a href="/articles/123">
    Read more
    <span class="sr-only">about Getting Started with HTML</span>
</a>
```

With CSS:

```css
.sr-only {
    position: absolute;
    width: 1px;
    height: 1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border: 0;
}
```

**`aria-label`** provides an accessible name for links without visible text:

```html
<a href="https://twitter.com/user" aria-label="Follow us on Twitter">
    <svg><!-- Twitter icon --></svg>
</a>
```

**`aria-labelledby`** references another element as the link's accessible name:

```html
<h2 id="card-title">Wireless Headphones</h2>
<p>High-quality wireless headphones with noise cancellation.</p>
<a href="/products/42" aria-labelledby="card-title">View Details</a>
```

**External link indicators.** Warn users when a link opens in a new tab or leads to an external site:

```html
<a href="https://external-site.com" target="_blank" rel="noopener">
    External Site
    <span class="sr-only">(opens in new tab)</span>
</a>
```

CSS can add a visual icon:

```css
a[target="_blank"]::after {
    content: "↗";
    display: inline-block;
    margin-left: 4px;
    font-size: 0.8em;
}
```

### Link Relationships

The `<link>` element (in `<head>`) defines relationships between the current document and external resources. It is not visible on the page but affects browser behavior, SEO, and performance.

```html
<head>
    <!-- Stylesheet -->
    <link rel="stylesheet" href="styles.css">

    <!-- Favicon -->
    <link rel="icon" href="favicon.ico" type="image/x-icon">

    <!-- Canonical URL -->
    <link rel="canonical" href="https://example.com/page">

    <!-- Preload critical resources -->
    <link rel="preload" href="fonts/inter.woff2" as="font" crossorigin>

    <!-- Preconnect to third-party origin -->
    <link rel="preconnect" href="https://analytics.example.com">

    <!-- DNS prefetch -->
    <link rel="dns-prefetch" href="https://cdn.example.com">

    <!-- Alternate language versions -->
    <link rel="alternate" href="https://example.com/fr/" hreflang="fr">

    <!-- RSS feed -->
    <link rel="alternate" type="application/rss+xml" title="Blog" href="/feed.xml">

    <!-- Previous and next in pagination -->
    <link rel="prev" href="/page/1">
    <link rel="next" href="/page/3">
</head>
```

Common `rel` values for `<link>`:

| Value | Purpose |
|---|---|
| `stylesheet` | Links to a CSS file |
| `icon` | Specifies favicon or app icon |
| `canonical` | Preferred URL when duplicates exist (SEO) |
| `preload` | Tells the browser to fetch a resource early |
| `preconnect` | Establishes early connection to an origin |
| `dns-prefetch` | Resolves DNS early for a domain |
| `prefetch` | Hints to fetch a resource for the next navigation |
| `alternate` | Alternate version of the page (language, format) |
| `prev` / `next` | Previous and next pages in a sequence |
| `modulepreload` | Preloads a JavaScript module |
| `manifest` | Links to a web app manifest (PWA) |

The `media` attribute on `<link>` can conditionally load stylesheets:

```html
<link rel="stylesheet" href="print.css" media="print">
<link rel="stylesheet" href="dark.css" media="(prefers-color-scheme: dark)">
```

### Common Mistakes

**"Click here" links.** Links must describe their destination when read out of context. "Click here" and "Read more" provide no information about where the link goes.

```html
<!-- Bad -->
<a href="/products">Click here to see our products</a>

<!-- Good -->
<a href="/products">View our product catalog</a>
```

**Links that do not look like links.** If a link is not visually distinguishable from regular text, users will not know they can click it. Underline links and ensure sufficient color contrast.

**Broken fragment links.** If the target `id` on the page does not match the fragment in the link, clicking does nothing. This is a common maintenance issue when content is reorganized.

```html
<!-- Target id was removed or renamed -->
<a href="#section-3">Go to Section 3</a>
<!-- There is no id="section-3" on the page -->
```

**Empty links.** An `<a>` element with no content or only whitespace:

```html
<a href="/page"></a>
```

These are invisible to screen readers and cannot be activated by keyboard users. If the link has meaning, it must have accessible text content.

**Links that open in new tabs without warning.** Users should be informed when a link opens a new tab. This applies especially to screen reader users who may not notice the new tab opening.

**Multiple links to the same destination with different text.** If two links on the same page point to the same URL, they should have the same accessible name. Inconsistent naming confuses screen reader users who navigate by link lists.

**Using `<div>` or `<span>` as links.** Elements with JavaScript click handlers are not true links. They cannot be opened in new tabs, bookmarked, or indexed by search engines. Always use `<a href="...">` for navigation.

```html
<!-- Bad -->
<div onclick="window.location='/page'" class="fake-link">Page</div>

<!-- Good -->
<a href="/page">Page</a>
```

**External links without `rel="noopener"`.** For `target="_blank"` links, always add `rel="noopener"` to prevent the new page from accessing `window.opener`.

**Non-descriptive link text in icon-only links.** An icon alone is not accessible. Provide `aria-label` or visually hidden text:

```html
<a href="https://twitter.com/user" aria-label="Twitter">
    <img src="twitter-icon.svg" alt="">
</a>
```

The `alt=""` on the image tells screen readers to ignore it — the `aria-label` on the link provides the accessible name.

## Glossary

| Term | Definition |
|------|------------|
| Anchor | The `<a>` element that creates a hyperlink |
| `href` | The attribute specifying the link destination URL |
| Fragment | The part of a URL after `#` that targets a specific `id` |
| Absolute URL | A full URL including protocol and domain |
| Relative URL | A URL relative to the current document or domain root |
| Root-relative URL | A URL starting with `/`, resolved from the domain root |
| Protocol-relative URL | A URL starting with `//`, using the current protocol |
| `target` | Attribute controlling where the link opens |
| `rel` | Attribute defining the relationship with the linked resource |
| `download` | Attribute that triggers a file download instead of navigation |
| `mailto:` | URI scheme for email links |
| `tel:` | URI scheme for telephone links |
| Skip link | A link that jumps to the main content, bypassing navigation |
| Breadcrumb | Navigation showing the page's position in the site hierarchy |
| Tabnapping | An attack where a new tab manipulates the opener window via `window.opener` |
| `noopener` | Prevents `window.opener` access |
| `noreferrer` | Prevents sending the `Referer` HTTP header |
| `nofollow` | Tells search engines not to pass link equity |
| Link state | The condition of a link (unvisited, visited, hover, active, focus) |
| `aria-label` | Attribute that provides an accessible name |
| `aria-current` | Attribute indicating the current item in a set |
| `sr-only` | A CSS class that hides content visually but keeps it accessible to screen readers |

## Quick References

- [MDN: `<a>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/a) — complete reference
- [MDN: `<link>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/link) — complete reference
- [MDN: `rel` Attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes/rel) — all rel values
- [MDN: Creating Hyperlinks](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Creating_hyperlinks) — guide
- [HTML Spec: Links](https://html.spec.whatwg.org/multipage/links.html) — official specification
- [WebAIM: Links and Hypertext](https://webaim.org/techniques/hypertext/) — accessibility guide
- [Google: Link Best Practices](https://developers.google.com/style/link-text) — SEO and UX recommendations
- [Can I Use: `<a>`](https://caniuse.com/links) — browser support for link features

## Next Steps

- [Images & Responsive Images](images.md) — embedding and optimizing images, srcset, picture
- [Tables](tables.md) — tabular data structure and accessibility
- [Meta Tags & Social Sharing](meta-tags.md) — metadata for SEO and social media
- [Semantic HTML & Document Outline](semantic-html.md) — structural meaning in markup
