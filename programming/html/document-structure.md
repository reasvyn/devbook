# Document Structure

## Description

Every HTML document follows a predictable skeleton that browsers rely on to render content correctly. Understanding this structure — the doctype, root element, head, and body — is the foundation of writing valid, accessible, and performant web pages.

## Prerequisites

- [HTML: Introduction](intro/index.md) — what HTML is and how it works

## Table of Contents

- [The Minimum Valid Document](#the-minimum-valid-document)
- [The DOCTYPE Declaration](#the-doctype-declaration)
- [The `<html>` Root Element](#the-html-root-element)
- [The `<head>` Section](#the-head-section)
- [The `<body>` Section](#the-body-section)
- [The Document Tree](#the-document-tree)
- [Whitespace in HTML](#whitespace-in-html)
- [Comments](#comments)
- [Character Encoding](#character-encoding)
- [Rendering Modes](#rendering-modes)
- [Validation](#validation)
- [Common Structural Mistakes](#common-structural-mistakes)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Minimum Valid Document

The smallest possible HTML document that a browser will render correctly:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8">
    <title>My Page</title>
</head>
<body>
    <h1>Hello, World!</h1>
</body>
</html>
```

Every HTML page must have four things to be valid:

1. A `<!DOCTYPE html>` declaration
2. A `<html>` root element
3. A `<head>` with `<meta charset="utf-8">` and `<title>`
4. A `<body>` with visible content

Anything beyond this is optional but recommended.

### The DOCTYPE Declaration

The DOCTYPE is a declaration, not an HTML element. It appears before the `<html>` tag and tells the browser which version of HTML the document uses.

```html
<!DOCTYPE html>
```

This single declaration triggers **standards mode** in all modern browsers. Without it, browsers fall back to **quirks mode**, emulating bugs from Internet Explorer 5 and Netscape Navigator 4 from the late 1990s.

In earlier versions of HTML, the DOCTYPE was much more verbose:

```html
<!-- HTML 4.01 Strict -->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">

<!-- HTML 4.01 Transitional -->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">

<!-- XHTML 1.0 Strict -->
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
```

HTML5 simplified this to `<!DOCTYPE html>`, case-insensitive. This is the only DOCTYPE you should use today, regardless of whether you are writing HTML or XHTML syntax.

The DOCTYPE must be the very first thing in the document. Nothing — not even whitespace — can come before it. If you put a blank line, a comment, or a byte order mark (BOM) before the DOCTYPE, some versions of Internet Explorer will enter quirks mode.

### The `<html>` Root Element

The `<html>` element wraps the entire document except for the DOCTYPE. It is the root of the DOM tree.

```html
<html lang="en">
```

The `lang` attribute specifies the primary language of the document. It is important for:

- **Screen readers** — the browser tells the assistive technology which language to use for pronunciation
- **Search engines** — language-specific search results and indexing
- **Translation tools** — automatic translation services use `lang` to detect the source language
- **Typography** — some browsers and OS select appropriate fonts and hyphenation rules based on language

Use language codes from BCP 47:

| Code | Language |
|------|----------|
| `en` | English |
| `en-US` | American English |
| `en-GB` | British English |
| `es` | Spanish |
| `fr` | French |
| `ar` | Arabic |
| `zh-Hans` | Chinese (Simplified) |
| `ja` | Japanese |

Do not omit `lang`. Even if your page is only for English speakers, `lang="en"` tells the browser and assistive technology to handle the content accordingly.

The `<html>` element can also include other attributes:

```html
<html lang="en" dir="ltr">
```

`dir` specifies text direction: `ltr` (left-to-right, default) or `rtl` (right-to-left, for Arabic, Hebrew, Persian). If you set `dir` on `<html>`, it applies to the entire document unless overridden on individual elements.

### The `<head>` Section

The `<head>` contains metadata about the document — information that is not displayed as content. The browser reads the head before rendering the body.

Required elements in `<head>`:

```html
<head>
    <meta charset="utf-8">
    <title>Page Title</title>
</head>
```

**`<meta charset="utf-8">`** — declares the character encoding. UTF-8 is the universal standard for the web. It supports every character from every human language. This line should be the first child of `<head>` because the browser needs to know the encoding before it can parse the rest of the document correctly.

**`<title>`** — the document title. It appears in the browser tab, search engine results, and social media previews. Every page must have a unique, descriptive title. Search engines use the title as the primary headline in search results.

```html
<title>Document Structure - HTML | DevBook</title>
```

Recommended elements in `<head>`:

**Viewport meta tag** — controls how the page is displayed on mobile devices:

```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

Without this tag, mobile browsers render the page at a desktop width (typically 980px) and then shrink it to fit the screen, resulting in tiny text that users must zoom to read. With the viewport tag, the page matches the device width and renders at a readable size.

**Description meta tag** — provides a summary for search engine results:

```html
<meta name="description" content="Learn how HTML document structure works — doctype, head, body, and the DOM tree.">
```

Search engines often display this description below the title in search results. Keep it between 150 and 160 characters.

**Canonical link** — specifies the preferred URL when the same content is accessible from multiple URLs:

```html
<link rel="canonical" href="https://example.com/html/document-structure">
```

This prevents duplicate content penalties from search engines.

Other common elements in `<head>`:

```html
<!-- External stylesheets -->
<link rel="stylesheet" href="styles.css">

<!-- Preload critical resources -->
<link rel="preload" href="fonts/inter.woff2" as="font" crossorigin>

<!-- Open Graph tags for social sharing -->
<meta property="og:title" content="Document Structure">
<meta property="og:description" content="Learn how HTML document structure works.">
<meta property="og:image" content="https://example.com/og-image.png">

<!-- Favicon -->
<link rel="icon" href="favicon.ico" type="image/x-icon">

<!-- RSS feed -->
<link rel="alternate" type="application/rss+xml" title="Blog RSS" href="/feed.xml">
```

Elements in `<head>` should appear in a logical order: charset first, then viewport, then title, then everything else. Scripts in `<head>` block rendering, so they should be deferred or loaded asynchronously when possible.

### The `<body>` Section

The `<body>` contains all visible content. Everything inside `<body>` is rendered by the browser — text, images, videos, forms, buttons, and interactive elements.

```html
<body>
    <header>
        <h1>My Website</h1>
        <nav>
            <a href="/">Home</a>
            <a href="/about">About</a>
        </nav>
    </header>
    <main>
        <article>
            <h2>Article Title</h2>
            <p>Content goes here.</p>
        </article>
    </main>
    <footer>
        <p>Copyright 2026</p>
    </footer>
</body>
```

There is only one `<body>` element per document. It is a direct child of `<html>` and a sibling of `<head>`.

The `<body>` element supports several event handler attributes that fire at the document level:

```html
<body onload="init()" onunload="cleanup()">
```

These are rarely used in modern development — event listeners are typically attached via JavaScript using `addEventListener` instead.

### The Document Tree

The browser parses HTML into a tree structure called the **DOM (Document Object Model)**. Every element, attribute, and text node becomes a node in this tree.

For the HTML:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <title>My Page</title>
</head>
<body>
    <h1>Hello</h1>
    <p>Welcome to <strong>my</strong> site.</p>
</body>
</html>
```

The browser builds this tree:

```
document
 └── html
      ├── head
      │    └── title
      │         └── "My Page"
      └── body
           ├── h1
           │    └── "Hello"
           └── p
                ├── "Welcome to "
                └── strong
                     └── "my"
                └── " site."
```

Understanding the document tree is essential for:

- **CSS selectors** — styles cascade down the tree and inherit from parent to child
- **JavaScript DOM manipulation** — `querySelector`, `parentNode`, `children`, `closest` all traverse the tree
- **Event propagation** — events bubble up from the target element to the root
- **Accessibility** — screen readers navigate the tree to determine reading order and hierarchy
- **Performance** — deep or wide trees take longer to render and update

### Whitespace in HTML

HTML has specific rules for how whitespace (spaces, tabs, newlines) is handled:

**Collapsing whitespace.** Consecutive whitespace characters in text content are collapsed into a single space:

```html
<p>Hello     world</p>
<p>Hello
world</p>
<!-- Both render as: "Hello world" -->
```

**Whitespace around block elements.** Whitespace before and after block-level elements is ignored:

```html
<body>

    <p>Hello</p>

</body>
<!-- Renders the same as without the blank lines -->
```

**Whitespace inside inline elements.** Whitespace within inline content is preserved as a single space:

```html
<p>This is <strong>  bold  </strong> text.</p>
<!-- Renders as: "This is  bold  text." (spaces around "bold" are preserved) -->
```

**Whitespace in `<pre>` and `<code>`.** Elements with `white-space: pre` (like `<pre>` and `<code>`) preserve every space and newline:

```html
<pre>
    function greet() {
        return "Hello";
    }
</pre>
```

**Whitespace between inline-block elements.** Two `<span>` or `<img>` elements separated by a newline in the source will have a visible space between them on screen. This is a common source of layout bugs:

```html
<span>First</span>
<span>Second</span>
<!-- There is a space between "First" and "Second" because of the newline -->
```

To remove the space, either remove the whitespace in the source or use `font-size: 0` on the parent.

### Comments

HTML comments are not displayed in the browser but are visible in the source code:

```html
<!-- This is a comment -->
<p>Visible content</p>

<!-- 
    Multi-line comments
    are useful for documenting
    complex structures
-->

<!--[if IE]>
    <p>This is only visible to Internet Explorer.</p>
<![endif]-->
```

Comments can also be used to comment out sections of code temporarily during development:

```html
<!--
<div class="experimental">
    <p>This section is hidden.</p>
</div>
-->
```

Best practices for comments:

- Do not put sensitive information in comments — they are visible to anyone who views the page source
- Use comments sparingly in production code; clean HTML should be self-documenting
- Conditional comments (`<!--[if IE]>`) only work in Internet Explorer 9 and earlier; they are obsolete and should not be used

Comments cannot be nested. An opening `<!--` will match the next `-->`, which can cause unexpected results:

```html
<!-- Outer comment
    <!-- Inner comment -->
<p>Still commented out?</p>
-->
```

The inner `-->` closes the outer comment, leaving the rest of the line exposed. The `-->` at the end is treated as invalid HTML (ignored by browsers).

### Character Encoding

Character encoding determines how bytes in the file are mapped to characters. The wrong encoding produces garbled text (mojibake):

```
Wrong encoding:  Ã©clatÃ©  (HTML file saved as Latin-1 but declared as UTF-8)
Correct:         éclaté   (consistent UTF-8 throughout)
```

**UTF-8** is the standard for the web. Over 98% of websites use UTF-8 according to W3Techs surveys.

Declaring the encoding:

```html
<meta charset="utf-8">
```

This should be the first element in `<head>`. The browser needs to know the encoding before it parses any content that follows.

If the encoding is not declared, browsers attempt to detect it by analyzing byte patterns. This heuristic approach is unreliable and can lead to security vulnerabilities (UTF-7 cross-site scripting attacks).

To ensure consistency:

1. Set `<meta charset="utf-8">` in every page
2. Save your HTML files as UTF-8 in your editor
3. Serve pages with the `Content-Type: text/html; charset=utf-8` HTTP header
4. Remove the BOM (Byte Order Mark) from UTF-8 files — it can cause issues in some browsers

For special characters that are hard to type or might be misinterpreted, use HTML entities:

```html
<p>&copy; 2026 &mdash; All&nbsp;Rights&nbsp;Reserved.</p>
```

| Entity | Character | Description |
|--------|-----------|-------------|
| `&amp;` | & | Ampersand |
| `&lt;` | < | Less than |
| `&gt;` | > | Greater than |
| `&quot;` | " | Double quote |
| `&apos;` | ' | Apostrophe / single quote |
| `&nbsp;` | | Non-breaking space |
| `&copy;` | © | Copyright |
| `&mdash;` | — | Em dash |
| `&hellip;` | … | Ellipsis |

### Rendering Modes

Browsers have three rendering modes for HTML documents:

**Standards mode** — triggered by `<!DOCTYPE html>`. The browser follows W3C specifications as closely as possible. This is what you want.

**Almost standards mode** — triggered by transitional or frameset DOCTYPEs from HTML 4.01. The browser follows standards except for a small number of layout quirks related to images inside table cells. Rarely encountered today.

**Quirks mode** — triggered by no DOCTYPE or an incomplete DOCTYPE. The browser emulates bugs from Internet Explorer 5 and Netscape Navigator 4. Box models, font sizing, and layout algorithms differ significantly from standards.

The difference in box model:

```
Standards mode box:
┌──── content-box ────┐
│ width = content      │
│ padding on outside   │
│ border on outside    │
└──────────────────────┘

Quirks mode box:
┌──── border-box ──────┐
│ width = content      │
│   + padding + border │
└──────────────────────┘
```

In quirks mode, `width: 100px` with `padding: 10px` and `border: 5px` results in a content width of 70px (100 - 10 - 10 - 5 - 5). In standards mode, the same CSS gives a content width of 100px and a total width of 130px.

To detect the current rendering mode in JavaScript:

```javascript
const mode = document.compatMode;
// "CSS1Compat" for standards mode
// "BackCompat" for quirks mode
```

Always use `<!DOCTYPE html>` to stay in standards mode.

### Validation

HTML validation checks whether your document follows the HTML specification. It catches:

- Missing required elements and attributes
- Improper nesting
- Duplicate IDs
- Incorrect attribute values
- Invalid element placements (e.g., a `<div>` inside a `<p>`)

Validation tools:

- **W3C Validator** (validator.w3.org) — the official HTML validator
- **Nu HTML Checker** — the validator integrated into developer tools
- **Editor plugins** — VS Code extensions like HTMLHint provide real-time validation

Example validation error:

```
Error: Element "div" not allowed as child of element "ul" in this context.
```

This means you put a `<div>` directly inside a `<ul>`, which is not allowed — only `<li>` can be children of `<ul>`.

Validation is not strictly required for rendering. Browsers will display invalid HTML. But validation helps:

- Identify structural issues before they cause layout bugs
- Ensure accessibility by using elements correctly
- Improve SEO through proper element usage
- Make code more predictable for other developers

Not all validation errors are equally important. Some are critical (missing `alt` on images), some are warnings (obsolete attributes), and some can be safely ignored (validator-specific recommendations).

### Common Structural Mistakes

**Missing or incorrect DOCTYPE.** Without `<!DOCTYPE html>`, the browser enters quirks mode, causing unpredictable layout across browsers.

**Missing `<title>`.** The page has no title in the tab bar, no headline in search results, and screen readers announce the filename instead.

**Duplicate `<head>` or `<body>` content.** Some server-side includes or template engines accidentally output multiple `<head>` or `<body>` sections. Browsers handle this by ignoring everything after the first occurrence, which can lead to missing stylesheets or scripts.

**Scripts blocking rendering.** Placing `<script>` tags in `<head>` without `async` or `defer` blocks the HTML parser until the script downloads and executes. This delays rendering and makes the page appear to load slowly.

**Empty or whitespace-only `<head>`.** A page with nothing useful in `<head>` — missing charset, viewport, and title — will have poor accessibility, bad SEO, and no mobile optimization.

**Putting block elements inside inline elements.** HTML parsing rules automatically close the inline element when a block element is encountered:

```html
<a href="#">
    <div>Click me</div>
</a>
```

The browser interprets this as:

```html
<a href="#"></a>
<div>Click me</a></div>
```

The link is empty and the div is no longer inside the anchor. Always ensure block elements are not inside inline elements.

**Nested `<a>` elements.** Links cannot contain other links. If you nest them, the browser closes the outer link when it encounters the inner one.

**Forgetting the `lang` attribute.** The page lacks language information, which affects screen reader pronunciation and search engine indexing.

```html
<!-- Missing -->
<html>

<!-- Correct -->
<html lang="en">
```

**Using `<br>` for spacing.** Multiple `<br>` elements to create vertical space is a structural misuse. Use CSS `margin` instead.

```html
<!-- Wrong -->
<p>Line one</p>
<br><br><br>
<p>Line two</p>

<!-- Correct -->
<p style="margin-bottom: 3em;">Line one</p>
<p>Line two</p>
```

## Glossary

| Term | Definition |
|------|------------|
| DOCTYPE | A declaration that tells the browser which rendering mode to use |
| Standards mode | Browser rendering mode that follows W3C specifications |
| Quirks mode | Browser rendering mode that emulates bugs from IE5 and NN4 |
| Root element | The topmost element in the DOM tree (`<html>`) |
| `<head>` | Container for metadata about the document |
| `<body>` | Container for all visible content |
| `<title>` | The document title, shown in tabs and search results |
| `<meta>` | Element that provides structured metadata about the document |
| Charset | Character encoding that maps bytes to characters |
| UTF-8 | Universal character encoding standard for the web |
| Viewport | The visible area of a web page in the browser |
| DOM | Document Object Model — the tree representation of HTML |
| Node | A single element, text, or attribute in the DOM tree |
| Whitespace collapsing | The rule that multiple spaces become one in HTML rendering |
| Entity | A named character reference like `&amp;` for & |
| BOM | Byte Order Mark — a Unicode signature at the start of a file |
| Validation | Checking HTML against the specification for correctness |
| Canonical URL | The preferred URL for a page when duplicates exist |
| Open Graph | A protocol for rich social media previews |
| BCP 47 | The standard for language tags like `en-US` |
| hreflang | Attribute that specifies the language of a linked document |
| Rendering mode | How the browser interprets CSS and layout rules |
| Metadata | Data about the document, not the document content |
| Mojibake | Garbled text caused by incorrect character encoding |

## Quick References

- [MDN: HTML Document Structure](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Document_and_website_structure) — MDN guide
- [WHATWG HTML Spec](https://html.spec.whatwg.org/multipage/semantics.html#the-document-element) — official specification for `<html>`, `<head>`, `<body>`
- [W3C Validator](https://validator.w3.org/) — validate your HTML documents
- [Can I Use: Charset](https://caniuse.com/mdn-html_elements_meta_charset) — browser support for charset declaration
- [BCP 47 Language Tags](https://www.iana.org/assignments/language-subtag-registry/language-subtag-registry) — registered language subtags
- [Open Graph Protocol](https://ogp.me/) — official specification for social media previews
- [HTML Living Standard: The DOCTYPE](https://html.spec.whatwg.org/multipage/syntax.html#the-doctype) — DOCTYPE specification details

## Next Steps

- [Elements, Tags & Attributes](elements-and-attributes.md) — building blocks of HTML
- [Block vs Inline Elements](block-vs-inline.md) — understanding element display behavior
- [Text Content](text-content.md) — headings, paragraphs, lists, quotes, and code
- [Meta Tags & Social Sharing](meta-tags.md) — deep dive into metadata
