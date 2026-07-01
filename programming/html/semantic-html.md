# Semantic HTML & Document Outline

## Description

Semantic HTML uses elements that convey meaning about their content — telling browsers, search engines, and assistive technologies what each part of the page represents. A well-structured semantic document creates a clear outline that benefits accessibility, SEO, and maintainability.

## Prerequisites

- [Document Structure](document-structure.md) — the basic HTML document structure
- [Block vs Inline Elements](block-vs-inline.md) — element types

## Table of Contents

- [What is Semantic HTML?](#what-is-semantic-html)
- [The Document Outline](#the-document-outline)
- [Semantic Page Structure](#semantic-page-structure)
- [Heading Hierarchy](#heading-hierarchy)
- [Sectioning Elements](#sectioning-elements)
- [Content Elements](#content-elements)
- [Text-Level Semantics](#text-level-semantics)
- [Semantic vs Presentational](#semantic-vs-presentational)
- [SEO Benefits](#seo-benefits)
- [Accessibility Benefits](#accessibility-benefits)
- [Common Mistakes](#common-mistakes)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What is Semantic HTML?

Semantic HTML uses elements that describe the meaning of their content:

```html
<!-- Non-semantic (meaningless structure) -->
<div id="header">
    <div class="nav">...</div>
</div>
<div id="main">
    <div class="article">...</div>
</div>

<!-- Semantic (meaningful structure) -->
<header>
    <nav>...</nav>
</header>
<main>
    <article>...</article>
</main>
```

Semantic elements tell browsers, search engines, and assistive technologies:
- "This is a navigation menu" (`<nav>`)
- "This is the main content" (`<main>`)
- "This is a self-contained article" (`<article>`)
- "This content is tangentially related" (`<aside>`)

**Why semantics matter:**
- **Accessibility** — screen readers use semantic elements to navigate and announce content
- **SEO** — search engines use semantic structure to understand page content
- **Maintainability** — semantic HTML is easier for developers to read and understand
- **Future compatibility** — browsers may add new features based on semantic elements

### The Document Outline

Every HTML document has an implicit outline based on its heading hierarchy. The outline represents the structure of the content.

**Correct outline example:**

```
1. HTML Guide (h1)
    1.1 Introduction (h2)
    1.2 Elements (h2)
        1.2.1 Block Elements (h3)
        1.2.2 Inline Elements (h3)
    1.3 Forms (h2)
        1.3.1 Input Types (h3)
        1.3.2 Validation (h3)
```

**Corresponding HTML:**

```html
<h1>HTML Guide</h1>

<h2>Introduction</h2>

<h2>Elements</h2>
<h3>Block Elements</h3>
<h3>Inline Elements</h3>

<h2>Forms</h2>
<h3>Input Types</h3>
<h3>Validation</h3>
```

**Sectioning elements** (`<article>`, `<section>`, `<nav>`, `<aside>`) can modify the outline. Each sectioning element creates a new section scope:

```html
<h1>Site Title</h1>

<section>
    <h2>Section Title</h2>
    <p>Content in this section.</p>

    <section>
        <h3>Sub-section</h3>
        <p>Nested content.</p>
    </section>
</section>

<article>
    <h2>Article Title</h2>
    <p>Independent content.</p>
</article>
```

**The HTML5 outline algorithm** (how browsers and screen readers interpret headings) treats each sectioning element as a new section. However, the HTML5 outline algorithm is not well-supported by screen readers — it is safer to use heading levels (h1-h6) consistently rather than relying on sectioning elements to reset the outline.

### Semantic Page Structure

A typical semantic page layout:

```html
<body>
    <header>
        <nav>
            <ul>
                <li><a href="/">Home</a></li>
                <li><a href="/about">About</a></li>
            </ul>
        </nav>
    </header>

    <main>
        <article>
            <header>
                <h1>Article Title</h1>
                <time datetime="2026-06-28">June 28, 2026</time>
            </header>

            <p>Article content...</p>

            <section>
                <h2>Related Details</h2>
                <p>Additional information.</p>
            </section>
        </article>

        <aside>
            <h2>Related Articles</h2>
            <ul>
                <li><a href="/article-2">Article 2</a></li>
            </ul>
        </aside>
    </main>

    <footer>
        <p>&copy; 2026 DevBook</p>
    </footer>
</body>
```

### Heading Hierarchy

Headings define the document structure. Rules for a clear hierarchy:

**Use one `<h1>` per page.** The `<h1>` should describe the page's primary topic:

```html
<h1>Complete Guide to HTML Semantics</h1>
```

**Do not skip heading levels.** Going from `<h2>` to `<h4>` creates gaps in the outline:

```html
<!-- Bad: skipping h3 -->
<h1>Guide</h1>
<h2>Introduction</h2>
<h4>Details</h4>  <!-- Skipped h3 -->

<!-- Good: sequential -->
<h1>Guide</h1>
<h2>Introduction</h2>
<h3>Details</h3>
```

**Use headings for structure, not styling.** Use CSS for visual size:

```html
<!-- Bad: using h4 for visual style -->
<h4 class="section-title">Important Section</h4>

<!-- Good: correct heading level, styled with CSS -->
<h2 class="section-title">Important Section</h2>
```

**Nest headings within sectioning elements for deeper structure:**

```html
<article>
    <h1>Blog Post Title</h1>

    <section>
        <h2>Background</h2>
        <p>...</p>
    </section>

    <section>
        <h2>Analysis</h2>
        <p>...</p>
        <section>
            <h3>Methodology</h3>
            <p>...</p>
        </section>
    </section>
</article>
```

### Sectioning Elements

**`<header>`** — introductory content for a page or section:

```html
<article>
    <header>
        <h1>Article Title</h1>
        <p class="meta">By Author | June 28, 2026</p>
    </header>
    <p>Article content...</p>
</article>
```

A page can have multiple `<header>` elements — one for the page and one per article/section.

**`<nav>`** — navigation links:

```html
<nav aria-label="Main navigation">
    <ul>
        <li><a href="/">Home</a></li>
        <li><a href="/docs">Docs</a></li>
        <li><a href="/about">About</a></li>
    </ul>
</nav>
```

Use `<nav>` for primary navigation blocks. Do not wrap every link group in `<nav>` — only major navigation sections.

**`<main>`** — the dominant content of the page:

```html
<main>
    <article>
        <h1>Page Content</h1>
        <p>...</p>
    </article>
</main>
```

Only one `<main>` element per page. It should not contain content that repeats across pages (site headers, footers, sidebars).

**`<article>`** — self-contained, reusable content:

```html
<article>
    <h2>Blog Post Title</h2>
    <p>Blog content...</p>
    <footer>
        <a href="/post/123">Read more</a>
    </footer>
</article>

<article>
    <h2>Another Post</h2>
    <p>More content...</p>
</article>
```

Use `<article>` for blog posts, news articles, forum posts, comments, and any content that could be independently distributed.

**`<section>`** — a thematic grouping of content:

```html
<section>
    <h2>Installation</h2>
    <p>How to install the software...</p>
</section>

<section>
    <h2>Configuration</h2>
    <p>How to configure the software...</p>
</section>
```

Each `<section>` should have a heading. Do not use `<section>` as a generic wrapper (use `<div>` for that).

**`<aside>`** — content tangentially related to the main content:

```html
<aside>
    <h2>Related Articles</h2>
    <ul>
        <li><a href="/semantic-html">Semantic HTML</a></li>
        <li><a href="/accessibility">Accessibility</a></li>
    </ul>
</aside>
```

Use `<aside>` for sidebars, pull quotes, callout boxes, and advertising.

**`<footer>`** — footer for a page or section:

```html
<footer>
    <p>&copy; 2026 DevBook. All rights reserved.</p>
    <nav aria-label="Footer navigation">
        <a href="/privacy">Privacy Policy</a>
        <a href="/terms">Terms of Service</a>
    </nav>
</footer>
```

A page can have multiple `<footer>` elements (one per article/section).

**`<address>`** — contact information:

```html
<address>
    Written by <a href="mailto:author@example.com">Author Name</a><br>
    DevBook, 123 Learning Street<br>
    San Francisco, CA 94102
</address>
```

The `<address>` element is for contact information related to the document or its author, not arbitrary addresses.

### Content Elements

**`<figure>` and `<figcaption>`** — self-contained media with caption:

```html
<figure>
    <img src="chart.png" alt="Bar chart showing quarterly sales">
    <figcaption>Quarterly sales for fiscal year 2026.</figcaption>
</figure>
```

**`<blockquote>`** — a block-level quotation:

```html
<blockquote cite="https://example.com/source">
    <p>The only way to learn a new programming language is by writing programs in it.</p>
    <footer>— <cite>Dennis Ritchie</cite></footer>
</blockquote>
```

**`<details>` and `<summary>`** — expandable disclosure widget:

```html
<details>
    <summary>Click to show more information</summary>
    <p>This content is hidden until the user clicks the summary.</p>
</details>
```

The `open` attribute makes the details visible by default.

**`<dl>` / `<dt>` / `<dd>`** — description list:

```html
<dl>
    <dt>HTML</dt>
    <dd>HyperText Markup Language — the standard language for web documents.</dd>
    <dt>CSS</dt>
    <dd>Cascading Style Sheets — a language for describing the presentation of web documents.</dd>
</dl>
```

**`<pre>`** — preformatted text (preserves whitespace and line breaks):

```html
<pre><code>function hello() {
    console.log('Hello, world!');
}</code></pre>
```

### Text-Level Semantics

| Element | Purpose | Example |
|---|---|---|
| `<strong>` | Strong importance | `<strong>Warning</strong>` |
| `<em>` | Emphasis | `<em>never</em>` |
| `<abbr>` | Abbreviation | `<abbr title="HTML">HTML</abbr>` |
| `<code>` | Code fragment | `<code>&lt;article&gt;</code>` |
| `<kbd>` | Keyboard input | `<kbd>Ctrl</kbd> + <kbd>C</kbd>` |
| `<samp>` | Sample output | `<samp>Hello</samp>` |
| `<var>` | Variable | `<var>E</var> = <var>mc</var><sup>2</sup>` |
| `<time>` | Date/time | `<time datetime="2026-06-28">June 28</time>` |
| `<mark>` | Highlighted text | `<mark>important</mark>` |
| `<small>` | Fine print | `<small>Legal text</small>` |
| `<sub>` / `<sup>` | Sub/superscript | `H<sub>2</sub>O`, `footnote<sup>1</sup>` |
| `<b>` / `<i>` | Stylistic bold/italic | `<b>Product name</b>`, `<i>Falcon 9</i>` |
| `<br>` | Line break | `Address<br>City` |
| `<wbr>` | Word break opportunity | `long-<wbr>word` |

`<b>` is for text stylistically offset (keywords, product names). `<i>` is for text in an alternate voice (technical terms, foreign words). Prefer semantic elements (`<strong>`, `<em>`, `<cite>`) when they fit. Use `<br>` for line breaks within an address or poem, never for paragraph spacing.

### Semantic vs Presentational

| Presentational (avoid) | Semantic (prefer) |
|---|---|
| `<b>` | `<strong>` for importance |
| `<i>` | `<em>` for emphasis, `<cite>` for citations |
| `<font>` | CSS `font-family` |
| `<center>` | CSS `text-align: center` |
| `<u>` | CSS `text-decoration: underline` (but avoid underlining non-links) |
| `<s>` | `<del>` for deleted content |
| `<big>` | CSS `font-size` |

**Deprecated presentational elements** that must not be used:
- `<basefont>` — use CSS
- `<big>` — use CSS
- `<blink>` — use CSS animation
- `<center>` — use CSS
- `<font>` — use CSS
- `<marquee>` — use CSS animation
- `<s>` — use `<del>` or `<ins>`
- `<strike>` — use `<del>` or `<ins>`
- `<tt>` — use `<code>`, `<kbd>`, `<samp>`, or `<var>`
- `<u>` — use CSS `text-decoration`

### SEO Benefits

Search engines use semantic HTML to understand page content:

- **`<h1>`** — primary topic of the page
- **`<nav>`** — identifies navigation links (not primary content)
- **`<main>`** — the primary content (not repeated elements)
- **`<article>`** — self-contained content for indexing
- **`<time>`** — publication dates for search snippets

Search engines weigh content in semantic elements differently:
- Content in `<main>` is more important than content in `<header>` or `<footer>`
- Content in `<aside>` is treated as supplementary
- Heading hierarchy signals topic structure

Using semantic HTML does not guarantee better rankings, but it helps search engines understand your content, which can improve search visibility.

### Accessibility Benefits

Semantic HTML is the foundation of web accessibility:

**Screen reader navigation:**
- Users can jump between headings (`h1`-`h6`) to understand the page structure
- Users can navigate by landmark elements (`main`, `nav`, `aside`, `header`, `footer`)
- Users can list all links, headings, or landmarks on the page

**Landmark roles** are automatically applied by semantic elements:

| Element | Implicit ARIA role |
|---|---|
| `<header>` | `banner` (within body) |
| `<nav>` | `navigation` |
| `<main>` | `main` |
| `<article>` | `article` |
| `<section>` | `region` (if it has a heading) |
| `<aside>` | `complementary` |
| `<footer>` | `contentinfo` (within body) |
| `<form>` | `form` |

These roles allow screen reader users to jump directly to specific parts of the page using keyboard shortcuts.

**The Accessibility Tree** — browsers create an accessibility tree from the DOM. Semantic elements produce better accessibility tree entries:

```html
<!-- Non-semantic: screen reader sees generic elements -->
<div>Navigation</div>
<div>Main content</div>

<!-- Semantic: screen reader sees landmarks -->
<nav>Navigation</nav>
<main>Main content</main>
```

### Common Mistakes

**Using `<div>` for everything.** "Div soup" makes the document structure invisible to assistive technologies:

```html
<!-- Div soup -->
<div class="header">
    <div class="nav">...</div>
</div>
<div class="content">
    <div class="article">...</div>
</div>
```

**Using `<section>` as a generic wrapper.** `<section>` must represent a thematic grouping with a heading:

```html
<!-- Wrong: section used as generic wrapper -->
<section class="wrapper">
    <img src="photo.jpg" alt="">
    <p>Caption text</p>
</section>

<!-- Correct: div for generic wrapper -->
<div class="wrapper">
    <img src="photo.jpg" alt="">
    <p>Caption text</p>
</div>
```

**Multiple `<h1>` elements.** While HTML5 allows multiple `<h1>` elements within sectioning elements, it is simpler and more compatible to use one `<h1>` per page.

**Skipping heading levels.** Going from `<h2>` to `<h4>` creates gaps that confuse screen readers and break the outline.

**Using `<br>` for spacing.** Each `<br>` is a line break, not a paragraph separator. Use CSS `margin` for spacing:

```html
<!-- Wrong -->
<p>Line 1</p>
<br>
<br>
<p>Line 2</p>

<!-- Correct -->
<p>Line 1</p>
<p>Line 2</p>
```

**Using presentational elements.** `<b>`, `<i>`, `<font>`, `<center>` should be replaced with CSS and semantic elements.

**Not using headings in `<section>` or `<article>`.** These elements should typically have a heading that describes their content.

## Glossary

| Term | Definition |
|------|------------|
| Semantic HTML | HTML that conveys meaning about the structure and content |
| Document outline | The hierarchical structure of headings in a document |
| Sectioning element | Elements that create a section in the outline (article, section, nav, aside) |
| Landmark | A region of a page identified by ARIA role or semantic element |
| `<header>` | Introductory content for a page or section |
| `<nav>` | Navigation links |
| `<main>` | Primary content (one per page) |
| `<article>` | Self-contained, reusable content |
| `<section>` | Thematic grouping of content |
| `<aside>` | Tangentially related content |
| `<footer>` | Footer for a page or section |
| `<address>` | Contact information |
| `<figure>` | Self-contained media with caption |
| `<blockquote>` | Block-level quotation |
| `<details>` | Expandable disclosure widget |
| `<summary>` | Summary for a details element |
| `<dl>` | Description list |
| Accessibility Tree | The browser's representation of the page for assistive technologies |
| Div soup | Pages built entirely from non-semantic div elements |
| Implicit role | ARIA role automatically assigned by a semantic HTML element |

## Quick References

- [MDN: HTML Elements Reference](https://developer.mozilla.org/en-US/docs/Web/HTML/Element) — complete list
- [MDN: Sectioning Elements](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements) — heading guide
- [MDN: Document Structure](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/Document_and_website_structure) — structure guide
- [MDN: Landmark Roles](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/Roles/landmark_role) — ARIA landmarks
- [HTML Spec: Semantics](https://html.spec.whatwg.org/multipage/semantics.html) — official specification
- [WAI: Page Structure](https://www.w3.org/WAI/tutorials/page-structure/) — W3C structure guide
- [HTML5 Doctor: Semantic Elements](http://html5doctor.com/element-index/) — semantic element index
- [WebAIM: Semantic Structure](https://webaim.org/techniques/semanticstructure/) — accessibility guide

## Next Steps

- [HTML & ARIA](aria-basics.md) — accessible rich internet applications
- [Focus Management](focus-management.md) — controlling keyboard focus
- [Screen Reader Compatibility](screen-reader-compatibility.md) — testing with screen readers
