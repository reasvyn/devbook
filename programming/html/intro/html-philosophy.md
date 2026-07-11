# HTML Philosophy

## Description

HTML is not just a technical specification — it is shaped by a set of design principles that reflect the values of the web: openness, resilience, backward compatibility, and universal access. Understanding these principles helps developers write better code and appreciate why HTML works the way it does.

## Prerequisites

- [What Is HTML?](what-is-html.md) — core HTML concepts
- [History of HTML](history-of-html.md) — how HTML evolved

## Table of Contents

- [The Web's Founding Values](#the-webs-founding-values)
- [Principle 1: Backward Compatibility](#principle-1-backward-compatibility)
- [Principle 2: Error Recovery (The Lenient Parser)](#principle-2-error-recovery-the-lenient-parser)
- [Principle 3: Separation of Concerns](#principle-3-separation-of-concerns)
- [Principle 4: Progressive Enhancement](#principle-4-progressive-enhancement)
- [Principle 5: Accessibility First](#principle-5-accessibility-first)
- [Principle 6: Semantic Markup](#principle-6-semantic-markup)
- [Principle 7: Universal Access](#principle-7-universal-access)
- [Principle 8: Platform, Not Framework](#principle-8-platform-not-framework)
- [Principle 9: The Priority of Constituencies](#principle-9-the-priority-of-constituencies)
- [Principle 10: Don't Break the Web](#principle-10-dont-break-the-web)
- [Tensions Between Principles](#tensions-between-principles)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Web's Founding Values

The web was born in a specific environment — academia, collaboration, and openness. Tim Berners-Lee designed the World Wide Web to be a universal information system, free from proprietary control. This vision shaped every subsequent decision about HTML.

Four core values underlie the web platform:

1. **Universality.** Anyone should be able to publish and access information regardless of hardware, software, network, language, or disability. The web is for everyone.
2. **Decentralization.** No central authority controls who can publish or what they can say. Anyone can create a web page without asking permission.
3. **Resilience.** The web should work even when things go wrong — broken links, malformed HTML, slow networks, or old browsers.
4. **Evolution.** The web must be able to grow and change without breaking what already exists.

These values are not abstract — they are encoded in how HTML works.

### Principle 1: Backward Compatibility

Backward compatibility is the single most important principle in HTML design. Every HTML document ever written should render in every browser ever made. This is not a nice-to-have; it is a hard requirement.

When the WHATWG designed HTML5, they tested every proposed feature against existing web content. If a feature would break an existing page — even a page that used invalid HTML — the feature was redesigned or rejected. The HTML5 parser algorithm was developed by analyzing millions of real-world web pages and reverse-engineering how browsers actually handled them.

The practical implication for developers: you can upgrade your doctype, add semantic elements, and use new HTML5 features incrementally. Your old pages do not break when browsers update. You do not need to rewrite your entire site when a new HTML version is released.

This principle also explains why deprecated elements still work. `<font>`, `<center>`, and `<strike>` are not valid HTML, but browsers will render them forever because removing support would break pages that depend on them.

### Principle 2: Error Recovery (The Lenient Parser)

Most programming languages and formats respond to errors by refusing to proceed. A JSON parser throws an exception on a trailing comma. A JavaScript engine stops at a syntax error. A C compiler rejects programs with type mismatches.

HTML does the opposite. When faced with malformed input, HTML browsers do everything possible to render something useful. The parser applies a complex set of recovery rules — specified in detail in the HTML Living Standard — to produce a DOM tree even from deeply broken HTML.

```html
<!-- This is technically invalid, but every browser renders it -->
<p>This is a paragraph
<p>This is another paragraph
```

Most browsers will implicitly close the first `<p>` when they encounter the second `<p>`, treating them as two separate paragraphs. This is not guesswork — the HTML specification defines exactly how this situation should be handled, and every browser implements the same algorithm.

The lenient parser is a double-edged sword:

**Advantages:**
- The web is remarkably resilient. Pages work even when the markup is sloppy.
- Beginners can write HTML that renders, giving them immediate feedback.
- Legacy content continues to work without modification.

**Disadvantages:**
- Developers can write sloppy code and never notice the problem because "it works in my browser."
- Browsers must carry the burden of complex, slow parsing algorithms to handle edge cases that should not exist.
- The lack of visible errors makes debugging harder. A missing closing tag may cause layout issues that are not obviously connected to the root cause.

The key insight: HTML's error recovery exists because the web serves a diverse, global audience with varying skill levels and tools. A strict parser would exclude people who cannot or do not write perfect markup. The web trades purity for accessibility.

**How the parser works in practice.** The HTML parser algorithm, detailed in section 12.2 of the HTML Living Standard, spans over 100 pages of specification text. It is divided into two phases:

**Tokenization.** The parser reads the input character by character and produces a stream of tokens: DOCTYPE tokens, start tags, end tags, comments, character tokens, and end-of-file tokens. The tokenizer has over 80 distinct states, each handling a specific character sequence — `<!` triggers a comment or doctype attempt, `</` signals an end tag, `<?` is a parse error (treated as a comment in some contexts). When the tokenizer encounters an unexpected character, it switches to a recovery state and consumes input until it can resume normal processing.

**Tree construction.** The token stream feeds into the tree construction algorithm, which builds the DOM node by node. Tree construction maintains several data structures: a stack of open elements (tracking currently open tags), a list of active formatting elements (for managing `<b>`, `<i>`, and similar overlapping tags), and an insertion mode pointer that changes as the parser encounters certain elements like `<select>`, `<table>`, or `<template>`. When a misnested tag like `<b><i>text</b></i>` is encountered, the algorithm automatically closes the inner `<i>` before closing the `<b>`, producing a well-formed DOM tree.

This specification-by-algorithm approach was revolutionary. Previous HTML standards described what valid documents looked like — a grammar. The HTML5 specification described what the parser should do — a program. This shift from descriptive to procedural specification made browser behavior deterministic across all implementations for the first time.

### Principle 3: Separation of Concerns

HTML is responsible for structure and meaning. CSS handles presentation. JavaScript manages behavior. This separation is a foundational principle of web development.

The benefits of maintaining clear boundaries:

**Maintainability.** A developer can change the visual design of a site by editing CSS files without touching the HTML structure. A developer can add interactive behavior by editing JavaScript without changing the markup.

**Accessibility.** Semantic HTML provides meaning that assistive technologies can interpret. CSS does not affect how a screen reader reads a page. JavaScript interactivity can enhance or degrade accessibility depending on how it is implemented.

**Performance.** Browsers can cache CSS and JavaScript files separately from HTML. When a visitor navigates from page to page, only the HTML changes — stylesheets and scripts are reused from cache.

**Team collaboration.** Designers can work on CSS, developers on JavaScript, and content authors on HTML, all in parallel, without merge conflicts.

The principle is violated when developers:
- Use HTML attributes for styling (`<font>`, `<center>`, inline `style=""`)
- Use CSS to create non-decorative structures (using `::before` to insert meaningful content)
- Use JavaScript to set styles that should be in CSS
- Use HTML elements for purely visual effects (`<br>` multiple times for spacing, `<table>` for layout)

### Principle 4: Progressive Enhancement

Progressive enhancement is a strategy that starts with a baseline of core functionality — the essential content and features — and layers enhancements on top for capable browsers.

The three-layer model maps directly to HTML, CSS, and JavaScript:

```
Layer 3: JavaScript behavior       (enhancement)
Layer 2: CSS presentation          (enhancement)
Layer 1: HTML content & structure  (baseline)
```

Each layer has a single responsibility:

- **Layer 1 (HTML)** — the content must be accessible and functional even if CSS and JavaScript fail to load or are disabled. Links must navigate. Forms must submit. Text must be readable.
- **Layer 2 (CSS)** — visual design enhances the experience but is not required for understanding the content. A page should be readable with or without stylesheets.
- **Layer 3 (JavaScript)** — interactivity enhances usability but is not required for completing core tasks. A form should work without JavaScript, but JavaScript can validate input, provide autocomplete, and prevent unnecessary page reloads.

Practical implications:

- Your HTML should work without CSS or JavaScript
- Links should navigate to real URLs (not `javascript:void(0)`)
- Forms should submit to server endpoints (not rely on AJAX)
- Images should have meaningful `alt` text
- Buttons should be `<button>` elements, not `<div>` elements with click handlers

Progressive enhancement is the opposite of "graceful degradation," which starts with the full-featured version and tries to not crash when something is missing. Progressive enhancement builds up from a working baseline, ensuring that every user gets a functional experience.

### Principle 5: Accessibility First

Accessibility in HTML means designing content that can be used by people with disabilities — visual, auditory, motor, cognitive, or speech. It is not an afterthought or a checklist; it is a core design requirement.

The web's universal access goal demands accessibility. If content is not accessible to someone, the web has failed that person.

Key accessibility patterns in HTML:

**Semantic structure.** Screen readers rely on HTML elements to understand page structure. A `<nav>` element tells the screen reader "this is navigation." An `<h1>` tells it "this is the page's main heading." A `<button>` tells it "this is an interactive control."

```html
<!-- Inaccessible -->
<div onclick="submit()" class="button">Submit</div>

<!-- Accessible -->
<button type="submit">Submit</button>
```

**Alternative text.** Every image must have an `alt` attribute that describes its content. Screen readers read the alt text aloud. If the image is purely decorative, use `alt=""` (empty alt) to tell the screen reader to skip it.

```html
<!-- Informative image -->
<img src="chart.png" alt="Bar chart showing revenue growth from $1M to $5M">

<!-- Decorative image -->
<img src="divider-line.png" alt="">
```

**Labeled form controls.** Every input must have an associated label. This can be done with the `<label>` element or with `aria-label`.

```html
<label for="email">Email address</label>
<input type="email" id="email" name="email">
```

**Heading hierarchy.** Headings must be nested in order (`<h1>` then `<h2>` then `<h3>`) without skipping levels. This creates a logical outline that screen readers can navigate.

```html
<!-- Good hierarchy -->
<h1>Page Title</h1>
  <h2>Section</h2>
    <h3>Subsection</h3>
  <h2>Another Section</h2>

<!-- Broken hierarchy (skips h2) -->
<h1>Page Title</h1>
  <h3>Subsection</h3> <!-- Wrong level, confuses screen readers -->
```

**Focus management.** Interactive elements must be focusable and have visible focus indicators. Never remove `outline: none` without providing an alternative focus style.

**Color contrast.** Text must have sufficient contrast against its background. The WCAG (Web Content Accessibility Guidelines) require a minimum contrast ratio of 4.5:1 for normal text and 3:1 for large text.

The Web Content Accessibility Guidelines (WCAG 2.1) define three conformance levels:

| Level | Description |
|---|---|
| A | Minimum — must be satisfied to remove basic barriers |
| AA | Acceptable — most legal and organizational standards require this |
| AAA | Optimal — highest level, may not be achievable for all content |

The HTML specification explicitly references WCAG and requires that all elements and attributes include accessibility considerations.

### Principle 6: Semantic Markup

Semantic markup means choosing HTML elements based on the meaning of the content, not its appearance. The semantic web ideal — articulated by Tim Berners-Lee — envisions a web where machines can understand the meaning of content, not just display it.

Semantic HTML serves several purposes:

**Machine readability.** Search engines, feed readers, and AI agents can extract structured information from semantic markup. An `<article>` element clearly identifies a self-contained piece of content. An `<address>` element tells machines where to find contact information.

**Accessibility.** As discussed in Principle 5, semantic elements are the foundation of web accessibility. Screen readers use them to navigate and announce content.

**Styling hooks.** Semantic elements provide consistent CSS targets without adding extra classes. You can style all `<blockquote>` elements without needing to add `.blockquote` classes.

**Future compatibility.** Semantic elements are more likely to receive native browser features. For example, the `<details>` element provides built-in disclosure widget behavior. The `<dialog>` element provides native modal functionality.

Choosing between semantic and non-semantic markup:

```html
<!-- Non-semantic -->
<div class="navigation">
    <div class="item"><a href="/">Home</a></div>
</div>

<!-- Semantic -->
<nav>
    <ul>
        <li><a href="/">Home</a></li>
    </ul>
</nav>
```

The semantic version conveys the same information more concisely and more meaningfully. Any tool that processes HTML — browser, screen reader, search engine, test framework — can understand what `<nav>` means. A `<div>` with `class="navigation"` requires a human to read the class name and infer the meaning.

### Principle 7: Universal Access

Universal access means the web should work for everyone, regardless of:

- **Device** — desktop, laptop, tablet, phone, smart TV, game console, screen reader, braille terminal
- **Network** — fiber optic, 4G, 3G, satellite, dial-up
- **Ability** — visual, auditory, motor, cognitive, speech
- **Language** — any human language, any writing system
- **Knowledge** — expert, beginner, child, adult
- **Economic circumstances** — high-end hardware, low-end hardware, shared devices

This is the most ambitious aspect of HTML's philosophy, and it is the hardest to achieve. Every design decision must consider whether it excludes someone.

Practical implications:

**Internationalization.** HTML supports the world's writing systems through the `lang` attribute, Unicode encoding, and bidirectional text support (`dir="rtl"` for Arabic, Hebrew).

```html
<html lang="ar" dir="rtl">
<head>
    <meta charset="utf-8">
    <title>مرحباً بالعالم</title>
</head>
<body>
    <p>هذا نص باللغة العربية</p>
</body>
</html>
```

**Bandwidth awareness.** Large resources should not block content. The `loading="lazy"` attribute defers off-screen images. The `async` and `defer` attributes on scripts prevent JavaScript from blocking rendering.

**Input modality.** Interfaces should work with mouse, keyboard, touch, stylus, and voice. A button should be clickable, tappable, focusable with Tab and activated with Enter/Space, and triggerable with voice commands.

**Low-end devices.** JavaScript-heavy SPAs (Single Page Applications) exclude users on low-end phones or slow networks. Server-side rendering and progressive enhancement ensure that core content reaches everyone.

### Principle 8: Platform, Not Framework

HTML is designed to be a **platform** — a stable foundation that others can build upon — not a **framework** that dictates how applications should be structured.

A framework tells developers: "Build your application this way." A platform says: "Here are the building blocks. Build however you like."

This distinction explains several characteristics of HTML:

**Framework-agnostic.** HTML works with any JavaScript framework — React, Vue, Angular, Svelte, or no framework at all. The final output of all these frameworks is HTML, CSS, and JavaScript.

**Library-agnostic.** HTML does not require jQuery, Bootstrap, or any other library. You can build a complete, functional website with nothing but HTML elements. Libraries are optional enhancements.

**Tool-agnostic.** You can write HTML in any text editor. No build tools, transpilers, or bundlers are required. The browser understands raw HTML files.

**Server-agnostic.** HTML does not care what programming language your server uses. A URL can return HTML generated by PHP, Python, Ruby, Java, Go, Rust, Node.js, or any static file server.

The platform approach has a cost: HTML evolves slowly. Adding a new element requires consensus among browser vendors, specification editors, and the developer community. A framework can add a feature in a single release. This tradeoff is intentional — stability and universality matter more than speed.

The platform-vs-framework distinction has concrete consequences for how HTML features are designed:

**Features must be composable.** A platform provides primitives that can be combined in unpredictable ways. HTML elements compose freely: an `<a>` element can contain an `<img>`, which can be wrapped in a `<figure>`, inside an `<article>`, inside a `<section>`. There are no restrictions on most nesting combinations beyond a few semantic rules — for example, an `<a>` cannot contain another interactive `<a>`, and a `<p>` cannot contain block-level elements. Frameworks, by contrast, often impose structural constraints and component hierarchies that dictate application architecture.

**Features must be independently useful.** Every HTML element should work on its own, without requiring companion elements or libraries. A `<button>` functions without CSS or JavaScript. A `<form>` submits data over HTTP without AJAX libraries. A `<details>` element opens and closes natively. A `<video>` element renders controls and plays media without a JavaScript player library. This independence ensures that the platform serves as a reliable baseline — content is always functional, even when layers above it fail.

**Features must degrade gracefully.** New HTML elements should provide value in modern browsers while falling back to something functional in older ones. The `<dialog>` element, for example, is natively modal in supporting browsers, managing focus trapping and overlay stacking automatically. In older browsers, it appears as a positioned `<div>` — visually less sophisticated but still readable and interactive. The `loading="lazy"` attribute on images is ignored by older browsers, which simply load the image eagerly — a slower experience but not a broken one.

**The neutrality contract.** By refusing to prescribe application architecture, HTML maintains neutrality across frameworks and methodologies. React, Vue, Angular, Svelte, and vanilla JavaScript all produce the same target: HTML elements in a DOM tree. This neutrality is why the web platform can evolve independently from the JavaScript framework ecosystem — a new HTML element benefits all frameworks equally the moment browsers ship it.

### Principle 9: The Priority of Constituencies

The WHATWG specification defines a hierarchy of constituencies that should be prioritized when making design decisions:

1. **Users** — people who consume web content
2. **Authors** — people who write HTML
3. **Implementors** — people who build browsers
4. **Specifiers** — people who write the HTML specification
5. **Theoretical purity** — abstract design ideals

This hierarchy means: when there is a conflict between what is theoretically elegant and what works for users, users win. When there is a conflict between implementor convenience and author convenience, authors win. When there is a conflict between specifier preferences and implementor constraints, implementors win.

Examples of this principle in action:

- The `<img>` element was added to HTML even though purists argued that hypertext should not include embedded images. Users wanted images, so images were added.
- The lenient parser exists because strict error handling would punish users for author mistakes. Users should never see an error message because an author forgot a closing tag.
- Some HTML5 features were specified based on what browsers had already implemented, not on what was theoretically best. The specification reflects reality rather than imposing an ideal.

### Principle 10: Don't Break the Web

This is the overarching principle that governs all others. It is sometimes called the "Don't Break the Web" (DBTW) rule.

Concretely, this means:

- No change to HTML can cause an existing page to render differently
- No element or attribute can be removed from the specification (deprecation is allowed, but the behavior must be maintained)
- Every browser must implement the same parsing algorithm so that the same HTML produces the same DOM everywhere
- New features must be designed to degrade gracefully in older browsers

This principle explains why HTML has accumulated so many elements over the years, including ones that are widely considered bad practice (`<font>`, `<center>`, `<blink>`, `<marquee>`). Removing them would break pages. The cost of maintaining support for bad elements is lower than the cost of breaking existing content.

Don't Break the Web also constrains new features. Before adding a new element or attribute, the WHATWG tests whether existing web content uses that name differently. If a significant number of pages use a custom `data-*` attribute or an invented element name that conflicts with a proposed feature, the feature must be renamed or redesigned.

### Tensions Between Principles

The principles do not always agree. Real HTML design involves tradeoffs:

**Leniency vs correctness.** The lenient parser makes HTML resilient but allows bad code to propagate. Should browsers be stricter to encourage better code, or lenient to avoid breaking pages? The answer (leniency wins) reflects the Priority of Constituencies — users should not see errors.

**Backward compatibility vs progress.** Adding new features requires maintaining old ones forever. The complexity of the HTML specification grows with every addition. At some point, the accumulated weight of backward compatibility could slow progress. HTML5's living standard model attempts to manage this by deprecating features without removing them.

**Platform stability vs innovation.** HTML evolves slowly by design. But slow evolution frustrates developers who want new capabilities today. The tension is managed by the layer model: CSS and JavaScript evolve faster than HTML, providing new capabilities without requiring changes to the markup language itself.

**Universality vs specialization.** Designing for everyone means no one gets a perfect experience. A feature that works well on desktop might be awkward on mobile. A feature that is intuitive for experts might confuse beginners. The web platform aims for the broadest possible middle ground.

### Study Cases

**Case 1: `<div>`-Spam and the Rise of Semantic Elements**

In the early 2000s, virtually every website used `<div>` elements with class names for all structural components: `<div class="header">`, `<div class="nav">`, `<div class="content">`, `<div class="footer">`. This pattern worked visually — CSS could style any `<div>` — but it was meaningless to machines. A screen reader could not distinguish a navigation `<div>` from a content `<div>`, because both were identical `<div>` elements differentiated only by class names that human authors chose arbitrarily. HTML5 addressed this by introducing dedicated semantic elements: `<header>`, `<nav>`, `<main>`, `<article>`, `<section>`, `<aside>`, and `<footer>`. These elements express meaning in the tag name itself, making the document structure discoverable by any tool that parses HTML — search engines, screen readers, browser extensions, and AI agents. This case study illustrates how the platform recognizes widespread usage patterns and elevates them to first-class syntax, improving accessibility and machine readability without requiring authors to change their underlying mental model.

**Case 2: The jQuery Era — When Libraries Fill Platform Gaps**

From 2006 to 2015, jQuery was the most widely used JavaScript library on the web, present on over 70% of all websites. jQuery succeeded because the platform had significant gaps: browsers implemented inconsistent DOM APIs, there was no standardized way to select elements by CSS selector (`document.querySelectorAll()` did not exist), the `classList` API was not yet available, and AJAX required verbose `XMLHttpRequest` code. jQuery provided a unified, cross-browser API that "just worked." Over time, HTML5 and its companion specifications filled these gaps natively — `querySelector`, `classList`, `fetch`, and the `dataset` API for custom data attributes — reducing the need for jQuery to the point where modern projects rarely include it. This case demonstrates the dynamic between platform and libraries: libraries flourish where the platform is weak and recede as the platform improves, validating the platform approach of providing stable primitives that others build upon.

**Case 3: Polyfills — Community-Driven Platform Extension**

Polyfills exemplify progressive enhancement at the specification level. When HTML5 introduced `<canvas>`, `<video>`, and semantic elements, developers could not wait years for all browsers to implement them natively. Polyfills — JavaScript libraries that emulate missing browser features — bridged this gap. The `html5shiv` polyfill made HTML5 semantic elements work in Internet Explorer 6-8 by using `document.createElement()` to register unknown elements so they could be styled with CSS. Google's `html5shiv` was included in the HTML5 Boilerplate project and became standard practice for years. This case study shows how the web community self-organizes around platform gaps, enabling adoption years before native implementation reaches all users. Polyfills also create pressure on browser vendors: when a polyfill proves widely used and performant, it signals real developer demand for native implementation.

**Case 4: Mobile Viewports — Proprietary Innovation Becomes Standard**

When the iPhone launched in 2007, most websites were designed for desktop monitors and rendered as tiny, unreadable shrunken pages on the 320px-wide screen. Apple introduced the viewport meta tag as a proprietary extension to iOS Safari:

```html
<meta name="viewport" content="width=device-width, initial-scale=1">
```

This tag instructed the browser to render at the device's actual pixel width rather than scaling down a desktop layout. It was a vendor-specific solution to a universal problem. Other mobile browsers adopted the same meta tag, and it eventually became standardized in the CSS Device Adaptation specification. Today it is considered mandatory for any responsive website. This case illustrates how the platform absorbs successful innovations from any source — a feature does not need to originate from a standards body to become part of the platform. What matters is adoption and interoperability.

### Examples

**Progressive Enhancement — A Form for All Conditions**

This form demonstrates progressive enhancement: baseline HTML guarantees submission, while JavaScript enhances the experience:

```html
<form action="/subscribe" method="POST" id="subscribe-form">
    <label for="email">Email address</label>
    <input type="email" id="email" name="email" required>
    <button type="submit">Subscribe</button>
</form>
<script>
    document.getElementById('subscribe-form')
        .addEventListener('submit', async (e) => {
            e.preventDefault();
            const data = new FormData(e.target);
            await fetch('/api/subscribe', {
                method: 'POST', body: data
            });
        });
</script>
```

With JavaScript enabled, the form submits via AJAX without a page reload. Without JavaScript, it performs a standard HTTP POST to `/subscribe`. Both paths reach the server. The HTML layer guarantees core functionality; JavaScript enhances the user experience by eliminating the full-page refresh.

**Semantic vs Non-Semantic Navigation**

Two approaches to building a breadcrumb trail, with markedly different machine readability:

```html
<!-- Non-semantic: meaning must be inferred from class names -->
<div class="breadcrumbs">
    <span class="crumb"><a href="/">Home</a></span>
    <span class="separator">&gt;</span>
    <span class="crumb"><a href="/products">Products</a></span>
    <span class="separator">&gt;</span>
    <span class="crumb current">Widgets</span>
</div>

<!-- Semantic: meaning is expressed in element and attribute names -->
<nav aria-label="Breadcrumb">
    <ol>
        <li><a href="/">Home</a></li>
        <li><a href="/products">Products</a></li>
        <li aria-current="page">Widgets</li>
    </ol>
</nav>
```

The semantic version uses `<nav>` for navigation context, `<ol>` for ordered hierarchy (breadcrumbs are inherently ordered), `aria-label` for screen reader identification, and `aria-current="page"` to mark the current position in the navigation. The non-semantic version requires humans and automated tools to infer meaning from arbitrary class names and presentation characters (`&gt;`), which is fragile and inaccessible.

**Accessibility-First Interactive Element**

The difference between a native button and a `<div>` pretending to be a button illustrates why platform primitives matter:

```html
<!-- Accessible by default: native button provides semantics, keyboard
     interaction, focus management, and form participation for free -->
<button type="button" onclick="handleClick()">Submit</button>

<!-- Must manually reimplement every browser feature:
     role, tabindex, keyboard handler, ARIA states -->
<div role="button" tabindex="0" onclick="handleClick()"
     onkeydown="if(event.key==='Enter'||event.key===' ')
         handleClick()">
    Submit
</div>
```

The native `<button>` announces itself to screen readers, participates in Tab key navigation, responds to Enter and Space keys, and integrates with form submission — all without any additional code. The `<div>` requires manual implementation of each of these features, and in practice, at least one is usually missed.

## Glossary

| Term | Definition |
|------|------------|
| Progressive enhancement | Building from a working baseline and adding layers of enhancement |
| Graceful degradation | Starting with a full-featured version and degrading when features are missing |
| Backward compatibility | The ability to work with content created for older versions |
| Error recovery | The process of handling malformed input without crashing |
| Lenient parser | HTML's forgiving parsing algorithm that tolerates errors |
| Separation of concerns | Dividing functionality into distinct layers (HTML, CSS, JS) |
| Semantic markup | Using elements that describe meaning, not appearance |
| Universal access | Designing for all people regardless of device, ability, or circumstance |
| Accessibility (a11y) | Making content usable by people with disabilities |
| WCAG | Web Content Accessibility Guidelines — the standard for web accessibility |
| ARIA | Accessible Rich Internet Applications — attributes that enhance accessibility |
| Internationalization (i18n) | Designing for users in any language or region |
| Platform | A foundation that others can build upon (as opposed to a framework) |
| Priority of Constituencies | The hierarchy: users > authors > implementors > specifiers > purity |
| Don't Break the Web (DBTW) | The principle that no change should break existing content |
| Deprecation | Marking a feature as discouraged while maintaining its behavior |
| Tabnapping | A phishing attack using `window.opener` on `target="_blank"` links |
| Living Standard | Continuously updated specification model |
| Polyfill | JavaScript code that provides modern features in older browsers |

## Quick References

- [HTML Design Principles](https://www.w3.org/TR/html-design-principles/) — W3C document on the design principles behind HTML5
- [The Priority of Constituencies](https://www.w3.org/TR/html-design-principles/#priority-of-constituencies) — official hierarchy
- [WCAG 2.1 Quick Reference](https://www.w3.org/WAI/WCAG21/quickref/) — accessibility guidelines reference
- [Inclusive Design Principles](https://inclusivedesignprinciples.org/) — principles for designing inclusive digital products
- [Progressive Enhancement by MDN](https://developer.mozilla.org/en-US/docs/Glossary/Progressive_Enhancement) — MDN glossary entry
- [A Dao of Web Design](https://alistapart.com/article/dao/) — John Allsopp's influential essay
- [Don't Break the Web](https://www.w3.org/2001/tag/doc/leastPower.html) — TAG finding on web evolution

## Next Steps

- [HTML: Introduction](index.md) — all introductory materials
- [What Is HTML?](what-is-html.md) — apply these principles to practical HTML
- [History of HTML](history-of-html.md) — see these principles in historical context
- [Semantic HTML](../semantic-html.md) — putting semantics into practice
- [HTML & Accessibility](../accessibility.md) — building accessible interfaces
