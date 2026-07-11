# Text Content

## Description

Text is the core of the web. HTML provides a rich set of elements for structuring text — headings, paragraphs, lists, quotations, code blocks, and inline semantics. Using them correctly makes content readable, accessible, and meaningful for both humans and machines.

## Prerequisites

- [Elements, Tags & Attributes](elements-and-attributes.md) — fundamental HTML building blocks
- [Block vs Inline Elements](block-vs-inline.md) — understanding element display behavior

## Table of Contents

- [Headings](#headings)
- [Paragraphs](#paragraphs)
- [Line Breaks and Horizontal Rules](#line-breaks-and-horizontal-rules)
- [Lists: Unordered](#lists-unordered)
- [Lists: Ordered](#lists-ordered)
- [Lists: Description](#lists-description)
- [Nested Lists](#nested-lists)
- [Quotations](#quotations)
- [Inline Quotations](#inline-quotations)
- [Code and Preformatted Text](#code-and-preformatted-text)
- [Inline Semantic Elements](#inline-semantic-elements)
- [Abbreviations and Definitions](#abbreviations-and-definitions)
- [Subscript and Superscript](#subscript-and-superscript)
- [Time and Dates](#time-and-dates)
- [Highlighted Text](#highlighted-text)
- [Special Characters and Entities](#special-characters-and-entities)
- [Comments](#comments)
- [Whitespace and Text Formatting](#whitespace-and-text-formatting)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Headings

HTML provides six levels of headings, from `<h1>` (most important) to `<h6>` (least important).

```html
<h1>Document Title</h1>
<h2>Section Title</h2>
<h3>Subsection Title</h3>
<h4>Sub-subsection Title</h4>
<h5>Fifth-Level Heading</h5>
<h6>Sixth-Level Heading</h6>
```

**Heading hierarchy rules:**

- Exactly one `<h1>` per page for the main topic
- Do not skip levels (`<h2>` → `<h4>` is invalid)
- Screen readers and search engines rely on heading hierarchy for navigation and topic understanding
- Headings are block-level elements

Correct: `<h1>` → `<h2>` → `<h3>` → `<h3>` → `<h2>`. Incorrect: jumping from `<h1>` to `<h3>` (breaks the document outline). Skipping levels makes navigation harder for assistive technology.

### Paragraphs

The `<p>` element represents a paragraph of text. It is one of the most common HTML elements.

```html
<p>This is a paragraph. It contains text content that forms a coherent block of related sentences. Browsers add margin above and below paragraphs by default.</p>

<p>This is another paragraph. The browser automatically separates it from the previous paragraph with vertical space.</p>
```

Key rules:

- `<p>` is a block-level element — each paragraph starts on a new line
- `<p>` can only contain phrasing content (inline elements and text)
- A `<p>` implicitly closes when another block-level element is encountered (like `<div>`, another `<p>`, or headings)
- Do not use empty paragraphs (`<p></p>`) for spacing — use CSS `margin` instead
- Do not use multiple `<br>` tags inside a `<p>` for spacing between lines — use CSS `padding` or `margin`

Line breaks within paragraphs:

```html
<p>
    This is a single paragraph
    that spans multiple lines in the source code.
    The line breaks in the source do not appear in the rendered output.
</p>
```

Line breaks in the HTML source are collapsed into a single space. To create a line break within a paragraph, use `<br>`:

```html
<p>
    First Street<br>
    Second Avenue<br>
    New York, NY 10001
</p>
```

Use `<br>` for line breaks that are part of the content (addresses, poems). Do not use `<br>` to create space between paragraphs — that is a styling concern.

### Line Breaks and Horizontal Rules

**`<br>`** (line break) — creates a line break in text. It is a void element with no closing tag.

```html
<p>
    Roses are red,<br>
    Violets are blue,<br>
    Sugar is sweet,<br>
    And so are you.
</p>
```

Use `<br>` only when the line break is part of the content: addresses, poems, song lyrics, or code snippets where the line break carries meaning. Do not use `<br>` as a substitute for CSS margins or padding between paragraphs.

**`<hr>`** (horizontal rule) — represents a thematic break between sections of content. It is a void element.

```html
<section>
    <h2>Chapter 1</h2>
    <p>Content of chapter 1.</p>
</section>

<hr>

<section>
    <h2>Chapter 2</h2>
    <p>Content of chapter 2.</p>
</section>
```

The `<hr>` does not necessarily need to display as a horizontal line. Its semantic meaning is "thematic break." Browsers render it as a line by default, but CSS can change its appearance entirely:

```css
hr {
    border: none;
    height: 2px;
    background: linear-gradient(to right, transparent, #333, transparent);
}
```

### Lists: Unordered

Unordered lists (`<ul>`) contain items marked with bullet points. Use them when the order of items does not matter.

```html
<ul>
    <li>Apples</li>
    <li>Bananas</li>
    <li>Cherries</li>
</ul>
```

Renders as:

- Apples
- Bananas
- Cherries

The `<ul>` element can only contain `<li>` (list item) children. Direct text or other elements inside `<ul>` are invalid.

The `<li>` element can contain any flow content — paragraphs, images, links, other lists, or even entire sections:

```html
<ul>
    <li>
        <h3>Complex item</h3>
        <p>This list item contains a heading and a paragraph.</p>
        <img src="photo.jpg" alt="Photo">
    </li>
    <li>
        <a href="/page">Link as a list item</a>
    </li>
</ul>
```

The `type` attribute on `<ul>` sets the bullet style, but it is deprecated in HTML5. Use CSS `list-style-type` instead:

```css
ul {
    list-style-type: square; /* disc, circle, square, none */
}
```

### Lists: Ordered

Ordered lists (`<ol>`) contain items displayed with sequential markers. Use them when the order matters — instructions, rankings, steps, or ranked items.

```html
<ol>
    <li>Preheat oven to 180°C</li>
    <li>Mix flour and sugar</li>
    <li>Bake for 30 minutes</li>
</ol>
```

The `<ol>` element supports several attributes:

**`type`** — sets the numbering style:

| Value | Style | Example |
|---|---|---|
| `1` | Decimal numbers | 1, 2, 3 |
| `A` | Uppercase letters | A, B, C |
| `a` | Lowercase letters | a, b, c |
| `I` | Uppercase Roman | I, II, III |
| `i` | Lowercase Roman | i, ii, iii |

```html
<ol type="A">
    <li>First item</li>
    <li>Second item</li>
    <li>Third item</li>
</ol>
```

Renders as:

A. First item
B. Second item
C. Third item

**`start`** — sets the starting number:

```html
<ol start="5">
    <li>Fifth item</li>
    <li>Sixth item</li>
</ol>
```

Renders as:

5. Fifth item
6. Sixth item

**`reversed`** — a boolean attribute that reverses the order:

```html
<ol reversed>
    <li>Third place</li>
    <li>Second place</li>
    <li>First place</li>
</ol>
```

Renders as:

3. Third place
2. Second place
1. First place

**`value`** on `<li>` — overrides the number for a specific item and resets numbering for subsequent items:

```html
<ol>
    <li>Item 1</li>
    <li value="10">Item 10 (renumbered)</li>
    <li>Item 11 (continues from 10)</li>
</ol>
```

### Lists: Description

Description lists (`<dl>`) group terms and their descriptions. They are like a dictionary or key-value pair list.

```html
<dl>
    <dt>HTML</dt>
    <dd>HyperText Markup Language — the standard markup language for web documents.</dd>

    <dt>CSS</dt>
    <dd>Cascading Style Sheets — used to style HTML elements.</dd>

    <dt>JavaScript</dt>
    <dd>A programming language that adds interactivity to web pages.</dd>
</dl>
```

A `<dl>` contains pairs of `<dt>` (description term) and `<dd>` (description details). Multiple `<dt>` elements can share one `<dd>`:

```html
<dl>
    <dt>Color</dt>
    <dt>Colour</dt>
    <dd>The property of objects as perceived by the eye, determined by the wavelengths of reflected light.</dd>
</dl>
```

And one `<dt>` can have multiple `<dd>` entries:

```html
<dl>
    <dt>Apple</dt>
    <dd>A fruit grown on apple trees.</dd>
    <dd>A technology company based in Cupertino.</dd>
</dl>
```

Use `<dl>` for glossaries, metadata (key-value pairs), or any list of term-description pairs. Do not use `<dl>` purely for visual formatting — use a table or a custom structure instead.

### Nested Lists

Lists can contain other lists. The nested list must be inside an `<li>` element, not directly inside a `<ul>` or `<ol>`.

```html
<ul>
    <li>Fruits
        <ul>
            <li>Apples</li>
            <li>Bananas</li>
            <li>Cherries</li>
        </ul>
    </li>
    <li>Vegetables
        <ul>
            <li>Carrots</li>
            <li>Broccoli</li>
        </ul>
    </li>
</ul>
```

Renders as:

- Fruits
  - Apples
  - Bananas
  - Cherries
- Vegetables
  - Carrots
  - Broccoli

Nested lists can mix types:

```html
<ol>
    <li>Step one: gather ingredients
        <ul>
            <li>Flour</li>
            <li>Sugar</li>
            <li>Eggs</li>
        </ul>
    </li>
    <li>Step two: mix</li>
</ol>
```

### Quotations

**Block quotations (`<blockquote>`)** represent content quoted from another source. It is a block-level element.

```html
<blockquote cite="https://example.com/source">
    <p>The only way to do great work is to love what you do.</p>
    <footer>— Steve Jobs</footer>
</blockquote>
```

The `cite` attribute points to the source URL. It is not displayed by default but can be accessed by search engines and automated tools.

The `<blockquote>` element can contain any flow content — paragraphs, headings, lists, or other elements. It is not limited to just text.

Browsers apply default left margin and sometimes italic styling to `<blockquote>`, which can be customized with CSS.

### Inline Quotations

**`<q>`** represents a short inline quotation. Browsers automatically add quotation marks around the content.

```html
<p>As Aristotle said, <q cite="https://example.com/ethics">We are what we repeatedly do.</q></p>
```

**`<cite>`** — represents the title of a creative work (book, article, film, song, website). It is not for quoting a person.

```html
<p>In <cite>The Pragmatic Programmer</cite>, the authors discuss software craftsmanship.</p>
```

Browsers typically render `<cite>` in italics.

### Code and Preformatted Text

**`<code>`** — represents a fragment of computer code. It is an inline element displayed in a monospace font.

```html
<p>Use the <code>console.log()</code> function to print to the console.</p>
```

**`<pre>`** — represents preformatted text. Whitespace inside `<pre>` is preserved, and the text is displayed in a monospace font. It is a block-level element.

```html
<pre>
    function greet(name) {
        return "Hello, " + name + "!";
    }

    console.log(greet("World"));
</pre>
```

The `<pre>` element is ideal for:

- Code blocks with indentation
- ASCII art
- Log output
- Any content where whitespace matters (tables of data, poetry)

Combining `<pre>` with `<code>` is the standard way to display code blocks:

```html
<pre><code>function greet(name) {
    return `Hello, ${name}!`;
}</code></pre>
```

**`<samp>`** — represents sample output from a computer program. Inline, monospace.

```html
<p>When you run the program, it prints <samp>Hello, World!</samp></p>
```

**`<kbd>`** — represents keyboard input. Inline, monospace.

```html
<p>Press <kbd>Ctrl</kbd> + <kbd>C</kbd> to copy.</p>
```

**`<var>`** — represents a variable in programming or mathematical expression. Inline, typically rendered in italics.

```html
<p>The equation <var>E</var> = <var>mc</var><sup>2</sup> is famous.</p>
```

### Inline Semantic Elements

These elements add meaning to inline text. Browsers apply default styling, but the semantics matter more than the visual.

| Element | Meaning | Visual | Use when |
|---|---|---|---|
| `<strong>` | Strong importance | Bold | Warnings, alerts, key points |
| `<em>` | Stress emphasis | Italic | Changing meaning, tone |
| `<b>` | Stylistic offset | Bold | Keywords, product names |
| `<i>` | Alternate voice | Italic | Foreign words, technical terms, thoughts |
| `<u>` | Unarticulated annotation | Underline | Misspellings, proper names |
| `<span>` | No semantic meaning | None | Generic styling hook |

```html
<p><strong>Warning:</strong> The <i>Fahrvergnügen</i> feature is <em>not</em> available.</p>
```

### Abbreviations and Definitions

**`<abbr>`** — represents an abbreviation or acronym. The `title` attribute provides the full expansion.

```html
<p>The <abbr title="World Health Organization">WHO</abbr> issued a statement.</p>
```

On hover, most browsers display the `title` text as a tooltip. Screen readers typically announce the expansion when encountering the abbreviated text.

Provide the `title` on first use of an abbreviation:

```html
<p><abbr title="HyperText Markup Language">HTML</abbr> is the language of the web.</p>
```

**`<dfn>`** — marks the defining instance of a term. Use it when introducing a new term for the first time.

```html
<p><dfn>HTML</dfn> (HyperText Markup Language) is the standard markup language for creating web pages.</p>
```

### Subscript and Superscript

**`<sub>`** — subscript: text displayed below the baseline, typically smaller. Used for chemical formulas, mathematical indices, and footnotes.

```html
<p>Chemical formula for water: H<sub>2</sub>O</p>
<p>Footnote reference<sub><a href="#footnote-1">1</a></sub> in text.</p>
```

**`<sup>`** — superscript: text displayed above the baseline, typically smaller. Used for exponents, ordinal indicators, and footnote references.

```html
<p>Area = πr<sup>2</sup></p>
<p>The 1<sup>st</sup> of January.</p>
<p>This statement has a footnote.<sup><a href="#fn-1">1</a></sup></p>
```

### Time and Dates

**`<time>`** — represents a specific period in time. The `datetime` attribute provides a machine-readable format while the element content is human-readable.

```html
<p>Published on <time datetime="2026-06-28">June 28, 2026</time></p>
<p>The meeting is at <time datetime="14:30">2:30 PM</time>.</p>
<p>The event starts on <time datetime="2026-07-04T09:00:00Z">July 4, 2026 at 9:00 AM UTC</time>.</p>
```

The `datetime` attribute supports several formats:

| Format | Example | Meaning |
|---|---|---|
| `YYYY-MM-DD` | `2026-06-28` | Date |
| `HH:MM` | `14:30` | Time (24-hour) |
| `HH:MM:SS` | `14:30:00` | Time with seconds |
| `YYYY-MM-DDTHH:MM` | `2026-06-28T14:30` | Date and time |
| `YYYY-MM-DDTHH:MM:SSZ` | `2026-06-28T14:30:00Z` | Date and time (UTC) |
| `YYYY-MM-DDTHH:MM:SS+HH:MM` | `2026-06-28T14:30:00+05:00` | Date and time with timezone offset |
| `YYYY-Www` | `2026-W26` | Week number |
| `YYYY` | `2026` | Year only |
| `MM` | `06` | Month only |

If the `datetime` attribute is omitted, the element content must be a valid date/time string that the browser can parse. Always include `datetime` for consistency.

### Highlighted Text

**`<mark>`** — represents text highlighted for reference purposes, such as search results or key passages.

```html
<p>The search results for "HTML" show that <mark>HTML</mark> is the foundation of the web.</p>
```

The `<mark>` element does not indicate importance (unlike `<strong>`) or emphasis (unlike `<em>`). It indicates relevance — text that is marked for reference purposes.

```html
<p>
    In the paragraph below, the highlighted portions show where the search terms appear:
</p>
<blockquote>
    <p>
        <mark>HTML</mark> stands for HyperText <mark>Markup</mark> Language.
        It is the standard <mark>markup</mark> language for creating web pages.
    </p>
</blockquote>
```

### Special Characters and Entities

Some characters have special meaning in HTML and must be escaped using character entities:

| Character | Entity | Why escape |
|---|---|---|
| `<` | `&lt;` | Opens an HTML tag |
| `>` | `&gt;` | Closes an HTML tag |
| `&` | `&amp;` | Starts an entity |
| `"` | `&quot;` | Attribute value delimiter |
| `'` | `&apos;` | Attribute value delimiter (XHTML) |

```html
<p>The &lt;div&gt; element is used for grouping content.</p>
<p>AT&amp;T is a telecommunications company.</p>
<p>The attribute value was "hello &amp; goodbye".</p>
```

Common typographic entities: `&copy;` (©), `&reg;` (®), `&trade;` (™), `&mdash;` (—), `&ndash;` (–), `&hellip;` (…), `&ldquo;` ("), `&rdquo;` ("), `&lsquo;` ('), `&rsquo;` ('), `&bull;` (•), `&sect;` (§), `&nbsp;` (non-breaking space).

Numerical entities can reference any Unicode character:

```html
<p>&#x1F600; — this is a grinning face emoji (U+1F600).</p>
```

Decimal: `&#128512;` — same character. Hexadecimal: `&#x1F600;` — more common and readable.

The non-breaking space (`&nbsp;`) is the most commonly misused entity. It prevents a line break between two words or elements:

```html
<p>We offer 50&nbsp;years of experience.</p>
```

Without `&nbsp;`, "50" and "years" could break across lines. With `&nbsp;`, they stay together.

### Comments

HTML comments are not displayed in the browser but are visible in the source code:

```html
<!-- This is a comment -->
<p>Visible content</p>

<!--
    Multi-line comments
    are useful for temporarily hiding sections
-->
```

Comments can be used to document complex structures or to temporarily hide sections during development. Do not put sensitive information in comments — they are visible to anyone who views the page source.

### Whitespace and Text Formatting

Whitespace in HTML is collapsed into single spaces by default:

```html
<p>This     text      has      extra      spaces.</p>
<!-- Renders as: "This text has extra spaces." -->
```

Line breaks in the source become spaces:

```html
<p>
    This text spans
    multiple lines
    in the source.
</p>
<!-- Renders as: "This text spans multiple lines in the source." -->
```

To control whitespace explicitly:

**`<pre>`** preserves all whitespace (see code section above).

**CSS `white-space` property:**

```css
.nowrap {
    white-space: nowrap;   /* No line breaks within the element */
}

.pre-wrap {
    white-space: pre-wrap; /* Preserve whitespace but allow wrapping */
}

.pre-line {
    white-space: pre-line; /* Collapse whitespace but preserve line breaks */
}
```

**`<wbr>`** — a word break opportunity. It tells the browser where a word may be broken at the end of a line.

```html
<p>This is a verylongwordthatmight<wbr>breakacrosslines.</p>
```

The browser will only break the word at the `<wbr>` point if it needs to. If the line is long enough, the `<wbr>` is invisible.

## Glossary

| Term | Definition |
|------|------------|
| Heading | An element (`<h1>`–`<h6>`) that defines a section title in the document outline |
| Paragraph | A block of text wrapped in `<p>` |
| List | A group of related items (`<ul>`, `<ol>`, `<dl>`) |
| List item | A single item within a list (`<li>`) |
| Description list | A list of term-description pairs (`<dl>`, `<dt>`, `<dd>`) |
| Quotation | Content cited from another source (`<blockquote>`, `<q>`) |
| Citation | A reference to a creative work (`<cite>`) |
| Code | Computer code text (`<code>`) |
| Preformatted text | Text where whitespace is preserved (`<pre>`) |
| Keyboard input | User input from a keyboard (`<kbd>`) |
| Sample output | Output from a computer program (`<samp>`) |
| Variable | A variable in math or programming (`<var>`) |
| Strong importance | Content with strong emphasis (`<strong>`) |
| Emphasis | Stressed text (`<em>`) |
| Abbreviation | A shortened form of a word or phrase (`<abbr>`) |
| Definition | The defining instance of a term (`<dfn>`) |
| Subscript | Text displayed below the baseline (`<sub>`) |
| Superscript | Text displayed above the baseline (`<sup>`) |
| Highlighted text | Text marked for reference (`<mark>`) |
| Time | A machine-readable date or time (`<time>`) |
| Entity | A named or numerical character reference in HTML |
| Non-breaking space | A space that prevents line breaks (`&nbsp;`) |
| Document outline | The hierarchical structure of headings in a document |
| Line break | A forced line break within text (`<br>`) |
| Thematic break | A separation between content sections (`<hr>`) |
| Whitespace collapsing | The rule that multiple consecutive spaces become one |

## Quick References

- [MDN: HTML Text Fundamentals](https://developer.mozilla.org/en-US/docs/Learn/HTML/Introduction_to_HTML/HTML_text_fundamentals) — comprehensive guide
- [MDN: `<h1>`–`<h6>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Heading_Elements) — heading element reference
- [MDN: `<ul>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ul) — unordered list reference
- [MDN: `<ol>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ol) — ordered list reference
- [MDN: `<dl>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dl) — description list reference
- [MDN: `<blockquote>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/blockquote) — block quotation reference
- [MDN: `<q>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/q) — inline quotation reference
- [MDN: `<code>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/code) — code element reference
- [MDN: `<pre>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/pre) — preformatted text reference
- [MDN: `<abbr>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/abbr) — abbreviation reference
- [MDN: `<time>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/time) — time element reference
- [HTML Living Standard: Text Semantics](https://html.spec.whatwg.org/multipage/text-level-semantics.html) — official specification
- [HTML Entity Reference](https://html.spec.whatwg.org/multipage/named-characters.html) — complete entity list

## Next Steps

- [Links & Navigation](links.md) — anchors, URLs, and navigation patterns
- [Images & Responsive Images](images.md) — embedding and optimizing images
- [Tables](tables.md) — tabular data structure and accessibility
- [Semantic HTML & Document Outline](semantic-html.md) — structural meaning in markup
