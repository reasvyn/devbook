# Display Types & Normal Flow

## Description

Every element has a display type that determines its layout behavior — whether it fills a line, sits inline with text, or participates in a formatting context like flex or grid.

## Prerequisites

- [The Box Model](box-model.md) — how display affects box behavior
- [CSS Selectors](selectors.md) — targeting elements by tag

## Table of Contents

- [Normal Flow](#normal-flow)
- [Block Elements](#block-elements)
- [Inline Elements](#inline-elements)
- [Inline-Block](#inline-block)
- [None](#none)
- [Contents](#contents)
- [Block Formatting Context](#block-formatting-context)
- [Inline Formatting Context](#inline-formatting-context)
- [Flow Root](#flow-root)
- [List-Item](#list-item)
- [Table Display Values](#table-display-values)
- [Flex and Grid](#flex-and-grid)
- [Run-In](#run-in)
- [Changing Display](#changing-display)
- [Display and Accessibility](#display-and-accessibility)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Normal Flow

Normal flow is the default layout mode. Elements stack in the order they appear in HTML:

```html
<div>Block 1</div>
<div>Block 2</div>
<span>Inline 1</span>
<span>Inline 2</span>
```

Block elements stack vertically. Inline elements flow horizontally within the line.

### Block Elements

Block-level elements:
- Start on a new line
- Take full available width by default
- Respect all box model properties (width, height, margin, padding)
- Stack vertically

```css
display: block;
```

Common block elements: `<div>`, `<p>`, `<h1>`–`<h6>`, `<ul>`, `<ol>`, `<li>`, `<section>`, `<article>`, `<header>`, `<footer>`, `<nav>`, `<aside>`, `<main>`, `<form>`, `<table>`, `<blockquote>`, `<pre>`, `<hr>`.

### Inline Elements

Inline-level elements:
- Do not start on a new line
- Flow horizontally within text
- Ignore explicit width and height
- Only respect horizontal margin
- Padding works but may overlap other lines vertically

```css
display: inline;
```

Common inline elements: `<span>`, `<a>`, `<strong>`, `<em>`, `<b>`, `<i>`, `<u>`, `<code>`, `<img>`, `<input>`, `<label>`, `<abbr>`, `<cite>`.

**Replaced inline elements** (`<img>`, `<input>`, `<video>`) have intrinsic size and do respect width/height:

```css
img {
    display: inline;        /* Default */
    width: 200px;           /* Works — replaced element */
}

span {
    display: inline;        /* Non-replaced */
    width: 200px;           /* Ignored */
}
```

### Inline-Block

`inline-block` combines inline external behavior with block internal behavior:

```css
.element {
    display: inline-block;
    width: 200px;           /* Works (unlike inline) */
    height: 100px;          /* Works */
    margin: 10px;           /* Works in all directions */
    padding: 10px;          /* Works — no vertical overlap */
}
```

The element flows inline with text, but its box behaves like a block (full box model).

Common uses:
- Navigation items in a horizontal bar
- Cards in a responsive grid (before flexbox/grid)
- Button groups

**The whitespace gap issue:**

```html
<!-- HTML whitespace between inline-block elements creates gaps -->
<nav>
    <a href="#">Home</a>
    <a href="#">About</a>
    <a href="#">Contact</a>
</nav>
```

Fixes:

```css
/* Fix 1: Remove whitespace in HTML */
<nav><a href="#">Home</a><a href="#">About</a><a href="#">Contact</a></nav>

/* Fix 2: Font-size 0 on parent, restore on children */
nav { font-size: 0; }
nav a { font-size: 1rem; }

/* Fix 3: Negative margin (fragile) */
nav a { margin-right: -4px; }

/* Fix 4: Flexbox (modern, preferred) */
nav { display: flex; gap: 0; }
```

### None

`display: none` removes the element from the document entirely:

```css
.hidden {
    display: none;          /* Removed from layout, not visible, not interactive */
}
```

The element is not rendered and does not take up space. Screen readers typically do not read `display: none` content.

Compare with alternatives:

| Value | Space | Visible | Interactive | Accessible |
|---|---|---|---|---|
| `display: none` | No | No | No | No |
| `visibility: hidden` | Yes | No | No | No |
| `opacity: 0` | Yes | No | Yes | Yes |
| `opacity: 0; pointer-events: none` | Yes | No | No | Yes |
| `position: absolute; left: -9999px` | No | No | No | No |

### Contents

`display: contents` removes the element's box from the layout while keeping its children:

```css
.grid {
    display: grid;
    grid-template-columns: 1fr 1fr 1fr;
}

.wrapper {
    display: contents;      /* Wrapper box disappears */
}
```

```html
<div class="grid">
    <div class="wrapper">   <!-- This wrapper is invisible to the grid -->
        <div>Item 1</div>   <!-- Direct child of grid for layout -->
        <div>Item 2</div>
    </div>
    <div>Item 3</div>
</div>
```

The children of `.wrapper` become direct grid items. The `.wrapper` no longer generates a box.

Caveats:
- The element is removed from the accessibility tree — do not use on interactive elements.
- Focusable children still work (links, inputs).
- Browser support is good (2020+).

### Block Formatting Context

A Block Formatting Context (BFC) is a region where block elements layout according to specific rules:

- Internal boxes are placed vertically, one after another
- Margins between adjacent boxes collapse
- Floats are contained within the BFC
- The BFC does not overlap floats

Creating a BFC:

```css
.bfc {
    display: flow-root;         /* Creates BFC — cleanest way */
    overflow: hidden;           /* Creates BFC (had side effects) */
    display: inline-block;      /* Creates BFC */
    display: flex;              /* Creates BFC */
    display: grid;              /* Creates BFC */
    float: left;                /* Creates BFC */
    position: absolute;         /* Creates BFC */
    contain: layout;            /* Creates BFC */
}
```

`display: flow-root` is the modern, side-effect-free way to create a BFC:

```css
.container {
    display: flow-root;         /* Contains floats, prevents margin collapse */
}
```

### Inline Formatting Context

An Inline Formatting Context (IFC) is where inline-level elements flow:

- Inline boxes flow horizontally, wrapping to new lines when needed
- Vertical alignment is controlled by `vertical-align`
- Line height determines the height of each line box
- Text-align controls horizontal distribution

```css
p {
    text-align: justify;        /* Distributes inline content */
    line-height: 1.6;           /* Line box height */
}
```

Every text node creates an anonymous inline box. Inline-block elements create an IFC boundary.

### Flow Root

`display: flow-root` was specifically designed to create a BFC without side effects:

```css
.card {
    display: flow-root;
}

/* Instead of */
.card {
    overflow: hidden;       /* Creates BFC but clips content */
}
```

Use cases:
- Contain floats without `overflow: hidden` (which may clip shadows/overflow)
- Prevent margin collapse without `padding: 1px`
- Establish a fresh layout context for children

### List-Item

`display: list-item` creates a marker box (bullet/number):

```css
.custom-item {
    display: list-item;
    list-style-type: disc;          /* Default */
    list-style-position: outside;   /* Default (outside content box) */
}

.custom-item::marker {
    color: red;                     /* Style the bullet */
    font-size: 1.5em;
}
```

Every `<li>` defaults to `display: list-item`. The `::marker` pseudo-element styles the marker.

### Table Display Values

CSS table display mimics HTML table layout without table markup:

```css
.table {
    display: table;
}

.table-row {
    display: table-row;
}

.table-cell {
    display: table-cell;
    padding: 0.5rem;
    border: 1px solid #ddd;
}

.table-caption { display: table-caption; }
.table-column { display: table-column; }
.table-column-group { display: table-column-group; }
.table-header-group { display: table-header-group; }
.table-footer-group { display: table-footer-group; }
.table-row-group { display: table-row-group; }
```

Table display provides equal-height columns by default.

### Flex and Grid

`display: flex` and `display: grid` create their own formatting contexts:

```css
.flex-container {
    display: flex;                  /* Block-level flex container */
    display: inline-flex;           /* Inline-level flex container */
}

.grid-container {
    display: grid;                  /* Block-level grid container */
    display: inline-grid;           /* Inline-level grid container */
}
```

Both establish a new formatting context for their children, overriding normal flow. They are detailed in separate documents.

### Run-In

`display: run-in` lets an element run into the next block element:

```css
h2 {
    display: run-in;
}
```

```html
<h2>Title</h2>
<p>This paragraph follows.</p>
<!-- In supported browsers: "Title This paragraph follows." on same line -->
```

Poor browser support — rarely used in practice.

### Changing Display

Changing display mid-page is common:

```css
/* Mobile: hide main nav, show hamburger */
.main-nav {
    display: none;
}

@media (min-width: 768px) {
    .main-nav {
        display: block;             /* Visible on desktop */
    }
}

/* Toggle visibility with class */
.menu.is-open {
    display: block;
}
```

**Transition note:** `display` cannot be animated. To animate visibility, use `opacity` or `transform` with `visibility: hidden`:

```css
.panel {
    opacity: 0;
    visibility: hidden;
    transition: opacity 0.3s, visibility 0.3s;
}

.panel.is-open {
    opacity: 1;
    visibility: visible;
}
```

### Display and Accessibility

`display: none` removes content from the accessibility tree — screen readers do not read it.

`visibility: hidden` also hides from screen readers.

`opacity: 0` and `position: absolute; left: -9999px` keep content in the accessibility tree — use for visually hidden but accessible labels:

```css
.sr-only {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border: 0;
}
```

## Glossary

| Term | Definition |
|---|---|
| Normal flow | Default layout: block elements stack, inline elements flow |
| Block Formatting Context | Region with block layout rules |
| Inline Formatting Context | Region with inline layout rules |
| display: contents | Remove element's box, keep children |
| display: flow-root | Create BFC without side effects |
| Replaced element | Element with intrinsic size (img, input) |
| Anonymous box | Implicit box created for text or other content |
| Run-in | Heading that merges with following paragraph |

## Quick References

- [MDN: display](https://developer.mozilla.org/en-US/docs/Web/CSS/display) — complete display reference
- [MDN: Block Formatting Context](https://developer.mozilla.org/en-US/docs/Web/Guide/CSS/Block_formatting_context) — BFC guide
- [CSS Display Level 3](https://www.w3.org/TR/css-display-3/) — specification
- [Accessible Hiding](https://www.a11yproject.com/posts/how-to-hide-content/) — `sr-only` pattern

## Next Steps

- [Flexbox](flexbox.md) — modern one-dimensional layout
- [CSS Grid](grid.md) — modern two-dimensional layout