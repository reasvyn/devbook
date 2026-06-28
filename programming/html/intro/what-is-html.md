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
- [Study Cases](#study-cases)
- [Examples](#examples)
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

## Study Cases

**Case 1: An inaccessible navigation menu.**

A developer builds a navigation bar using `<div>` elements styled with CSS:

```html
<div class="nav">
    <div class="nav-item" onclick="goHome()">Home</div>
    <div class="nav-item" onclick="goAbout()">About</div>
    <div class="nav-item" onclick="goContact()">Contact</div>
</div>
```

A screen reader user tabs through the page. The navigation items do not appear in the tab order (they are not focusable by default). There is no semantic structure to navigate past the menu. The user cannot tell these are navigation links.

With semantic HTML:

```html
<nav aria-label="Main navigation">
    <ul>
        <li><a href="/">Home</a></li>
        <li><a href="/about">About</a></li>
        <li><a href="/contact">Contact</a></li>
    </ul>
</nav>
```

Now the navigation is announced by screen readers, the links appear in the tab order, keyboard users can navigate with Tab and Enter, and the structure is clear to anyone reading the markup.

**Case 2: An article that is not a document.**

A blog page uses `<div>` for the entire layout:

```html
<div class="page">
    <div class="header">My Blog</div>
    <div class="content">
        <div class="post">
            <div class="title">How HTML Works</div>
            <div class="body">...</div>
        </div>
    </div>
    <div class="footer">Copyright 2025</div>
</div>
```

Search engines see a flat structure with no hierarchy. The `<h1>` is missing, so the page lacks a clear topic signal. Screen readers cannot differentiate the article from the sidebar.

With semantic HTML:

```html
<header>
    <h1>My Blog</h1>
</header>
<main>
    <article>
        <h2>How HTML Works</h2>
        <p>...</p>
    </article>
</main>
<footer>
    <small>Copyright 2025</small>
</footer>
```

The `<article>` element tells search engines and assistive technology that this is a self-contained piece of content. The `<h1>` and `<h2>` create a clear heading hierarchy. The `<main>` element marks where the primary content begins.

## Examples

**Example 1: Minimal valid HTML document.**

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>Minimal Page</title>
</head>
<body>
    <h1>Hello, World!</h1>
    <p>This is a paragraph.</p>
</body>
</html>
```

This is the smallest HTML document that renders something useful. It validates, it degrades gracefully, and it sets a good foundation.

**Example 2: Hyperlinks and images.**

```html
<a href="https://developer.mozilla.org/en-US/docs/Web/HTML">
    <img src="html-logo.svg" alt="HTML logo" width="100" height="100">
    Learn HTML on MDN
</a>
```

The image sits inside the link. Clicking the image or the text navigates to the MDN HTML documentation. The `alt` attribute provides a description for screen readers and shows when the image fails to load.

**Example 3: A simple form.**

```html
<form action="/submit" method="post">
    <label for="email">Email address:</label>
    <input type="email" id="email" name="email" required>

    <label for="message">Message:</label>
    <textarea id="message" name="message" rows="4"></textarea>

    <button type="submit">Send</button>
</form>
```

The `<label>` elements are associated with their inputs via the `for` attribute, which matches the input's `id`. Clicking the label focuses the input. The `required` attribute triggers browser-side validation. The `type="email"` attribute validates the format automatically.

**Example 4: Lists — ordered and unordered.**

Unordered list (bullets):

```html
<ul>
    <li>Apples</li>
    <li>Bananas</li>
    <li>Cherries</li>
</ul>
```

Ordered list (numbers):

```html
<ol>
    <li>Preheat oven to 180°C</li>
    <li>Mix flour and sugar</li>
    <li>Bake for 30 minutes</li>
</ol>
```

Lists are not just for bullet points. Navigation menus, search results, comments, and product listings should all use `<ul>` or `<ol>` because they convey structure to assistive technology.

**Example 5: Tables with proper structure.**

```html
<table>
    <caption>Monthly Savings</caption>
    <thead>
        <tr>
            <th>Month</th>
            <th>Amount</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>January</td>
            <td>$100</td>
        </tr>
        <tr>
            <td>February</td>
            <td>$80</td>
        </tr>
    </tbody>
</table>
```

The `<caption>` provides a description. `<thead>` and `<tbody>` group rows semantically. `<th>` marks header cells, which screen readers announce as row or column labels.

## Glossary

| Term | Definition |
|------|------------|
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
| Attribute | A name-value pair in the opening tag that configures the element |
| Parser | The browser component that converts HTML text into a DOM tree |
| Render tree | The combination of DOM nodes and CSS styles used to compute layout |
| Screen reader | Assistive technology that reads on-screen content aloud |
| Accessibility (a11y) | Designing content usable by people with disabilities |
| SEO | Search Engine Optimization — practices that improve visibility in search results |
| Quirks mode | A browser rendering mode that emulates old browser bugs |
| Standards mode | The rendering mode triggered by `<!DOCTYPE html>` |
| WHATWG | Web Hypertext Application Technology Working Group — maintains the HTML standard |
| Polyfill | Code that provides modern functionality in older browsers |

## Quick References

- [MDN HTML Documentation](https://developer.mozilla.org/en-US/docs/Web/HTML) — authoritative reference for every HTML element and attribute
- [HTML Living Standard](https://html.spec.whatwg.org/) — the official specification maintained by WHATWG
- [HTML Validator](https://validator.w3.org/) — check whether your HTML conforms to the standard
- [WebAIM: Semantic Structure](https://webaim.org/techniques/semanticstructure/) — guide to accessibility through semantic HTML
- [Can I Use](https://caniuse.com/) — browser support tables for HTML, CSS, and JavaScript features
- [HTML5 Please](https://html5please.com/) — recommendations for using HTML5 features responsibly

## Next Steps

- [HTML: Introduction](index.md) — all intro materials including history and philosophy
- [Document Structure](../document-structure.md) — the HTML skeleton
- [Elements & Attributes](../elements-and-attributes.md) — deep dive into elements, attributes, and their proper usage
- [CSS Fundamentals](../../css/intro/what-is-css.md) — styling HTML documents
- [JavaScript Basics](../../javascript/intro/what-is-javascript.md) — adding interactivity to HTML
