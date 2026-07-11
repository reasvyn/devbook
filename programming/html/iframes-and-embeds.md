# IFrames & Embeds

## Description

The `<iframe>` element embeds another HTML document within the current page. IFrames power embedded content from other domains — maps, videos, payment forms, social media posts, and interactive widgets. Security and performance considerations are critical when embedding third-party content.

## Prerequisites

- [Document Structure](document-structure.md) — the `<head>` and `<body>` structure
- [Script Loading & Integration](scripts-and-defer.md) — JavaScript loading

## Table of Contents

- [The `<iframe>` Element](#the-iframe-element)
- [Iframe Attributes](#iframe-attributes)
- [Sandboxing](#sandboxing)
- [Cross-Origin Communication](#cross-origin-communication)
- [Embedding Third-Party Content](#embedding-third-party-content)
- [Embedded Content Best Practices](#embedded-content-best-practices)
- [The `<embed>` Element](#the-embed-element)
- [The `<object>` Element](#the-object-element)
- [The `<param>` Element](#the-param-element)
- [Performance Impact](#performance-impact)
- [Security Considerations](#security-considerations)
- [Common Mistakes](#common-mistakes)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The `<iframe>` Element

An iframe embeds a full HTML document inside the current page:

```html
<iframe src="https://example.com/map" width="600" height="450"></iframe>
```

Each iframe creates its own browsing context — its own:
- Document (DOM)
- JavaScript execution environment
- CSS scope
- History stack
- Focus and navigation

The embedded document is completely independent unless cross-origin messaging is explicitly established.

### Iframe Attributes

**`src`** — the URL of the embedded document:

```html
<iframe src="/widget.html"></iframe>
```

**`width` and `height`** — dimensions in CSS pixels:

```html
<iframe src="https://maps.example.com" width="600" height="450"></iframe>
```

Always specify dimensions to prevent layout shifts. Use CSS for responsive sizing.

**`title`** — accessible name for the iframe:

```html
<iframe src="https://maps.example.com"
        title="Store location map — 123 Main Street"></iframe>
```

Screen readers announce the title when entering the iframe. Every iframe must have a descriptive title.

**`allow`** — feature policy (controls access to browser APIs):

```html
<iframe src="app.html" allow="geolocation; microphone; camera"></iframe>
```

| Feature | Purpose |
|---|---|
| `geolocation` | Access to location data |
| `microphone` | Access to audio input |
| `camera` | Access to video input |
| `fullscreen` | Allow fullscreen mode |
| `payment` | Allow Payment Request API |
| `autoplay` | Allow autoplay with audio |
| `clipboard-read` | Read clipboard contents |
| `clipboard-write` | Write to clipboard |

**`loading`** — lazy loading for iframes (same as images):

```html
<iframe src="widget.html" loading="lazy"></iframe>
```

| Value | Behavior |
|---|---|
| `eager` | Load immediately (default) |
| `lazy` | Defer loading until near the viewport |

**`referrerpolicy`** — referrer information sent with the iframe request:

```html
<iframe src="https://example.com" referrerpolicy="no-referrer"></iframe>
```

**`name`** — target name for form submissions and links:

```html
<iframe name="results" src="about:blank"></iframe>
```

Links with `target="results"` open in the iframe.

**`srcdoc`** — inline HTML content instead of `src`:

```html
<iframe srcdoc="<p>Hello from inline HTML</p>"></iframe>
```

When both `src` and `srcdoc` are present, `srcdoc` takes precedence. `srcdoc` is useful for generating embed code programmatically.

### Sandboxing

The `sandbox` attribute imposes restrictions on the iframe content:

```html
<iframe src="https://example.com" sandbox></iframe>
```

An empty `sandbox` attribute applies all restrictions. Add space-separated values to lift specific restrictions:

| Value | Allows |
|---|---|
| (none) | All restrictions apply |
| `allow-forms` | Submit forms |
| `allow-scripts` | Run JavaScript |
| `allow-same-origin` | Access the parent's origin (use with caution) |
| `allow-popups` | Open popups |
| `allow-modals` | Show modal dialogs |
| `allow-orientation-lock` | Lock screen orientation |
| `allow-pointer-lock` | Lock the pointer |
| `allow-presentation` | Start a presentation session |
| `allow-top-navigation` | Navigate the top-level window |

**Typical sandbox for untrusted content:**

```html
<iframe src="user-content.html" sandbox="allow-scripts"></iframe>
```

**Sandbox for a trusted widget:**

```html
<iframe src="widget.html" sandbox="allow-scripts allow-same-origin allow-forms"></iframe>
```

**Never use `allow-scripts` without `allow-same-origin`** — this combination allows the embedded document to escape the sandbox by running scripts that can navigate the top-level window. Use `allow-scripts` alone for untrusted content.

### Cross-Origin Communication

**`postMessage()`** enables communication between the parent page and an iframe:

In the parent:

```javascript
const iframe = document.getElementById('myIframe');
iframe.contentWindow.postMessage('Hello iframe!', 'https://example.com');
```

In the iframe:

```javascript
window.addEventListener('message', function(event) {
    // Always check the origin
    if (event.origin !== 'https://parent.example.com') return;

    console.log('Received:', event.data);
});
```

The parent receives messages from the iframe via:

```javascript
window.addEventListener('message', function(event) {
    if (event.origin !== 'https://widget.example.com') return;
    console.log('From widget:', event.data);
});
```

**Security rules for `postMessage`:**
- Always validate `event.origin`
- Never use `event.source` without verifying the origin
- Be specific about target origins — avoid `*`
- Validate the structure of received data
- Use `postMessage` only when necessary — prefer URL parameters for simple data

### Embedding Third-Party Content

**YouTube videos:**

```html
<iframe width="560" height="315" src="https://www.youtube.com/embed/VIDEO_ID"
        title="YouTube video player"
        allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture"
        allowfullscreen>
</iframe>
```

**Google Maps:**

```html
<iframe src="https://www.google.com/maps/embed?pb=LOCATION_ID"
        width="600" height="450" style="border:0;"
        allowfullscreen="" loading="lazy"
        title="Store location on Google Maps">
</iframe>
```

**Vimeo:**

```html
<iframe src="https://player.vimeo.com/video/VIDEO_ID"
        width="640" height="360" style="border:0;"
        allow="autoplay; fullscreen; picture-in-picture"
        allowfullscreen
        title="Vimeo video player">
</iframe>
```

**Social media posts (Twitter, Instagram, Facebook):**

Most social platforms provide embed code via their UI. These typically use iframes loaded from their domains.

```html
<!-- Twitter embed (generated) -->
<blockquote class="twitter-tweet">
    <p lang="en">Tweet content</p>
    &mdash; Author (@handle) <a href="https://twitter.com/...">Date</a>
</blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
```

### Embedded Content Best Practices

**Use `loading="lazy"` for iframes below the fold.** Like images, offscreen iframes do not need to load immediately:

```html
<iframe src="widget.html" loading="lazy" title="Widget"></iframe>
```

**Always set a descriptive `title`.** Screen reader users rely on the title to understand the iframe's purpose.

**Use `sandbox` for untrusted content.** This provides defense-in-depth against malicious embedded content.

**Set `width` and `height` to prevent layout shifts.** Iframes without dimensions cause cumulative layout shift (CLS).

**Consider the performance impact.** Each iframe loads a separate HTML document with its own scripts, styles, and resources. Only embed what is necessary.

**Provide a fallback** for browsers that do not support iframes:

```html
<iframe src="https://example.com/widget" title="Widget">
    <p>Your browser does not support iframes.
       <a href="https://example.com/widget">Open the widget directly</a>.</p>
</iframe>
```

**Use `allow` attribute to restrict features.** Only grant permissions the embedded content needs.

### The `<embed>` Element

The `<embed>` element embeds external content using a browser plugin (deprecated):

```html
<embed src="file.pdf" type="application/pdf" width="600" height="400">
```

`<embed>` is a void element with no closing tag. It supports `src`, `type`, `width`, and `height`.

Most use cases for `<embed>` have been replaced by:
- `<video>` and `<audio>` for media
- `<iframe>` for HTML content
- Native browser PDF viewers

Avoid `<embed>` for new content. Use native HTML elements or `<iframe>` instead.

### The `<object>` Element

The `<object>` element embeds external content with a fallback:

```html
<object data="file.pdf" type="application/pdf" width="600" height="400">
    <p>PDF cannot be displayed.
       <a href="file.pdf">Download the PDF</a>.</p>
</object>
```

Content between the `<object>` tags is the fallback, rendered when the object cannot be displayed.

`<object>` can also embed HTML documents (similar to iframe):

```html
<object data="widget.html" type="text/html" width="400" height="300">
    <p>Widget could not be loaded.</p>
</object>
```

`<object>` is more flexible than `<embed>` (supports fallback content) but both are legacy approaches. Use `<iframe>` for HTML content and `<video>`/`<audio>` for media.

### The `<param>` Element

The `<param>` element sets parameters for `<object>` (obsolete):

```html
<object data="movie.swf" type="application/x-shockwave-flash">
    <param name="quality" value="high">
    <param name="wmode" value="transparent">
</object>
```

`<param>` is only relevant for plugins like Flash, which is deprecated and removed from modern browsers. Do not use `<param>` or `<object>` for new content.

### Performance Impact

Iframes have a significant performance cost:

- **Each iframe loads a new document** — HTML, CSS, JavaScript, fonts, images
- **Parent page memory increases** — the iframe's DOM is stored in memory
- **Scroll performance can degrade** — iframes intercept scroll events
- **Network requests multiply** — each iframe makes its own requests

**Strategies to mitigate iframe performance impact:**

- **Lazy load offscreen iframes** with `loading="lazy"`
- **Remove unnecessary iframes** — consider if the content could be loaded via a JavaScript API instead
- **Use `sandbox` to restrict capabilities** — reduces the need for certain resources
- **Postpone iframe loading** until user interaction:

```html
<div id="widget-container" data-src="https://example.com/widget">
    <button id="loadWidget">Load Widget</button>
</div>

<script>
    document.getElementById('loadWidget').addEventListener('click', function() {
        const container = document.getElementById('widget-container');
        const iframe = document.createElement('iframe');
        iframe.src = container.dataset.src;
        iframe.title = 'Widget';
        iframe.width = '400';
        iframe.height = '300';
        container.appendChild(iframe);
        this.remove(); // Remove the load button
    });
</script>
```

### Security Considerations

**Clickjacking** — an attacker embeds your site in an iframe on a malicious page, tricking users into clicking buttons they cannot see.

Prevent clickjacking by sending the `X-Frame-Options` header:

```
X-Frame-Options: DENY
```

Or the more flexible Content Security Policy:

```
Content-Security-Policy: frame-ancestors 'self';
```

**Always sandbox untrusted iframes:**

```html
<iframe src="user-generated.html" sandbox="allow-scripts" title="User content"></iframe>
```

**Validate `postMessage` origins.** Never process messages from unknown origins.

**Use `allow` to restrict feature access.** Do not give embedded content more permissions than it needs.

**Be careful with `allow-same-origin` in sandbox.** Combining `allow-scripts` with `allow-same-origin` effectively removes the sandbox protection, because scripts can access the parent's origin using `parent.document`.

### Common Mistakes

**Missing `title` attribute.** Every iframe must have a descriptive title for accessibility.

**No `sandbox` on untrusted content.** Iframes from unknown sources should always be sandboxed.

**Oversized iframes** — loading many iframes on a single page. Each iframe is a full document load.

**No lazy loading.** Iframes below the fold should use `loading="lazy"`.

**Missing width and height.** Iframes without dimensions cause layout shifts.

**Using `<embed>` or `<object>` for modern content.** Use `<video>`, `<audio>`, or `<iframe>` instead.

**Allowing `allow-scripts` with `allow-same-origin` for untrusted content.** This removes sandbox protections.

**Not validating `postMessage` origin.** This enables cross-site scripting attacks through messaging.

## Glossary

| Term | Definition |
|------|------------|
| `<iframe>` | Inline frame — embeds another HTML document |
| Sandbox | Security restriction mechanism for iframes |
| `allow` | Feature policy controlling API access for the iframe |
| `srcdoc` | Inline HTML content for the iframe |
| Browsing context | The environment (window, document) of an iframe |
| Cross-origin | Content from a different origin (domain/protocol/port) |
| `postMessage` | API for cross-origin communication |
| Clickjacking | Attack tricking users into clicking elements in a hidden iframe |
| `X-Frame-Options` | HTTP header preventing iframe embedding |
| CSP | Content Security Policy — browser security mechanism |
| Lazy loading | Deferring resource loading until needed |
| `<embed>` | Legacy element for plugin content |
| `<object>` | Generic embedded object with fallback |
| `<param>` | Parameter for an `<object>` element |
| Feature policy | Mechanism to control browser API access |

## Quick References

- [MDN: Iframe Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe) — complete reference
- [MDN: Sandbox](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/iframe#sandbox) — sandbox reference
- [MDN: postMessage](https://developer.mozilla.org/en-US/docs/Web/API/Window/postMessage) — cross-origin messaging
- [MDN: Feature Policy](https://developer.mozilla.org/en-US/docs/Web/HTTP/Feature_Policy) — allow attribute reference
- [MDN: Embed Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/embed) — embed reference
- [MDN: Object Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/object) — object reference
- [HTML Spec: Iframes](https://html.spec.whatwg.org/multipage/iframe-embed-object.html) — official specification
- [web.dev: Iframe Performance](https://web.dev/iframe-lazy-loading/) — lazy loading guide
- [OWASP: Clickjacking Defense](https://cheatsheetseries.owasp.org/cheatsheets/Clickjacking_Defense_Cheat_Sheet.html) — security guide

## Next Steps

- [Semantic HTML & Document Outline](semantic-html.md) — structural meaning in markup
- [Performance & SEO Basics](performance-seo.md) — optimizing page performance
- [Web Components Introduction](web-components-intro.md) — custom HTML elements
