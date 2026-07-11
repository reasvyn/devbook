# Elements, Tags & Attributes

## Description

Elements, tags, and attributes are the fundamental building blocks of HTML. Every web page is composed of them — understanding how they work, how they nest, and how they behave is essential for writing correct, accessible, and maintainable markup.

## Prerequisites

- [Document Structure](document-structure.md) — the HTML document skeleton
- [HTML: Introduction](intro/index.md) — what HTML is

## Table of Contents

- [Elements vs Tags](#elements-vs-tags)
- [Anatomy of an Element](#anatomy-of-an-element)
- [Void Elements](#void-elements)
- [Attributes](#attributes)
- [Boolean Attributes](#boolean-attributes)
- [Global Attributes](#global-attributes)
- [Custom Data Attributes](#custom-data-attributes)
- [Element Categories](#element-categories)
- [Nesting Rules](#nesting-rules)
- [Case Sensitivity](#case-sensitivity)
- [Quoting Attributes](#quoting-attributes)
- [Self-Closing Tag Syntax](#self-closing-tag-syntax)
- [Common Pitfalls](#common-pitfalls)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Elements vs Tags

Developers often use "element" and "tag" interchangeably, but they have distinct meanings:

A **tag** is the markup syntax — the angle-bracketed keyword that marks the start or end of an element.

```html
<p>      <!-- opening tag -->
</p>     <!-- closing tag -->
```

An **element** is the complete structure — the opening tag, content, and closing tag together.

```html
<p>Hello, world!</p>  <!-- this is the element -->
```

The element is what the browser understands. When the parser reads `<p>Hello, world!</p>`, it creates a single "paragraph element" node in the DOM tree containing the text "Hello, world!". The tags are just delimiters — the element is the actual object.

### Anatomy of an Element

A typical HTML element has three parts:

```
Opening tag        Content        Closing tag
    ↓                 ↓               ↓
<p  class="intro">  Hello world!   </p>
↑                       ↑
Attributes             Element
```

**Opening tag** — the tag name inside angle brackets (`<p>`). If the element has attributes, they appear here, separated by spaces.

**Content** — the text or nested elements between the opening and closing tags. Not all elements have content (see void elements).

**Closing tag** — the tag name preceded by a forward slash (`</p>`). The closing tag must match the opening tag name.

In the DOM, the browser creates a node tree:

```
p element
├── attribute: class="intro"
└── text node: "Hello world!"
```

### Void Elements

Void elements (also called self-closing or empty elements) cannot contain any content. They have an opening tag but no closing tag.

```html
<br>       <!-- line break -->
<hr>       <!-- thematic break -->
<img src="photo.jpg" alt="Photo">
<input type="text" name="username">
<meta charset="utf-8">
<link rel="stylesheet" href="styles.css">
<wbr>      <!-- word break opportunity -->
<area>     <!-- area in an image map -->
<base>     <!-- base URL for relative links -->
<col>      <!-- column in a table -->
<embed>    <!-- external content container -->
<param>    <!-- parameter for an <object> -->
<source>   <!-- media source for <video> or <audio> -->
<track>    <!-- text track for <video> or <audio> -->
```

Void elements are a fixed set defined by the HTML specification. You cannot make a regular element void — for example, `<div />` is treated as an opening `<div>` tag followed by a stray `/` character, not as a self-closing div.

In HTML syntax, void elements are written without a trailing slash:

```html
<!-- HTML5 syntax (correct) -->
<img src="photo.jpg" alt="Photo">
<br>

<!-- XHTML syntax (also works in HTML5 but is unnecessary) -->
<img src="photo.jpg" alt="Photo" />
<br />
```

The slash is optional in HTML5. Most style guides recommend omitting it for consistency with HTML conventions.

### Attributes

Attributes provide additional information about an element. They are written as name-value pairs inside the opening tag, separated from the tag name and each other by spaces.

```html
<a href="https://example.com" target="_blank" rel="noopener">
    Visit Example
</a>
```

General syntax:

```
<tag attribute1="value1" attribute2="value2">
```

Rules for attributes:

- Attribute names are lowercase by convention (XHTML requires lowercase; HTML5 works either way but lowercase is standard)
- Attribute values are enclosed in double quotes (single quotes also work, but double is conventional)
- An element can have multiple attributes, separated by spaces
- The order of attributes does not matter
- An attribute can only appear once on an element (duplicate attributes are ignored — the first occurrence wins)

Common attributes organized by purpose:

**Identification and classification:**

```html
<div id="main-content">...</div>       <!-- unique identifier -->
<div class="container featured">...</div>  <!-- CSS class (space-separated list) -->
```

**Content relationships:**

```html
<a href="/about">About</a>             <!-- hyperlink target -->
<img src="logo.png" alt="Logo">        <!-- image source and alternative text -->
<link rel="stylesheet" href="style.css"> <!-- relationship and resource location -->
```

**Behavior control:**

```html
<button disabled>Submit</button>       <!-- prevents interaction -->
<input type="email" required>          <!-- requires a value -->
<details open>...</details>            <!-- starts in expanded state -->
```

**Internationalization:**

```html
<html lang="en">...</html>             <!-- language -->
<p dir="rtl">نص عربي</p>              <!-- text direction -->
```

**Event handlers (avoid when possible):**

```html
<button onclick="submitForm()">Submit</button>
```

Inline event handlers mix behavior with structure. Modern practice uses JavaScript's `addEventListener` instead, keeping HTML clean.

```javascript
document.querySelector('button').addEventListener('click', submitForm);
```

### Boolean Attributes

Some attributes are boolean — their presence means true and their absence means false. They do not need a value.

```html
<!-- Disabled button -->
<button disabled>Submit</button>

<!-- Required input -->
<input type="email" required>

<!-- Checked checkbox -->
<input type="checkbox" checked>

<!-- Read-only field -->
<input type="text" readonly>

<!-- Hidden element -->
<div hidden>This is hidden</div>
```

If you do write a value for a boolean attribute, it should be the empty string or the attribute name itself:

```html
<!-- These are all equivalent -- all mean "disabled" -->
<button disabled>Submit</button>
<button disabled="">Submit</button>
<button disabled="disabled">Submit</button>
```

Writing `disabled="false"` does NOT enable the button — it is still disabled because the attribute is present. To remove a boolean attribute, you must remove it entirely from the markup.

### Global Attributes

Global attributes can be used on any HTML element. They are defined in the HTML specification and apply universally.

**`id`** — a unique identifier for the element. No two elements on a page can share the same `id`. Used for CSS targeting, JavaScript references, and anchor links.

```html
<h2 id="introduction">Introduction</h2>
<!-- Link to this heading: <a href="#introduction"> -->
```

**`class`** — one or more CSS class names, separated by spaces. Unlike `id`, multiple elements can share the same class.

```html
<p class="note warning">This is a warning note.</p>
```

The element now belongs to both the `note` class and the `warning` class.

**`style`** — inline CSS declarations. Use sparingly — prefer external stylesheets or `<style>` blocks.

```html
<p style="color: red; font-size: 1.2em;">Styled inline</p>
```

**`title`** — advisory information displayed as a tooltip on hover. Not accessible to touch users or keyboard navigators, so do not rely on it for essential information.

```html
<abbr title="HyperText Markup Language">HTML</abbr>
```

**`lang`** — the language of the element's content. Overrides the document-level `lang` on `<html>`.

```html
<p>The French word <span lang="fr">bonjour</span> means hello.</p>
```

**`dir`** — text direction: `ltr`, `rtl`, or `auto`.

```html
<p dir="rtl">هذا نص بالعربية</p>
```

**`hidden`** — a boolean attribute that hides the element from rendering. It is still in the DOM but not displayed.

```html
<div hidden>This content is hidden.</div>
```

**`tabindex`** — controls whether an element is focusable via keyboard navigation and in what order.

```html
<button tabindex="1">First</button>
<button tabindex="3">Third</button>
<button tabindex="2">Second</button>
```

Values:

| Value | Behavior |
|---|---|
| `0` | Focusable, follows the DOM order |
| `-1` | Focusable via script but not via Tab key |
| Positive | Focusable in the specified order (use with caution — breaks expected navigation) |

Avoid positive `tabindex` values. They create a confusing tab order that differs from the visual reading order. Use `tabindex="0"` to make an element focusable in the natural order, or `tabindex="-1"` for programmatic focus management.

**`role`** — ARIA role that defines the element's purpose in the accessibility tree.

```html
<nav role="navigation">...</nav>
```

In most cases, semantic HTML elements have implicit roles. A `<nav>` element already has `role="navigation"` by default. Only use `role` when the element's semantics are not conveyed by the element itself.

**`aria-*`** — a family of attributes that enhance accessibility. They describe the state, property, or label of an element.

```html
<button aria-label="Close dialog" aria-expanded="false">✕</button>
```

**`data-*`** — custom data attributes for storing private information (see next section).

There are approximately 30 global attributes in the HTML specification. The ones listed above are the most commonly used. Others include `accesskey`, `autocapitalize`, `contenteditable`, `draggable`, `enterkeyhint`, `inputmode`, `is`, `itemid`, `itemprop`, `itemref`, `itemscope`, `itemtype`, `spellcheck`, `translate`, and `slot`.

### Custom Data Attributes

Custom data attributes let you store extra information on any HTML element without polluting the class list or using non-standard attributes.

Syntax:

```html
<li data-id="42" data-category="books" data-price="19.99">
    The Pragmatic Programmer
</li>
```

Rules:

- The name must start with `data-`
- Followed by one or more lowercase letters, numbers, or hyphens
- Multiple hyphens are allowed: `data-user-id`, `data-article-type`
- The value is a string; any data type must be serialized

Accessing in JavaScript:

```javascript
const item = document.querySelector('[data-id="42"]');

// Via dataset property
console.log(item.dataset.id);        // "42"
console.log(item.dataset.category);  // "books"
console.log(item.dataset.price);     // "19.99"

// Setting
item.dataset.status = "active";

// This adds: data-status="active" to the element
```

The `dataset` property camelCases the attribute name:

| HTML attribute | `dataset` property |
|---|---|
| `data-id` | `dataset.id` |
| `data-user-id` | `dataset.userId` |
| `data-article-type` | `dataset.articleType` |

Custom data attributes are purely for client-side storage. They do not affect layout, semantics, or accessibility. Screen readers ignore them by default.

Use cases for data attributes:

- Storing identifiers for JavaScript interactions
- Storing state information for UI components
- Storing configuration values for JavaScript plugins
- Passing data from server-rendered HTML to frontend JavaScript

Do not use data attributes for information that should be accessible — use ARIA attributes instead.

### Element Categories

The HTML specification groups elements into categories based on their content model — what type of content they can contain and what they represent.

**Metadata content.** Elements that set up the document's behavior or appearance. They are placed in `<head>` and do not display directly.

```html
<base>, <link>, <meta>, <noscript>, <script>, <style>, <title>
```

**Flow content.** Most elements that can appear in the document body. This is the broadest category.

```html
<a>, <div>, <p>, <span>, <table>, <form>, <img>, <input>, <video>, and many more
```

**Sectioning content.** Elements that define the document's structure and outline.

```html
<article>, <aside>, <nav>, <section>
```

**Heading content.** Elements that define section headings.

```html
<h1>, <h2>, <h3>, <h4>, <h5>, <h6>, <hgroup>
```

**Phrasing content.** Elements that mark up text at the inline level. These are the text-level semantics.

```html
<a>, <abbr>, <b>, <br>, <button>, <cite>, <code>, <em>, <i>, <img>, <input>, <kbd>, <label>, <mark>, <q>, <ruby>, <s>, <samp>, <small>, <span>, <strong>, <sub>, <sup>, <time>, <u>, <var>, <wbr>
```

**Embedded content.** Elements that import external resources.

```html
<audio>, <canvas>, <embed>, <iframe>, <img>, <math>, <object>, <picture>, <svg>, <video>
```

**Interactive content.** Elements specifically designed for user interaction.

```html
<a>, <button>, <details>, <input>, <label>, <select>, <textarea>
```

**Palpable content.** Elements that are non-empty and render visible content. This is a formal category used in specification conformance checks.

Understanding categories helps you know where an element can appear. For example, `<p>` cannot contain other flow content elements like `<div>` or `<table>` because `<p>` only accepts phrasing content.

### Nesting Rules

HTML elements must be properly nested — the inner element must close before the outer element.

```html
<!-- Correct nesting -->
<p>This is <strong>important</strong> text.</p>

<!-- Incorrect nesting (overlapping) -->
<p>This is <strong>important</p></strong>
```

In the incorrect example, the browser will close the `<strong>` when it encounters the `</p>` tag, resulting in unexpected DOM structure. The parser follows the "implied closing" rules defined in the HTML specification.

Specific nesting restrictions:

**`<a>` cannot contain other `<a>` elements.** Links inside links are not allowed. The browser closes the outer `<a>` when it encounters the inner one.

**`<p>` cannot contain block elements.** The `<p>` element's content model only permits phrasing content. A `<div>`, `<h1>`, or `<ul>` inside `<p>` will cause the browser to close the paragraph before the block element.

```html
<!-- Invalid -->
<p>Some text <div>inner content</div> more text</p>

<!-- What the browser does -->
<p>Some text</p><div>inner content</div><p>more text</p>
```

**`<h1>` through `<h6>` cannot contain block elements.** Like `<p>`, headings only accept phrasing content.

**`<li>` can only be a child of `<ul>`, `<ol>`, or `<menu>`.**

**`<dt>` and `<dd>` can only be children of `<dl>`.**

**`<tr>` can only be a child of `<table>`, `<thead>`, `<tbody>`, or `<tfoot>`.**

**`<caption>`, `<colgroup>`, `<thead>`, `<tbody>`, `<tfoot>`, and `<col>` can only be children of `<table>`.**

**`<option>` can only be a child of `<select>`, `<optgroup>`, or `<datalist>`.**

Browsers are lenient with nesting violations — they will attempt to recover and render something meaningful. But relying on this leniency leads to unpredictable behavior across browsers and should be avoided.

### Case Sensitivity

Tag names and attribute names in HTML are case-insensitive:

```html
<!-- These all create the same DOM element -->
<p>text</p>
<P>text</P>
<P>text</p>
```

The HTML specification recommends lowercase for both tag names and attribute names. This is the de facto convention across the web.

Attribute values are case-sensitive or case-insensitive depending on the attribute:

- `id` and `class` values are case-sensitive — `id="Main"` is different from `id="main"`
- `lang` values are case-insensitive — `lang="EN"` is the same as `lang="en"`
- `type` values are case-insensitive — `type="TEXT"` is the same as `type="text"`
- Attribute values for custom data attributes are case-sensitive

The safest approach: use lowercase everywhere.

### Quoting Attributes

Attribute values should be quoted. HTML5 allows unquoted attribute values in some cases, but the rules are complex and easy to get wrong.

Allowed:

```html
<!-- All of these are valid HTML5 -->
<input type="text">           <!-- double quotes -->
<input type='text'>           <!-- single quotes -->
<input type=text>             <!-- unquoted (no spaces or special chars) -->
```

The value must be quoted if it contains spaces, quotes, `=`, `<`, `>`, or `` ` ``:

```html
<!-- Invalid - contains space -->
<div class=container fluid></div>

<!-- Valid - quoted -->
<div class="container fluid"></div>
```

Unquoted values are fragile. One accidental space breaks the page. Always quote attribute values — it is consistent, safer, and the established convention.

Mixing quotes: if the attribute value contains double quotes, use single quotes as the delimiter (or vice versa):

```html
<button onclick='alert("Hello")'>Click</button>
<div title='He said "hello"'>Content</div>
```

Or use HTML entities:

```html
<button onclick="alert(&quot;Hello&quot;)">Click</button>
```

### Self-Closing Tag Syntax

In HTML5, self-closing tags (void elements) do not need a trailing slash:

```html
<!-- HTML5 syntax -->
<br>
<img src="photo.jpg">
<input type="text">
```

The trailing slash is a remnant from XHTML, where XML rules required all tags to close:

```html
<!-- XHTML syntax -->
<br />
<img src="photo.jpg" />
<input type="text" />
```

Both work in HTML5, but the trailing slash on non-void elements does nothing:

```html
<div />
<!-- This is treated as an opening <div> tag followed by a stray "/" character -->
```

The `/` is ignored by the HTML parser on non-void elements. Writing `<div />` in HTML5 does not create a self-closing div — it creates an opening `<div>` tag. The browser will then look for a matching `</div>`.

### Common Pitfalls

**Missing closing tags.** While browsers will infer missing closing tags for some elements (`<p>`, `<li>`, `<td>`), relying on this is error-prone. Always close tags explicitly except for void elements.

**Duplicate IDs.** Two elements with the same `id` break anchor links, JavaScript selectors, and accessibility. The browser will only return the first matching element when using `document.getElementById()`.

**Spaces in `id` or `class` values.** `id` values cannot contain spaces. If you write `id="main content"`, the browser interprets it as an element with `id="main"` and a stray attribute called `content=""`.

**Attributes without values.** See boolean attributes above —` disabled="false"` still disables the element.

**Wrong quote marks.** Smart quotes (`"`, `'`) or backticks are not valid HTML attribute delimiters. Only use straight quotes (`"`, `'`).

```html
<!-- Invalid - smart quotes -->
<a href=“https://example.com”>Link</a>

<!-- Valid - straight quotes -->
<a href="https://example.com">Link</a>
```

**Self-closing non-void elements.** Writing `<script src="app.js" />` does not work as expected. The browser treats it as an opening `<script>` tag and ignores the `/`. The script will not load because there is no `>` after `app.js"`.

**Inline event handlers mixing concerns.** Mixing HTML and JavaScript with `onclick="..."` attributes makes code harder to maintain. Separate behavior from structure.

**Overusing `<div>` and `<span>`.** Generic elements should be a last resort. Use semantic elements (`<nav>`, `<article>`, `<section>`, `<button>`) when they match the content's meaning.

## Glossary

| Term | Definition |
|------|------------|
| Element | The complete HTML construct: opening tag, content, and closing tag |
| Tag | The angle-bracketed keyword that marks the start or end of an element |
| Opening tag | The tag that begins an element (`<p>`) |
| Closing tag | The tag that ends an element (`</p>`) |
| Void element | An element that cannot contain content (`<br>`, `<img>`, `<input>`) |
| Self-closing tag | A void element with optional trailing slash (`<br />`) |
| Attribute | A name-value pair in the opening tag that configures the element |
| Boolean attribute | An attribute whose presence means true, absence means false (`disabled`, `required`) |
| Global attribute | An attribute that can be used on any HTML element (`id`, `class`, `lang`) |
| Data attribute | A custom attribute prefixed with `data-` for storing private information |
| Content model | The type of content an element can contain (phrasing, flow, metadata) |
| Nesting | Placing one HTML element inside another |
| Proper nesting | The inner element closes before the outer element |
| Case sensitivity | Whether uppercase and lowercase characters are treated as distinct |
| ARIA attribute | Attributes that enhance accessibility (`aria-label`, `aria-expanded`) |
| Dataset | The JavaScript API for accessing custom data attributes |
| Inline event handler | An event handler written directly in HTML (`onclick="..."`) |
| DOM tree | The browser's tree representation of the document structure |
| Parser | The browser component that converts HTML text into a DOM tree |
| Implied closing | When the browser closes a tag automatically based on parsing rules |
| Selector | A pattern used to match and select HTML elements (CSS or JavaScript) |
| Quirks mode | Browser rendering mode that emulates old browser bugs |
| Standards mode | Browser rendering mode that follows W3C specifications |
| Serialization | Converting a data structure into a string format for storage or transmission |

## Quick References

- [MDN: HTML Elements Reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Element) — complete list of all HTML elements
- [MDN: HTML Attributes Reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Attributes) — complete list of all HTML attributes
- [MDN: Global Attributes](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes) — attributes available on every element
- [MDN: Data Attributes](https://developer.mozilla.org/en-US/docs/Learn/HTML/Howto/Use_data_attributes) — guide to custom data attributes
- [HTML Spec: Elements and Attributes](https://html.spec.whatwg.org/multipage/dom.html#elements) — official specification
- [HTML Validator](https://validator.w3.org/) — validate your HTML documents
- [Can I Use](https://caniuse.com/) — browser support for HTML features

## Next Steps

- [Block vs Inline Elements](block-vs-inline.md) — how elements display in the document flow
- [Text Content](text-content.md) — headings, paragraphs, lists, quotes, and code
- [Links & Navigation](links.md) — anchors, URLs, and navigation patterns
- [HTML Best Practices](best-practices.md) — writing clean, maintainable HTML
