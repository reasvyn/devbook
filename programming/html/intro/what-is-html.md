# What Is HTML?

## Description

HTML (HyperText Markup Language) is the standard language for creating web pages and web applications. It structures content on the web using elements and tags, forming the backbone of every website a developer builds or interacts with.

## Prerequisites

- [What Is Programming?](../../intro/what-is-programming.md) — basic understanding of how programs work
- Basic computer literacy — files, folders, text editors

## Table of Contents

- [What HTML Is and Is Not](#what-html-is-and-is-not)
- [How HTML Works](#how-html-works)
- [The Document Structure](#the-document-structure)
- [Elements, Tags, and Attributes](#elements-tags-and-attributes)
- [Block vs Inline Elements](#block-vs-inline-elements)
- [Semantic HTML](#semantic-html)
- [HTML and the Browser](#html-and-the-browser)
- [HTML, CSS, and JavaScript](#html-css-and-javascript)
- [Common Misconceptions](#common-misconceptions)
- [HTML Parsing Model](#html-parsing-model)
- [Content Categories](#content-categories)
- [Global Attributes](#global-attributes)
- [A Brief History of DOCTYPE](#a-brief-history-of-doctype)
- [Web Components](#web-components)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What HTML Is and Is Not

HTML is a **markup language**, not a programming language. It describes the structure and meaning of content rather than defining logic or behavior. You cannot write conditional statements, loops, or functions in HTML. It has no runtime, no execution context, and no mutable state.

HTML tells the browser: "This is a heading. This is a paragraph. This is an image. This is a link." It answers _what_ the content is, not _how_ to process it.

HTML is:
- A markup language that annotates text with meaning
- The foundation of every web page
- Rendered by browsers into a visual or auditory interface
- Extensible through attributes and embedded technologies

HTML is not:
- A programming language
- A styling language (that is CSS)
- A scripting language (that is JavaScript)
- A database language
- A general-purpose computing language

### How HTML Works

An HTML document is a plain text file (usually with a `.html` extension) that contains structured content. When a browser loads the file — from a local disk or over HTTP — it parses the text into a tree of nodes called the **DOM (Document Object Model)** and renders it visually.

The process:

```
HTML file → [Parser] → DOM Tree → [Render Engine] → Pixels on screen
```

The parser is lenient by design. Unlike XML or JSON, HTML does not crash on malformed input. Browsers apply error-recovery rules to display something useful even when the markup is broken. This leniency is a double-edged sword: it makes the web resilient but also allows sloppy code to appear to work.

### The Document Structure

Every HTML document has a basic skeleton that the browser expects:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document Title</title>
</head>
<body>
    <!-- Visible content goes here -->
</body>
</html>
```

Breaking it down:

- `<!DOCTYPE html>` — a declaration (not a tag) that tells the browser to render in standards mode. Without it, browsers fall back to "quirks mode," emulating bugs from the 1990s.
- `<html>` — the root element. The `lang` attribute helps screen readers and search engines determine the document language.
- `<head>` — metadata about the document: character encoding, viewport settings, title, links to stylesheets, scripts, and other resources. Nothing in `<head>` is rendered directly.
- `<body>` — all visible content: text, images, videos, forms, and interactive elements.

### Elements, Tags, and Attributes

An **element** is the fundamental building block of HTML. Elements are represented by **tags** — angle-bracketed keywords that mark the start and (optionally) end of an element.

```
Opening tag        Content        Closing tag
    ↓                 ↓               ↓
<p  class="intro"> Hello, world!  </p>
↑                       ↑
Attribute              Element
```

Most elements have an opening tag and a closing tag (`<p>...</p>`). Some elements are **void elements** (self-closing) that cannot contain content:

```html
<br>       <!-- Line break -->
<img src="photo.jpg" alt="A photo">
<input type="text" name="username">
<hr>       <!-- Horizontal rule -->
<meta charset="UTF-8">
```

**Attributes** provide additional information about an element. They are written as name-value pairs inside the opening tag:

```html
<a href="https://example.com" target="_blank" rel="noopener">Visit Example</a>
```

| Attribute | Purpose |
|-----------|---------|
| `href` | The URL the link points to |
| `target="_blank"` | Opens the link in a new tab |
| `rel="noopener"` | Security measure for external links |

Common attributes that apply to most elements:

- `id` — a unique identifier for the element
- `class` — one or more CSS class names for styling
- `style` — inline CSS (use sparingly)
- `title` — tooltip text shown on hover
- `data-*` — custom data attributes for JavaScript

### Block vs Inline Elements

HTML elements are categorized by how they occupy space in the layout:

**Block-level elements** start on a new line and take up the full available width by default. They create a "block" in the document flow.

```html
<div>Block 1</div>
<div>Block 2</div>
<!-- Rendered as two separate lines -->
```

Common block elements: `<div>`, `<p>`, `<h1>` through `<h6>`, `<ul>`, `<ol>`, `<li>`, `<section>`, `<article>`, `<header>`, `<footer>`, `<nav>`, `<main>`, `<aside>`, `<blockquote>`, `<pre>`, `<table>`, `<form>`.

**Inline elements** do not start on a new line. They flow within the text, wrapping only as needed. They occupy only as much width as their content requires.

```html
<p>This is <strong>bold</strong> and this is <em>italic</em>.</p>
<!-- "bold" and "italic" appear inline within the paragraph -->
```

Common inline elements: `<span>`, `<a>`, `<strong>`, `<em>`, `<b>`, `<i>`, `<code>`, `<img>`, `<br>`, `<input>`, `<label>`, `<button>`.

This distinction affects layout and what elements can nest inside others. For example, inline elements cannot contain block elements in standard HTML parsing — browsers will close the inline element before the block element, which often results in unexpected DOM structures.

### Semantic HTML

Semantic HTML means using elements that describe the meaning of their content, not just its appearance. Instead of wrapping everything in `<div>` tags, you use meaningful elements:

| Non-semantic | Semantic | Purpose |
|---|---|---|
| `<div class="header">` | `<header>` | Introductory content or navigation |
| `<div class="nav">` | `<nav>` | Navigation links |
| `<div class="main">` | `<main>` | Primary content (unique per page) |
| `<div class="article">` | `<article>` | Self-contained composition |
| `<div class="section">` | `<section>` | Thematic grouping of content |
| `<div class="aside">` | `<aside>` | Tangentially related content |
| `<div class="footer">` | `<footer>` | Closing information (author, copyright) |

Benefits of semantic HTML:

- **Accessibility** — screen readers and assistive technologies use semantic elements to navigate pages. A blind user can jump from `<nav>` to `<main>` to `<footer>` without guessing.
- **SEO** — search engines give more weight to content in semantic elements. A `<h1>` tells Google what the page is about; a `<div>` styled to look like a heading does not.
- **Maintainability** — semantic documents are easier for developers to read and understand. You can scan the structure without reading every line.
- **Future-proofing** — browsers evolve. Semantic elements are more likely to receive native browser features (like built-in search or reader mode) than generic divs.

### HTML and the Browser

The browser is the runtime environment for HTML. When a user visits a URL, the browser:

1. **Fetches the HTML** — sends an HTTP request to the server and receives the HTML document
2. **Parses the HTML** — converts the text into a DOM tree
3. **Fetches resources** — downloads CSS, JavaScript, images, fonts, and other assets referenced in the HTML
4. **Builds the render tree** — combines the DOM with CSS styles to compute the visual layout
5. **Paints** — renders pixels to the screen

```
URL → HTTP GET → HTML → DOM Tree → Render Tree → Paint
                       ↗           ↗
               External CSS    External JS (may modify DOM)
```

JavaScript can modify the DOM after the page loads, which triggers reflows (recalculating layout) and repaints (redrawing pixels). These operations are expensive — excessive DOM manipulation is a common cause of slow web pages.

### HTML, CSS, and JavaScript

HTML is one leg of a three-legged stool that forms the web platform:

| Technology | Role | Analogy |
|---|---|---|
| HTML | Structure and content | The skeleton |
| CSS | Presentation and layout | The skin and clothes |
| JavaScript | Behavior and interaction | The muscles and brain |

Each technology has a distinct responsibility. Mixing them — for example, using JavaScript to set styles, or using HTML attributes (`<font>`, `<center>`) for layout — leads to code that is harder to maintain, debug, and scale.

Best practice: use HTML for content structure, CSS files for visual presentation, and JavaScript files for behavior. Link them together through the HTML document:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <h1>Hello</h1>
    <script src="script.js"></script>
</body>
</html>
```

The separation of concerns makes the codebase modular. A designer can tweak CSS without touching HTML. A developer can add behavior in JavaScript without altering the markup.

### Common Misconceptions

**"HTML is easy, so it is not important."** HTML is easy to start with, but mastering it — understanding semantics, accessibility, performance, and cross-browser behavior — takes time and experience. Most web accessibility issues trace back to incorrect HTML.

**"You should learn JavaScript first."** HTML is the entry point to web development. Without HTML, there is nothing for CSS to style or JavaScript to manipulate. Learn them in order: HTML, then CSS, then JavaScript.

**"Divs are fine for everything."** Divs work but they are semantically meaningless. Screen readers cannot distinguish a navigation from a footer when both are `<div>`s. Use semantic elements.

**"HTML5 works everywhere."** HTML5 features are well-supported in modern browsers, but older browsers (Internet Explorer 11 and below) do not support some elements and APIs. Polyfills and progressive enhancement address this gap.

**"HTML is only for web pages."** HTML is used beyond websites: email newsletters, e-book formats (EPUB), mobile app frameworks (Ionic, React Native Web), desktop app frameworks (Electron), and documentation systems (MDX, Storybook).

### HTML Parsing Model

The HTML parser is one of the most complex parsers in modern software. Unlike XML, which fails on malformed input, HTML implements an error-tolerant algorithm defined in the HTML specification.

The parsing algorithm has twelve states including:

1. **Initial state** — the parser looks for content before `<html>` (whitespace, comments, or DOCTYPE)
2. **Before DOCTYPE state** — processes `<!DOCTYPE html>` declaration
3. **Before head state** — expects `<head>` or implicit head creation
4. **In head state** — processes `<head>` content
5. **After head state** — expects `<body>` or implicit body creation
6. **In body state** — processes most visible content
7. **After body state** — handles content after `</body>`
8. **After after body state** — handles content after `</html>`

The parser also maintains a **stack of open elements** and a **list of active formatting elements** (for reconstructing missing tags like `<b>`, `<i>`). When it encounters a start tag, it may close previously open elements implicitly:

```html
<p>First paragraph
<p>Second paragraph</p>
<!-- Parser: first <p> is implicitly closed when second <p> opens -->
```

The parser handles **adoption agency algorithm** for misnested tags:

```html
<b>Bold <i>Bold and Italic</b> Only Italic?</i>
<!-- Browser: <i> is closed by </b>, then a new <i> is opened -->
```

This complexity exists because the web must render billions of existing pages correctly, even those with invalid markup.

### Content Categories

HTML5 introduced content categories that group elements by their purpose and where they can appear:

| Category | Description | Examples |
|---|---|---|
| **Metadata content** | Documents metadata, styling, behavior | `<base>`, `<link>`, `<meta>`, `<noscript>`, `<script>`, `<style>`, `<title>` |
| **Flow content** | Most elements that go in `<body>` | All phrasing, heading, sectioning, and embedded elements plus `<div>`, `<p>`, `<form>` |
| **Phrasing content** | Text-level inline elements | `<span>`, `<b>`, `<i>`, `<em>`, `<strong>`, `<a>`, `<code>`, `<input>`, `<img>` |
| **Heading content** | Section headings | `<h1>`-`<h6>`, `<hgroup>` |
| **Sectioning content** | Create sections in the document outline | `<article>`, `<aside>`, `<nav>`, `<section>` |
| **Embedded content** | External resources | `<audio>`, `<canvas>`, `<embed>`, `<iframe>`, `<img>`, `<math>`, `<object>`, `<picture>`, `<svg>`, `<video>` |
| **Interactive content** | Elements designed for user interaction | `<a>`, `<button>`, `<details>`, `<input>`, `<label>`, `<select>`, `<textarea>` |
| **Palpable content** | Elements that render visible content | Non-void elements that are not hidden |

Understanding categories helps determine valid nesting:

```html
<!-- Valid: phrasing content inside flow content -->
<p>This is <strong>valid</strong> HTML.</p>

<!-- Invalid: flow content inside phrasing content -->
<span><p>Paragraph inside span is invalid</p></span>

<!-- Valid: interactive content with conditions -->
<a href="#">
    <span>This is valid (span is not interactive)</span>
</a>

<!-- Invalid: nested interactive content -->
<a href="#"><button>Nested interactive</button></a>
```

### Global Attributes

Global attributes are available on every HTML element:

```html
<!-- Core attributes -->
<div id="unique-id" class="container highlighted" style="color: red;">
```

| Attribute | Purpose | Example |
|---|---|---|
| `id` | Unique identifier for fragment links, CSS, JS | `id="main-header"` |
| `class` | CSS class name(s), space-separated | `class="card featured"` |
| `style` | Inline CSS declarations | `style="display: none;"` |
| `title` | Advisory tooltip information | `title="More information"` |
| `lang` | Language of element content | `lang="en"` |
| `dir` | Text direction (`ltr`, `rtl`, `auto`) | `dir="rtl"` |
| `hidden` | Element is not yet or no longer relevant | `hidden` |
| `tabindex` | Tab order for keyboard navigation | `tabindex="0"` |
| `data-*` | Custom data attributes for scripting | `data-user-id="123"` |
| `aria-*` | ARIA accessibility attributes | `aria-label="Close"` |
| `role` | ARIA role for assistive technology | `role="navigation"` |
| `slot` | Web component slot assignment | `slot="header"` |
| `part` | CSS shadow part for styling web components | `part="heading"` |
| `exportparts` | Export shadow parts to outer styles | `exportparts="heading: title"` |
| `inert` | Makes element unfocusable and invisible to assistive tech | `inert` |
| `popover` | Declares element as a popover | `popover="auto"` |
| `spellcheck` | Enable/disable spell checking | `spellcheck="false"` |
| `contenteditable` | Element content is editable by the user | `contenteditable="true"` |
| `translate` | Element content should be translated | `translate="no"` |
| `autocapitalize` | Control automatic capitalization | `autocapitalize="sentences"` |
| `inputmode` | Virtual keyboard type hint | `inputmode="numeric"` |
| `enterkeyhint` | Enter key label hint on virtual keyboards | `enterkeyhint="done"` |

Custom data attributes (`data-*`) are particularly powerful for storing data without non-standard attributes:

```html
<article
    data-id="42"
    data-author="Jane"
    data-published="2025-03-15"
    data-tags='["html", "css", "javascript"]'
>
```

```javascript
const article = document.querySelector('article');
console.log(article.dataset.id);       // "42"
console.log(article.dataset.author);   // "Jane"
console.log(JSON.parse(article.dataset.tags)); // ["html", "css", "javascript"]
```

### A Brief History of DOCTYPE

The `<!DOCTYPE html>` declaration has a rich history tied to browser evolution:

**The DOCTYPE Switch (1998-2005)**: Before standards, browsers had two modes:
- **Quirks mode** — emulated bugs of IE4/5 and Navigator 4
- **Standards mode** — followed W3C specifications correctly

The DOCTYPE acts as a trigger. A correct DOCTYPE puts the browser in standards mode; missing or incorrect DOCTYPE triggers quirks mode.

```html
<!-- Standards mode (modern) -->
<!DOCTYPE html>

<!-- Quirks mode triggers -->
<!-- No DOCTYPE -->
<!-- Old HTML4 DOCTYPEs were designed to trigger standards mode -->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">

<!-- XHTML DOCTYPEs -->
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```

**The HTML5 simplification (2014)**: The `<!DOCTYPE html>` is the shortest DOCTYPE that triggers standards mode. It is case-insensitive and works in every browser from IE6 onwards. It is not an HTML tag — it is a special marker token for the parser.

**Limited quirks mode**: A third mode exists for transitional DOCTYPEs, known as "almost standards mode." Firefox calls this "limited quirks mode." It only differs from full standards mode in how it handles images inside table cells.

### Web Components

Web Components are a set of browser APIs that allow creating reusable custom HTML elements:

**Custom Elements** — define new HTML elements with JavaScript:

```javascript
class UserCard extends HTMLElement {
    constructor() {
        super();
        this.attachShadow({ mode: 'open' });
    }

    connectedCallback() {
        const name = this.getAttribute('name') || 'User';
        const avatar = this.getAttribute('avatar') || '';
        this.shadowRoot.innerHTML = `
            <style>
                .card { border: 1px solid #ddd; padding: 1rem;
                        border-radius: 8px; display: flex; gap: 1rem; }
                .name { font-weight: bold; }
            </style>
            <div class="card">
                <img src="${avatar}" alt="${name}" width="50" height="50">
                <div class="name">${name}</div>
                <slot></slot>
            </div>
        `;
    }
}

customElements.define('user-card', UserCard);
```

```html
<user-card name="Jane Doe" avatar="jane.jpg">
    <p>Frontend Developer</p>
</user-card>
```

**Shadow DOM** — encapsulated DOM subtree that cannot be accidentally styled or traversed from outside:

```html
<!-- Shadow DOM creates a boundary -->
<user-card>
    <!-- Light DOM (visible outside) -->
    <span slot="role">Developer</span>
    <!-- Shadow DOM (hidden, encapsulated) -->
</user-card>
```

Benefits of Shadow DOM encapsulation:
- Styles from the outside page do not leak into the component
- Styles from the component do not affect the outside page
- IDs and class names inside Shadow DOM do not conflict with the page
- The component is truly self-contained

**HTML Templates** — declarative fragments rendered via JavaScript:

```html
<template id="tooltip-template">
    <style>
        .tooltip { background: #333; color: #fff; padding: 0.5rem;
                   border-radius: 4px; position: absolute; }
    </style>
    <div class="tooltip">
        <slot name="content">Default tooltip</slot>
    </div>
</template>
```

**Slots** — placeholders for light DOM content to be projected into Shadow DOM:

```html
<!-- Component definition -->
<template id="modal-template">
    <div class="modal-backdrop">
        <div class="modal-content">
            <slot name="header"></slot>
            <slot name="body"></slot>
            <slot name="footer"></slot>
        </div>
    </div>
</template>

<!-- Usage -->
<my-modal>
    <h2 slot="header">Confirm Delete</h2>
    <p slot="body">Are you sure you want to delete this record?</p>
    <button slot="footer">Cancel</button>
</my-modal>
```

Web Components work across frameworks — a component built with the native API can be used in React, Vue, Angular, or plain HTML. Browser support is universal (Chrome, Firefox, Safari, Edge all support the APIs since 2020).

### Lifecycle Callbacks

Custom elements have lifecycle hooks:

| Callback | Triggered when |
|---|---|
| `constructor()` | Element is created (before attaching to DOM) |
| `connectedCallback()` | Element is inserted into the DOM |
| `disconnectedCallback()` | Element is removed from the DOM |
| `attributeChangedCallback(name, oldValue, newValue)` | Observed attribute changes |
| `adoptedCallback()` | Element is moved to a new document |

```javascript
class MyButton extends HTMLElement {
    static get observedAttributes() {
        return ['disabled', 'label'];
    }

    constructor() { super(); }

    connectedCallback() {
        console.log('Button added to page');
        this.render();
    }

    attributeChangedCallback(name, oldVal, newVal) {
        if (oldVal !== newVal) this.render();
    }

    render() {
        this.innerHTML = `<button ${this.hasAttribute('disabled') ? 'disabled' : ''}>
            ${this.getAttribute('label') || 'Click me'}
        </button>`;
    }
}
customElements.define('my-button', MyButton);
```

## Learning Tips

- **Validate your HTML.** Use the W3C validator (validator.w3.org) regularly. Most accessibility issues and layout bugs trace back to invalid or non-semantic HTML.
- **Use semantic elements by default.** Reach for `<article>`, `<section>`, `<nav>`, and `<header>` before `<div>`. A div should be your last choice, not your first.
- **Inspect the DOM in DevTools.** The Elements panel shows the parsed DOM tree, not the source text. This helps understand how the parser handles your markup.
- **Write HTML by hand first.** Before reaching for a framework or generator, write the HTML structure manually. This builds understanding of semantics, nesting, and accessibility.
- **Understand the difference between source and DOM.** Browser DevTools show the DOM after parsing and JavaScript modifications. The original source may differ from what the browser renders.
- **Test with assistive technology.** Use a screen reader (NVDA on Windows, VoiceOver on Mac) to experience your page as a visually impaired user would.
- **One `<h1>` per page.** Use a single `<h1>` for the page title and nest `<h2>`-`<h6>` to create a logical document outline.

## Glossary

| Term | Definition |
|---|---|
| HTML | HyperText Markup Language — the standard markup language for web documents |
| Element | A component of an HTML document, defined by a start tag and (usually) an end tag |
| Tag | Angle-bracketed keyword that marks the start or end of an element (`<p>`, `</p>`) |
| Attribute | A modifier on an element that provides additional information (`class`, `id`, `href`) |
| DOM | Document Object Model — the browser's tree representation of an HTML document |
| Void element | An element that cannot contain content (`<br>`, `<img>`, `<input>`) |
| Semantic HTML | Using elements that describe the meaning of their content (`<article>`, `<nav>`) |
| Block element | An element that starts on a new line and takes full width |
| Inline element | An element that flows within text without breaking the line |
| Hyperlink | A clickable reference that navigates to another document or resource |
| Parser | The browser component that converts HTML text into a DOM tree |
| Render tree | The combination of DOM nodes and CSS styles used to compute layout |
| Screen reader | Assistive technology that reads on-screen content aloud |
| Accessibility (a11y) | Designing content usable by people with disabilities |
| SEO | Search Engine Optimization — practices that improve visibility in search results |
| Quirks mode | A browser rendering mode that emulates old browser bugs |
| Standards mode | The rendering mode triggered by `<!DOCTYPE html>` |
| WHATWG | Web Hypertext Application Technology Working Group — maintains the HTML standard |
| Polyfill | Code that provides modern functionality in older browsers |
| Content categories | HTML5 classification system for element types (metadata, flow, phrasing, etc.) |
| Metadata content | Elements that configure document behavior and presentation |
| Flow content | Most elements that can appear inside the `<body>` element |
| Phrasing content | Inline text-level elements that mark up text |
| Sectioning content | Elements that define document sections and headings |
| Embedded content | Elements that import external resources (images, video, iframes) |
| Interactive content | Elements designed for user interaction |
| Global attributes | Attributes available on every HTML element |
| Custom element | User-defined HTML element registered via `customElements.define()` |
| Shadow DOM | Encapsulated DOM subtree attached to an element |
| HTML template | Declarative `<template>` fragment rendered by JavaScript |
| Slot | Placeholder in Shadow DOM that projects light DOM content |
| Web Components | Set of browser APIs for creating reusable custom elements |
| ARIA | Accessible Rich Internet Applications — attributes that enhance accessibility |
| DOCTYPE | Declaration that triggers standards mode in browsers |
| Adoption agency algorithm | Parser algorithm for handling misnested formatting elements |
| Fragment parser | Algorithm for parsing HTML fragments (not full documents) |
| Light DOM | Content inside a custom element that is projected into slots |
| `data-*` attributes | Custom attributes storing private application data |
| Lifecycle callbacks | Custom element hooks: constructor, connected, disconnected, attributeChanged |

## Quick References

- [MDN HTML Documentation](https://developer.mozilla.org/en-US/docs/Web/HTML) — authoritative reference for every HTML element and attribute
- [HTML Living Standard](https://html.spec.whatwg.org/) — the official specification maintained by WHATWG
- [HTML Validator](https://validator.w3.org/) — check whether your HTML conforms to the standard
- [WebAIM: Semantic Structure](https://webaim.org/techniques/semanticstructure/) — guide to accessibility through semantic HTML
- [Can I Use](https://caniuse.com/) — browser support tables for HTML, CSS, and JavaScript features
- [HTML5 Please](https://html5please.com/) — recommendations for using HTML5 features responsibly
- [web.dev: HTML](https://web.dev/learn/html/) — Google's comprehensive HTML learning path
- [HTML Spec: Parsing](https://html.spec.whatwg.org/multipage/parsing.html) — detailed parsing algorithm documentation
- [Web Components MDN](https://developer.mozilla.org/en-US/docs/Web/API/Web_components) — web components guide
- [custom-elements-everywhere.com](https://custom-elements-everywhere.com/) — framework compatibility matrix for web components
- [W3C HTML5 Differences](https://www.w3.org/TR/html5-diff/) — changes from HTML4 to HTML5
- [HTML Content Categories](https://developer.mozilla.org/en-US/docs/Web/HTML/Content_categories) — MDN reference on content categories
- [ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/) — accessible interactive patterns

## Next Steps

- [HTML: Introduction](index.md) — all intro materials including history and philosophy
- [Document Structure](../document-structure.md) — the HTML skeleton
- [Elements & Attributes](../elements-and-attributes.md) — deep dive into elements, attributes, and their proper usage
- [CSS Fundamentals](../../css/intro/what-is-css.md) — styling HTML documents
- [JavaScript Basics](../../javascript/intro/what-is-javascript.md) — adding interactivity to HTML
