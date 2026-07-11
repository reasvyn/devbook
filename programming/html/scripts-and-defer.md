# Script Loading & Integration

## Description

JavaScript brings interactivity to web pages, but how scripts are loaded dramatically affects page performance. Understanding the difference between blocking, defer, async, and module scripts is essential for building fast, interactive web applications.

## Prerequisites

- [Document Structure](document-structure.md) — the `<head>` and `<body>` structure
- [Link Relationships](link-relationships.md) — resource loading strategies

## Table of Contents

- [The `<script>` Element](#the-script-element)
- [Blocking Scripts](#blocking-scripts)
- [Async Scripts](#async-scripts)
- [Defer Scripts](#defer-scripts)
- [Async vs Defer](#async-vs-defer)
- [Module Scripts](#module-scripts)
- [Inline Scripts](#inline-scripts)
- [Script Placement](#script-placement)
- [Dynamic Script Loading](#dynamic-script-loading)
- [Event Listeners for Scripts](#event-listeners-for-scripts)
- [Subresource Integrity](#subresource-integrity)
- [Cross-Origin Scripting](#cross-origin-scripting)
- [Performance Considerations](#performance-considerations)
- [The `<noscript>` Element](#the-noscript-element)
- [Common Mistakes](#common-mistakes)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The `<script>` Element

The `<script>` element embeds or references JavaScript code:

```html
<!-- External script -->
<script src="app.js"></script>

<!-- Inline script -->
<script>
    console.log('Hello from inline script');
</script>
```

Key attributes:

| Attribute | Purpose |
|---|---|
| `src` | URL of external script file |
| `type` | Script type (`text/javascript` default, `module` for ES modules) |
| `async` | Download in parallel, execute as soon as downloaded |
| `defer` | Download in parallel, execute after HTML parsing |
| `integrity` | Subresource Integrity hash |
| `crossorigin` | CORS settings |
| `nomodule` | Fallback for browsers that do not support modules |
| `referrerpolicy` | Referrer policy for script requests |

### Blocking Scripts

A regular `<script>` element without `async` or `defer` blocks HTML parsing:

```html
<p>This text is parsed first.</p>
<script src="large.js"></script>
<p>This text is parsed only after large.js downloads and executes.</p>
```

Blocking behavior:
1. Browser encounters `<script>` tag
2. HTML parsing pauses
3. Script is downloaded (if external)
4. Script is executed
5. HTML parsing resumes

This is why scripts were traditionally placed at the end of `<body>` — to avoid delaying page rendering.

**Render blocking** — the browser will not render anything after the script until it executes. For scripts in `<head>`, the page remains blank until the script downloads and runs.

### Async Scripts

`async` downloads the script in parallel with HTML parsing and executes as soon as it is available:

```html
<script src="analytics.js" async></script>
```

Behavior:
1. Browser encounters `<script async>` tag
2. Script downloads in parallel (non-blocking)
3. HTML parsing continues
4. When download completes, HTML parsing pauses briefly
5. Script executes
6. HTML parsing resumes

**Execution order is not guaranteed** for multiple async scripts. The first script to finish downloading executes first, regardless of declaration order:

```html
<script src="a.js" async></script>
<script src="b.js" async></script>
```

Either `a.js` or `b.js` may execute first. Do not use `async` for scripts that have dependencies on each other.

**When to use `async`:**
- Analytics scripts
- Ads
- Social media widgets
- Any script that is independent of the page and other scripts

### Defer Scripts

`defer` downloads the script in parallel with HTML parsing but delays execution until the HTML is fully parsed:

```html
<script src="app.js" defer></script>
```

Behavior:
1. Browser encounters `<script defer>` tag
2. Script downloads in parallel (non-blocking)
3. HTML parsing continues
4. When HTML parsing is complete, scripts execute in declaration order
5. `DOMContentLoaded` event fires after deferred scripts execute

**Execution order is guaranteed** — deferred scripts execute in the order they appear in the HTML:

```html
<script src="library.js" defer></script>
<script src="app.js" defer></script>
```

`library.js` always executes before `app.js`.

**When to use `defer`:**
- Application scripts that modify the DOM
- Scripts that depend on the full DOM being available
- Scripts with dependencies on other scripts
- Multiple scripts that must execute in order

### Async vs Defer

| Aspect | `async` | `defer` |
|---|---|---|
| Download | Parallel with HTML parsing | Parallel with HTML parsing |
| Execution | Immediately after download | After HTML parsing completes |
| Execution order | Not guaranteed | Declaration order preserved |
| `DOMContentLoaded` | Before or after (depends on load time) | After deferred scripts execute |
| Use case | Independent scripts (analytics, widgets) | Dependent scripts (app code) |

**Visual timeline:**

```
No attribute:
HTML parsing ———[BLOCKED]———————————————>
Script download                        [DL]
Script execution                              [EXEC]

async:
HTML parsing ——————————————————————————————>
Script download         [DL————]
Script execution                 [EXEC]

defer:
HTML parsing ——————————————————————————————>
Script download         [DL————]
Script execution                                    [EXEC]
```

**Decision guide:**

```
Is the script independent (no DOM manipulation, no dependencies)?
    ├── Yes → Use `async`
    └── No  → Use `defer`

Does the script need to execute before other scripts?
    ├── Yes → Use `defer` (ensures order)
    └── No  → Use `async`

Is the script an ES module?
    └── Yes → Use `type="module"` (deferred by default)
```

### Module Scripts

ES modules provide a modern approach to organizing JavaScript:

```html
<script type="module" src="app.mjs"></script>
```

Module scripts are deferred by default (equivalent to `defer`). They:

- Automatically apply strict mode
- Support `import` and `export` statements
- Execute in declaration order (like `defer`)
- Are cached by URL (each module is downloaded once)
- Have their own scope (variables are not global)

```html
<script type="module">
    import { greet } from './utils.mjs';
    greet('World');
</script>
```

**Module script attributes:**

```html
<!-- Deferred by default — no need for async/defer -->
<script type="module" src="app.mjs"></script>

<!-- Async module — executes when downloaded, even if HTML is not fully parsed -->
<script type="module" src="widget.mjs" async></script>

<!-- Fallback for older browsers -->
<script type="module" src="app.mjs"></script>
<script nomodule src="fallback.js"></script>
```

The `nomodule` attribute provides a fallback for browsers that do not support ES modules. Modern browsers download and execute the module script but ignore the `nomodule` script. Older browsers do the opposite.

**Module path resolution:**
- Relative paths must start with `./` or `../`
- Bare specifiers (like `import 'lodash'`) are not supported in browser modules — use import maps

```html
<script type="importmap">
{
    "imports": {
        "lodash": "https://cdn.example.com/lodash/lodash-es.js"
    }
}
</script>
<script type="module">
    import _ from 'lodash';
</script>
```

### Inline Scripts

Inline scripts contain JavaScript directly in the HTML:

```html
<script>
    const name = 'DevBook';
    document.getElementById('title').textContent = name;
</script>
```

Inline scripts are blocking by default (no `src` attribute). They cannot use `async` or `defer` (those attributes only work with `src`).

**Inline module scripts:**

```html
<script type="module">
    import { format } from './utils.mjs';
    document.querySelector('.date').textContent = format(new Date());
</script>
```

**Inline JSON data** — use `type="application/json"` to embed data:

```html
<script id="config" type="application/json">
{
    "apiUrl": "https://api.example.com",
    "debug": false
}
</script>

<script>
    const config = JSON.parse(document.getElementById('config').textContent);
</script>
```

This pattern is commonly used to pass server-side data to client-side JavaScript.

### Script Placement

The placement of `<script>` elements affects perceived page speed:

**In `<head>` (prefer `defer`):**

```html
<head>
    <link rel="stylesheet" href="styles.css">
    <script src="app.js" defer></script>
</head>
```

Deferred scripts in `<head>` start downloading early without blocking rendering. This is the recommended approach for modern web development.

**At the end of `<body>` (traditional approach):**

```html
<body>
    <!-- Page content -->
    <script src="app.js"></script>
</body>
```

This ensures the page content renders before the script executes. However, the script download does not start until the HTML parser reaches the end of `<body>` — delaying execution compared to deferred scripts in `<head>`.

**Comparison:**

| Placement | Download starts | Execution | Blocking |
|---|---|---|---|
| `<head>` no attribute | Immediately | Immediately (blocks rendering) | Yes |
| `<head>` with `defer` | Immediately | After HTML parses | No |
| `<head>` with `async` | Immediately | When downloaded | No (interrupts parsing) |
| End of `<body>` no attribute | After HTML parses | After download | No (already parsed) |

**Bottom line:** use `<script src="app.js" defer></script>` in `<head>` for the best performance.

### Dynamic Script Loading

Scripts can be loaded programmatically with JavaScript:

```javascript
const script = document.createElement('script');
script.src = 'widget.js';
script.async = true;
document.head.appendChild(script);
```

Dynamic script elements are async by default. To make them execute in order:

```javascript
function loadScript(src) {
    return new Promise((resolve, reject) => {
        const script = document.createElement('script');
        script.src = src;
        script.onload = () => resolve(script);
        script.onerror = () => reject(new Error(`Failed to load ${src}`));
        document.head.appendChild(script);
    });
}

// Usage
loadScript('library.js')
    .then(() => loadScript('app.js'))
    .then(() => {
        console.log('Both scripts loaded');
    });
```

**Conditional loading — load scripts only when needed:**

```javascript
if ('IntersectionObserver' in window) {
    // Browser supports IntersectionObserver — load the script
    loadScript('lazy-load.js');
} else {
    // Browser does not support it — load polyfill instead
    loadScript('intersection-observer-polyfill.js');
}
```

**Feature detection before loading:**

```javascript
// Load modern build if browser supports modules
const script = document.createElement('script');
if ('noModule' in HTMLScriptElement.prototype) {
    script.src = 'app.modern.js';
} else {
    script.src = 'app.fallback.js';
}
document.head.appendChild(script);
```

### Event Listeners for Scripts

Script elements fire events during loading:

```javascript
const script = document.createElement('script');

script.onload = function() {
    console.log('Script loaded successfully');
};

script.onerror = function() {
    console.error('Script failed to load');
};

script.src = 'app.js';
document.head.appendChild(script);
```

**Global error handling for dynamically loaded scripts:**

```javascript
window.addEventListener('error', function(event) {
    if (event.target.tagName === 'SCRIPT') {
        console.error('Script failed:', event.target.src);
    }
}, true);
```

### Subresource Integrity

SRI (Subresource Integrity) ensures the loaded script has not been modified:

```html
<script src="https://cdn.example.com/library.js"
        integrity="sha384-oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/uxy9rx7HNQlGYl1kPzQho1wx4JwY8wC"
        crossorigin="anonymous"></script>
```

- `integrity` — base64-encoded cryptographic hash of the file
- `crossorigin="anonymous"` — required for CORS-enabled CDNs

The browser calculates the hash of the downloaded file and compares it to the `integrity` value. If they do not match, the script is not executed.

To generate an SRI hash:

```bash
openssl dgst -sha384 -binary file.js | openssl base64 -A
```

Always use SRI for scripts loaded from third-party CDNs. This prevents supply-chain attacks where a compromised CDN serves modified JavaScript.

### Cross-Origin Scripting

The `crossorigin` attribute controls CORS behavior for script requests:

```html
<!-- No CORS (default for same-origin scripts) -->
<script src="app.js"></script>

<!-- Anonymous CORS (no credentials sent) -->
<script src="https://cdn.example.com/app.js" crossorigin="anonymous"></script>

<!-- With credentials -->
<script src="https://app.example.com/app.js" crossorigin="use-credentials"></script>
```

When a script from a different origin fails, the browser reports "Script error" without details. Adding `crossorigin="anonymous"` and a CORS header on the server gives access to the full error message:

```javascript
window.addEventListener('error', function(event) {
    console.log(event.message);       // "Script error." without crossorigin
    console.log(event.filename);      // empty without crossorigin
});
```

### Performance Considerations

**Minimize the number of scripts.** Each script is an HTTP request (or cache lookup). Bundle multiple scripts into one using a build tool (Webpack, Rollup, Vite).

**Use `defer` or `async` for all external scripts.** Blocking scripts delay page rendering.

**Preload critical scripts.** For scripts that are needed early but defined later in the HTML:

```html
<link rel="preload" href="critical.js" as="script">
```

**Code-split large applications.** Load only the JavaScript needed for the current page:

```javascript
// Dynamic import — loaded on demand
button.addEventListener('click', async () => {
    const module = await import('./heavy-module.js');
    module.init();
});
```

**Use `modulepreload` for ES modules:**

```html
<link rel="modulepreload" href="app.mjs">
```

This downloads and compiles the module before it is imported.

**Remove unused JavaScript.** Use coverage tools in DevTools to find unused code.

**Consider the impact on Core Web Vitals.** Large scripts delay Time to Interactive (TTI) and First Input Delay (FID). Split and defer non-critical code.

### The `<noscript>` Element

The `<noscript>` element provides content for browsers with JavaScript disabled:

```html
<noscript>
    <p>JavaScript is required for this feature. Please enable JavaScript in your browser settings.</p>
</noscript>
```

When JavaScript is enabled, `<noscript>` content is not rendered. When JavaScript is disabled, it renders normally.

**Use cases for `<noscript>`:**

- Forms that require JavaScript for submission
- Pages that rely on JavaScript for core functionality
- Analytics opt-out messages
- SEO fallback for JavaScript-rendered content

```html
<noscript>
    <meta http-equiv="refresh" content="0; URL=/nojs">
</noscript>
```

Do not use `<noscript>` as a crutch for JavaScript-required features that should work without JS.

### Common Mistakes

**Using blocking scripts in `<head>`.** This delays page rendering. Always use `defer` or `async`.

**Confusing `async` and `defer`.** `async` is for independent scripts; `defer` is for scripts with dependencies.

**Not using `defer` for scripts that manipulate the DOM.** Without `defer`, the script may execute before the target elements exist.

**Forgetting SRI for third-party scripts.** Third-party CDN scripts are a security risk without integrity verification.

**Loading too many scripts.** Each script adds HTTP overhead. Bundle where possible.

**Dynamic scripts without error handling.** Dynamically loaded scripts that fail are silent — always add `onerror` handlers.

**Using `document.write` in deferred scripts.** `document.write` is forbidden in deferred and async scripts.

**Blocking parsing for analytics scripts.** Analytics scripts should use `async` to avoid delaying page rendering.

## Glossary

| Term | Definition |
|------|------------|
| `<script>` | HTML element for embedding or referencing JavaScript |
| `async` | Downloads in parallel, executes immediately when ready |
| `defer` | Downloads in parallel, executes after HTML parsing |
| `type="module"` | Loads as an ES6 module (deferred by default) |
| `nomodule` | Fallback script for browsers without module support |
| `integrity` | SRI hash for script verification |
| `crossorigin` | CORS attribute for cross-origin scripts |
| SRI | Subresource Integrity — cryptographic hash verification |
| ES module | JavaScript module using `import`/`export` syntax |
| Import map | JSON map for resolving bare module specifiers |
| Blocking script | Script that pauses HTML parsing during download/execution |
| `DOMContentLoaded` | Event fired when HTML is fully parsed |
| Dynamic import | `import()` expression for on-demand module loading |
| Code splitting | Dividing JavaScript into chunks loaded on demand |
| Polyfill | Script that adds missing browser functionality |
| `document.write` | Obsolete method that writes to the HTML stream |
| TTI | Time to Interactive — when the page becomes fully interactive |
| FID | First Input Delay — responsiveness metric |

## Quick References

- [MDN: Script Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script) — complete reference
- [MDN: Script Loading Strategies](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script#attributes) — async/defer guide
- [MDN: JavaScript Modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) — module guide
- [MDN: Import Map](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/script/type/importmap) — import map reference
- [MDN: Subresource Integrity](https://developer.mozilla.org/en-US/docs/Web/Security/Subresource_Integrity) — SRI guide
- [HTML Spec: Scripting](https://html.spec.whatwg.org/multipage/scripting.html) — official specification
- [web.dev: Script Loading Strategies](https://web.dev/efficiently-load-third-party-javascript/) — performance guide

## Next Steps

- [IFrames & Embeds](iframes-and-embeds.md) — embedding external content
- [Performance & SEO Basics](performance-seo.md) — optimizing page performance
- [Web Components Introduction](web-components-intro.md) — custom HTML elements
