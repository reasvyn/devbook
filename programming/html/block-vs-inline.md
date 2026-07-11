# Block vs Inline Elements

## Description

Every HTML element has a default display behavior — it is either block-level or inline. This distinction determines how elements flow in the document, how they respond to width and height, and how they interact with surrounding content. Mastering this distinction is essential for layout, styling, and avoiding common CSS bugs.

## Prerequisites

- [Elements, Tags & Attributes](elements-and-attributes.md) — fundamental HTML building blocks
- [Document Structure](document-structure.md) — the HTML document skeleton

## Table of Contents

- [Block-Level Elements](#block-level-elements)
- [Inline Elements](#inline-elements)
- [Inline-Block Elements](#inline-block-elements)
- [The Difference in Flow](#the-difference-in-flow)
- [Box Model Differences](#box-model-differences)
- [Nesting Rules: Block and Inline](#nesting-rules-block-and-inline)
- [Replaced vs Non-Replaced Elements](#replaced-vs-non-replaced-elements)
- [Changing Display Behavior with CSS](#changing-display-behavior-with-css)
- [The Display Property](#the-display-property)
- [Historic Elements: `<center>`, `<font>`, `<big>`, `<tt>`](#historic-elements-center-font-big-tt)
- [Hidden and None Elements](#hidden-and-none-elements)
- [Common Mistakes](#common-mistakes)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Block-Level Elements

Block-level elements create a "block" in the document flow. They always start on a new line and take up the full available width by default, pushing subsequent content to the next line.

Key characteristics:

- Start on a new line
- Occupy the full width of their parent by default
- Can contain other block elements and inline elements
- Respect `width`, `height`, `margin`, and `padding` properties
- Stack vertically (one after another)

Common block elements:

```html
<div>           <!-- Generic container (most used) -->
<p>             <!-- Paragraph -->
<h1> to <h6>    <!-- Headings -->
<ul>, <ol>, <li> <!-- Lists -->
<section>       <!-- Thematic section -->
<article>       <!-- Self-contained composition -->
<nav>           <!-- Navigation links -->
<header>        <!-- Introductory content -->
<footer>        <!-- Closing information -->
<main>          <!-- Primary content -->
<aside>         <!-- Tangential content -->
<blockquote>    <!-- Extended quotation -->
<pre>           <!-- Preformatted text -->
<table>         <!-- Tabular data -->
<form>          <!-- User input form -->
<fieldset>      <!-- Grouped form controls -->
<hr>            <!-- Thematic break -->
```

Visual demonstration of block behavior:

```
┌────────────────────────────────────────────┐
│ Div 1 (full width)                         │
└────────────────────────────────────────────┘
┌────────────────────────────────────────────┐
│ Div 2 (full width)                         │
└────────────────────────────────────────────┘
┌────────────────────────────────────────────┐
│ Div 3 (full width)                         │
└────────────────────────────────────────────┘
```

Each block element occupies its own horizontal "row," even if the content is short.

### Inline Elements

Inline elements do not break the flow of text. They sit within a line, wrapping only as needed, and occupy only as much width as their content requires.

Key characteristics:

- Do not start on a new line
- Occupy only the width of their content
- Cannot contain block-level elements (with some exceptions)
- `width` and `height` properties do not apply (they are determined by content)
- Only horizontal margins and padding work predictably; vertical margins and padding do not affect the flow
- Flow horizontally, wrapping to the next line only when they exceed the container width

Common inline elements:

```html
<a>             <!-- Anchor (hyperlink) -->
<span>          <!-- Generic inline container -->
<strong>        <!-- Strong importance -->
<em>            <!-- Emphasis -->
<b>             <!-- Bold (stylistic) -->
<i>             <!-- Italic (stylistic) -->
<u>             <!-- Underline -->
<code>          <!-- Code snippet -->
<kbd>           <!-- Keyboard input -->
<samp>          <!-- Sample output -->
<var>           <!-- Variable -->
<cite>          <!-- Citation -->
<q>             <!-- Inline quotation -->
<abbr>          <!-- Abbreviation -->
<sub>           <!-- Subscript -->
<sup>           <!-- Superscript -->
<small>         <!-- Side comment -->
<time>          <!-- Date/time -->
<mark>          <!-- Highlighted text -->
<label>         <!-- Form label -->
<br>            <!-- Line break -->
<img>           <!-- Image (inline but replaced) -->
<input>         <!-- Input control -->
<button>        <!-- Button -->
<textarea>      <!-- Multi-line text input -->
<select>        <!-- Dropdown -->
<svg>           <!-- SVG graphic -->
<canvas>        <!-- Canvas graphic -->
```

Visual demonstration of inline behavior:

```
Text before <span>inline content</span> text after.
↓
Text before inline content text after.
```

Multiple inline elements form a line:

```
┌──────────────────────────────────────┐
│  <a>Home</a>  <a>About</a>  <a>Contact</a>   │
│  Home About Contact                    │
│  If the line is long enough            │
│  <a>Very Long Link Text That Wraps</a> │
│  Very Long Link Text That              │
│  Wraps                                 │
└──────────────────────────────────────┘
```

### Inline-Block Elements

Inline-block is a hybrid display mode. Elements with `display: inline-block` behave like inline elements from the outside (they flow in the text line) but like block elements on the inside (they respect width, height, margins, and padding).

Key characteristics:

- Flow horizontally like inline elements
- Respect `width` and `height` like block elements
- Respect vertical margins and padding
- Create a new stacking context for their children
- Do not break the surrounding text flow

```css
.inline-block-demo {
    display: inline-block;
    width: 200px;
    height: 100px;
    margin: 10px;
    padding: 8px;
}
```

```html
<div style="display: inline-block; width: 150px;">Box 1</div>
<div style="display: inline-block; width: 150px;">Box 2</div>
<div style="display: inline-block; width: 150px;">Box 3</div>
```

```
┌──────────┐ ┌──────────┐ ┌──────────┐
│  Box 1   │ │  Box 2   │ │  Box 3   │
│ 150px w  │ │ 150px w  │ │ 150px w  │
└──────────┘ └──────────┘ └──────────┘
(All on the same line, but each respects width and height)
```

This makes inline-block useful for:

- Grid-like layouts without flexbox (though flexbox is now preferred)
- Navigation items that need padding and margins but flow horizontally
- Components that need to align with text but have explicit dimensions

### The Difference in Flow

The core difference between block and inline is how they participate in the document flow.

**Block flow:**

```html
<div>Block A</div>
<div>Block B</div>
<div>Block C</div>
```

Renders as:

```
Block A
Block B
Block C
```

Each div occupies a full line. The vertical space between blocks is determined by their margins (which collapse — see below).

**Inline flow:**

```html
<span>Inline A</span>
<span>Inline B</span>
<span>Inline C</span>
```

Renders as:

```
Inline A Inline B Inline C
```

All three spans sit on the same line. If the line is too narrow, they wrap:

```
Inline A Inline B
Inline C
```

**Mixed flow:**

```html
<p>This is <strong>important</strong> text.</p>
```

The `<strong>` element is inline, so it stays within the paragraph without breaking the line:

```
This is important text.
```

### Box Model Differences

The CSS box model applies differently to block and inline elements.

**Block box model:**

```
┌─── margin ────────────────────────────┐
│  ┌─── border ────────────────────┐    │
│  │  ┌─── padding ────────────┐   │    │
│  │  │  ┌─── content ──────┐  │   │    │
│  │  │  │                   │  │   │    │
│  │  │  └───────────────────┘  │   │    │
│  │  └─────────────────────────┘   │    │
│  └────────────────────────────────┘    │
└────────────────────────────────────────┘
```

Block elements:
- `width` sets the content width (defaults to 100% of parent)
- `height` sets the content height (defaults to content height)
- `margin` pushes surrounding elements away on all four sides
- `padding` works in all four directions
- Vertical margins **collapse** — when two block elements are stacked, the larger margin wins, and margins are not added together

Margin collapse example:

```html
<div style="margin-bottom: 20px;">A</div>
<div style="margin-top: 30px;">B</div>
```

The space between A and B is 30px (the larger margin wins), not 50px (20 + 30). This is intentional — it prevents double-spacing between block elements.

**Inline box model:**

```
┌─ content ─┐
│  Inline   │  ← margins/padding only horizontal
└───────────┘
```

Inline elements:
- `width` and `height` are ignored (determined by content)
- `margin-left` and `margin-right` work; `margin-top` and `margin-bottom` do not affect flow
- `padding-left` and `padding-right` work; `padding-top` and `padding-bottom` apply visually but do not affect surrounding elements
- No margin collapse (margins do not collapse inline)
- The line box height is determined by the tallest inline element in the line

Why vertical padding on inline elements is tricky:

```html
<span style="padding: 20px; background: yellow;">Hello</span>
```

The padding applies visually — the background extends above and below the text — but the surrounding lines are not pushed away. The padding visually overlaps with adjacent lines.

### Nesting Rules: Block and Inline

The HTML specification defines what can nest inside what. The rules are based on element categories:

**Block elements can contain:**
- Other block elements
- Inline elements
- Text and phrasing content
- (Exceptions: `<p>`, `<h1>`–`<h6>` cannot contain block elements)

**Inline elements can contain:**
- Other inline elements
- Text and phrasing content
- (Exception: `<a>` can contain block elements in HTML5, but only when it is transparent)

The most important rule: **block elements cannot be nested inside inline elements in standard HTML parsing.**

```html
<!-- Invalid -->
<strong>
    <p>This paragraph is inside a strong element.</p>
</strong>
```

The browser parses this as:

```html
<strong></strong>
<p>This paragraph is inside a strong element.</p>
```

The `<strong>` is closed before the `<p>` because `<p>` is a block element that cannot live inside an inline element.

**The `<p>` and `<h1>`–`<h6>` exception:**

Paragraphs and headings can only contain phrasing (inline) content. If you put a block element inside a `<p>`, the browser closes the paragraph:

```html
<!-- Invalid -->
<p>Some text <div>inner div</div> more text.</p>

<!-- What the browser does -->
<p>Some text</p><div>inner div</div><p>more text.</p>
```

**The `<a>` exception (HTML5):**

In HTML5, an `<a>` element can wrap block-level content if it does not contain interactive content (like `<button>` or another `<a>`):

```html
<!-- Valid HTML5 -->
<a href="/article/42">
    <article>
        <h2>Article Title</h2>
        <p>Article summary goes here.</p>
    </article>
</a>
```

This is useful for making entire card components clickable. However, the `<a>` still has inline semantics — it flows with the text. To make a block-level link, use `display: block` in CSS.

### Replaced vs Non-Replaced Elements

HTML elements are also classified as replaced or non-replaced.

**Replaced elements** are elements whose content is replaced by an external resource. The browser knows their dimensions from the resource itself, not from the HTML content.

```html
<img src="photo.jpg" alt="Photo">     <!-- replaced by image -->
<video src="video.mp4"></video>        <!-- replaced by video -->
<iframe src="page.html"></iframe>       <!-- replaced by another document -->
<canvas></canvas>                       <!-- replaced by script drawing -->
<svg>...</svg>                          <!-- replaced by SVG document -->
<input type="text">                     <!-- replaced by form control -->
<textarea></textarea>                   <!-- replaced by form control -->
<select></select>                       <!-- replaced by dropdown widget -->
```

Replaced elements have special behavior:

- They are inline by default, but they respect `width` and `height` (unlike other inline elements)
- They have intrinsic dimensions (natural width and height) derived from their content
- They cannot be pseudo-elements (`::before` and `::after` do not work on replaced elements)
- Their padding and margin behavior follows block rules, not inline rules

**Non-replaced elements** are elements whose content is rendered from the HTML source. Their appearance is determined by the CSS applied to them.

```html
<p>, <div>, <span>, <strong>, <em>, <h1>, <ul>, <li>, <table>, <form>
```

Most HTML elements are non-replaced.

The distinction matters because replaced elements behave like inline-block elements by default — they flow inline but accept width and height. An `<img>` element sits within text but can have an explicit width set:

```html
<p>Here is an image: <img src="icon.png" style="width: 24px; height: 24px;"> inline.</p>
```

The image respects `width: 24px` even though it is an inline (replaced) element.

### Changing Display Behavior with CSS

You can change any element's display behavior using CSS. This is one of the most common and powerful techniques in web development:

```css
/* Make a span act like a block */
span {
    display: block;
}

/* Make a div act like an inline element */
div {
    display: inline;
}

/* Make anything inline-block */
.nav-item {
    display: inline-block;
}
```

When you change `display: block` on an inline element, it gains all block characteristics:

```css
a {
    display: block;
    width: 100%;
    padding: 12px;
    background: blue;
    color: white;
}
```

```html
<a href="/" style="display: block;">Home</a>
<a href="/about" style="display: block;">About</a>
```

Renders as:

```
┌──────────────────────────────────┐
│ Home                             │
├──────────────────────────────────┤
│ About                            │
└──────────────────────────────────┘
```

Each link now fills the entire width and stacks vertically.

### The Display Property

The CSS `display` property is the most important property for controlling element layout. It has many values beyond `block`, `inline`, and `inline-block`:

```
display: block          → The element generates a block box
display: inline         → The element generates an inline box
display: inline-block   → The element is inline on the outside, block on the inside
display: none           → The element is not rendered at all (removed from flow)
display: flex           → Block-level flex container
display: inline-flex    → Inline-level flex container
display: grid           → Block-level grid container
display: inline-grid    → Inline-level grid container
display: flow-root      → Block container that establishes a new block formatting context
display: contents       → The element disappears; its children are rendered as if they were direct children of the parent
display: table          → Behaves like <table>
display: table-cell     → Behaves like <td>
display: list-item      → Behaves like <li>
```

The distinction between `display: none` and `visibility: hidden` is important:

```html
<div style="display: none;">Invisible and takes no space</div>
<div style="visibility: hidden;">Invisible but still occupies space</div>
```

`display: none` removes the element from the document flow entirely — it takes up no space and is not rendered. `visibility: hidden` hides the element visually but the space it occupies remains.

### Historic Elements: `<center>`, `<font>`, `<big>`, `<tt>`

Several elements from earlier versions of HTML have been deprecated in favor of CSS. They should not be used in modern HTML.

| Element | Use instead | Notes |
|---|---|---|
| `<center>` | `text-align: center` | Centers content horizontally |
| `<font>` | CSS `font-size`, `color`, `font-family` | Styling should be in CSS |
| `<big>` | `font-size: larger` | CSS for size control |
| `<tt>` | `<code>`, `<kbd>`, `<samp>` | Depends on semantic meaning |
| `<strike>` | `<s>` or `text-decoration: line-through` | `<s>` is still valid HTML5 for inaccurate content |
| `<u>` | `text-decoration: underline` | Only use for misspellings or proper names |

```html
<!-- Historic pattern -->
<font size="4" color="red">text</font>

<!-- Modern equivalent -->
<span style="font-size: 1.2em; color: red;">text</span>
```

**`<small>`** is still valid in HTML5 for fine print, copyright, and side comments.

```html
<p><small>&copy; 2026 DevBook. All rights reserved.</small></p>
```

Browsers still render deprecated elements for backward compatibility, but using them in new code mixes presentation with structure.

### Hidden and None Elements

Some HTML elements are not visible by default but serve structural or meta purposes:

**`<head>`** — contains metadata. Never rendered directly.

**`<script>`** with no content — can be embedded without outputting anything.

```html
<script>
    // This code runs but does not display anything
    console.log('Hello');
</script>
```

**`<style>`** — contains CSS rules. Not rendered but affects the page.

**`<template>`** — holds HTML that is not rendered until cloned by JavaScript.

```html
<template id="card-template">
    <div class="card">
        <h3 class="card-title"></h3>
        <p class="card-body"></p>
    </div>
</template>
```

**`<dialog>`** without `open` — hidden until shown via JavaScript or the `open` attribute.

```html
<dialog>
    <p>This dialog is hidden until opened.</p>
</dialog>
```

**Elements with `hidden` attribute** — any element can be hidden by adding the `hidden` attribute:

```html
<div hidden>This div is hidden until the attribute is removed.</div>
```

The `hidden` attribute applies `display: none` in the browser's user agent stylesheet. It can be overridden by CSS:

```css
div[hidden] {
    display: block;  /* Override the browser's hidden behavior */
    visibility: hidden; /* Hide it but keep the space */
}
```

### Common Mistakes

**Using inline elements where block is needed.** Wrapping a `<button>` or `<div>` inside a `<span>` is unnecessary and can cause layout issues. Use the appropriate element for the job.

**Assuming `width` works on inline elements.** Setting `width: 100px` on a `<span>` has no effect unless `display: block` or `display: inline-block` is applied.

**Vertical margin on inline elements does not affect flow.** Using `margin-top: 20px` on an inline element will not push surrounding content away, even though the margin is rendered visually.

**Forgetting margin collapse.** Two consecutive block elements with `margin-bottom: 20px` and `margin-top: 30px` will have 30px between them, not 50px.

**Whitespace between inline-block elements.** When using `display: inline-block`, whitespace in the HTML source (like a newline between two elements) creates a visible space on screen:

```html
<!-- Four-pixel gap between boxes -->
<div class="box" style="display: inline-block;">A</div>
<div class="box" style="display: inline-block;">B</div>
```

The gap is approximately 4px (the width of a space character). Fixes include:

```css
/* Remove whitespace in HTML */
<div class="box">A</div><div class="box">B</div>

/* Set font-size to 0 on parent */
.parent { font-size: 0; }
.parent > * { font-size: 16px; }

/* Use negative margins */
.box { margin-right: -4px; }

/* Use flexbox instead (recommended) */
.parent { display: flex; }
```

**Putting `<div>` inside `<p>`.** The `<p>` element closes automatically before the `<div>`, breaking the intended structure.

**Replaced elements not respecting inline rules.** An `<img>` inside a `<p>` will respect width and height (because it is replaced), contrary to what you might expect from an inline element.

**Confusing `display: none` with `visibility: hidden`.** The first removes the element from flow; the second only hides it visually.

## Glossary

| Term | Definition |
|------|------------|
| Block element | An element that starts on a new line and takes full width |
| Inline element | An element that flows within text without breaking the line |
| Inline-block | An element that flows inline but respects block box properties |
| Replaced element | An element whose content is replaced by an external resource (`<img>`, `<video>`, `<iframe>`) |
| Non-replaced element | An element whose content is rendered from the HTML source |
| Box model | The CSS model: content, padding, border, margin |
| Margin collapse | When adjacent vertical margins combine to the larger value instead of adding |
| Line box | The virtual box that contains a line of inline elements |
| Intrinsic dimensions | The natural width and height of a replaced element |
| Display | The CSS property that controls how an element generates boxes |
| Flow | How elements are positioned relative to each other in the document |
| Phrasing content | HTML content model for inline-level elements |
| Flow content | HTML content model for block-level elements |
| Sectioning content | Elements that define document structure (`<article>`, `<section>`) |
| Transparent content model | An element's content model mirrors its parent's (`<a>`, `<ins>`, `<del>`) |
| Deprecated | A feature that is obsolete and should not be used in new code |
| User agent stylesheet | The browser's default CSS styles for HTML elements |
| Formatting context | The rendering environment that determines how elements are laid out |

## Quick References

- [MDN: Block-level Elements](https://developer.mozilla.org/en-US/docs/Web/HTML/Block-level_elements) — complete reference
- [MDN: Inline Elements](https://developer.mozilla.org/en-US/docs/Web/HTML/Inline_elements) — complete reference
- [MDN: Replaced Elements](https://developer.mozilla.org/en-US/docs/Web/CSS/Replaced_element) — guide to replaced elements
- [MDN: The Display Property](https://developer.mozilla.org/en-US/docs/Web/CSS/display) — all display values
- [W3C: Visual Formatting Model](https://www.w3.org/TR/CSS22/visuren.html) — official CSS specification
- [CSS Tricks: Display Property](https://css-tricks.com/almanac/properties/d/display/) — practical guide
- [MDN: Margin Collapse](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model/Mastering_margin_collapsing) — understanding margin collapse

## Next Steps

- [Text Content](text-content.md) — headings, paragraphs, lists, quotes, and code
- [Links & Navigation](links.md) — anchors, URLs, and navigation patterns
- [Semantic HTML & Document Outline](semantic-html.md) — structural meaning in markup
- [HTML Best Practices](best-practices.md) — writing clean, maintainable HTML
