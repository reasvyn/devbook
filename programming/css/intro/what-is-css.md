# What Is CSS?

## Description

CSS (Cascading Style Sheets) is the language that controls the visual presentation of HTML documents. It separates content from design, enabling consistent styling across pages, responsive layouts, and rich visual experiences.

## Prerequisites

- [What Is HTML?](../../html/intro/what-is-html.md) — understanding of HTML structure
- [Elements, Tags & Attributes](../../html/elements-and-attributes.md) — HTML elements that CSS targets

## Table of Contents

- [Definition](#definition)
- [CSS Solves a Problem](#css-solves-a-problem)
- [How CSS Works](#how-css-works)
- [CSS is Declarative](#css-is-declarative)
- [The Cascade](#the-cascade)
- [Inheritance](#inheritance)
- [The Box Model](#the-box-model)
- [Layout Models](#layout-models)
- [Responsive Design](#responsive-design)
- [CSS Today](#css-today)
- [CSS is Not Programming](#css-is-not-programming)
- [Browser Rendering Pipeline](#browser-rendering-pipeline)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Definition

CSS stands for Cascading Style Sheets. It is a stylesheet language used to describe the presentation of a document written in HTML or XML.

A CSS rule consists of a selector and a declaration block:

```css
selector {
    property: value;
}
```

```css
h1 {
    color: blue;
    font-size: 2rem;
}
```

The selector (`h1`) targets HTML elements. The declarations (`color: blue`) set visual properties.

### CSS Solves a Problem

Before CSS, HTML mixed content and presentation:

```html
<!-- Bad: presentation mixed with content -->
<font color="red" size="7">Welcome</font>
<center>Centered text</center>
```

CSS separates concerns:

```html
<!-- Good: structure in HTML, style in CSS -->
<h1 class="title">Welcome</h1>
<p class="centered">Centered text</p>
```

```css
.title { color: red; font-size: 3rem; }
.centered { text-align: center; }
```

This separation provides:
- **Maintainability** — change one CSS file to update an entire site
- **Reusability** — the same class can style many elements
- **Accessibility** — screen readers ignore presentational markup
- **Performance** — smaller HTML, cached CSS files
- **Responsiveness** — different styles for different screen sizes

### How CSS Works

1. The browser loads HTML and parses it into the DOM (Document Object Model)
2. The browser loads CSS and parses it into the CSSOM (CSS Object Model)
3. The browser matches selectors against DOM nodes to find which styles apply
4. The cascade resolves conflicting declarations
5. The computed styles are applied during rendering (style → layout → paint → composite)

```css
/* The browser reads this and applies it to matching elements */
p {
    color: #333;
    line-height: 1.6;
}
```

CSS can be applied in three ways:

**External stylesheet** (recommended):

```html
<link rel="stylesheet" href="styles.css">
```

**Internal stylesheet** (within `<style>` in `<head>`):

```html
<style>
    body { font-family: sans-serif; }
</style>
```

**Inline styles** (on the element itself — avoid for most cases):

```html
<p style="color: red;">Warning</p>
```

### CSS is Declarative

CSS is a **declarative** language — you describe what you want, not how to achieve it:

```css
/* Declarative: "make this a centered column" */
.container {
    display: flex;
    justify-content: center;
    align-items: center;
}
```

You do not write loops, conditionals, or algorithms. The browser figures out how to render the result. This makes CSS easier to write but can feel mysterious when something does not behave as expected.

**Debugging CSS** requires understanding the browser's rendering model — the cascade, the box model, and layout algorithms.

### The Cascade

The *C* in CSS stands for Cascading. When multiple rules target the same element, the cascade determines which wins.

```css
/* Multiple rules targeting the same element */
h1 { color: blue; }
.title { color: red; }
#main-title { color: green; }
```

The cascade considers three factors, in order:
1. **Importance** — `!important` overrides normal declarations
2. **Specificity** — inline > ID > class > element
3. **Source order** — later declarations override earlier ones

```html
<h1 id="main-title" class="title" style="color: orange;">Text</h1>
```

Inline style wins over IDs, which win over classes, which win over element selectors. Source order breaks ties at the same specificity level.

### Inheritance

Some CSS properties are inherited from parent to child automatically:

```css
/* color is inherited */
body { color: #333; }
p { /* inherits color: #333 from body */ }

/* border is NOT inherited */
body { border: 1px solid red; }
p { /* no border inherited */ }
```

**Inherited properties:** `color`, `font-family`, `font-size`, `line-height`, `text-align`, `visibility`

**Non-inherited properties:** `margin`, `padding`, `border`, `background`, `width`, `height`, `display`

Control inheritance explicitly:

```css
.child {
    color: inherit;    /* Force inheritance from parent */
    color: initial;    /* Reset to browser default */
    color: unset;      /* inherit if inherited, initial if not */
    color: revert;     /* Reset to browser's default style */
}
```

### The Box Model

Every element in CSS is a rectangular box. The box model defines how size and spacing are calculated:

```
┌─────────────────────────────────┐
│          Margin                 │
│  ┌─────────────────────────┐    │
│  │       Border            │    │
│  │  ┌───────────────────┐  │    │
│  │  │    Padding         │  │    │
│  │  │  ┌─────────────┐  │  │    │
│  │  │  │  Content    │  │  │    │
│  │  │  └─────────────┘  │  │    │
│  │  └───────────────────┘  │    │
│  └─────────────────────────┘    │
└─────────────────────────────────┘
```

```css
.box {
    width: 300px;
    padding: 20px;
    border: 2px solid black;
    margin: 10px;
    /* Total width = 300 + 40 + 4 + 20 = 364px */
}
```

The `box-sizing` property changes how `width` is calculated:

```css
.box {
    box-sizing: border-box;
    width: 300px;
    padding: 20px;
    border: 2px solid black;
    /* Total width = 300px (padding and border inside) */
}
```

### Layout Models

CSS provides several layout models, each suited to different scenarios:

**Normal flow** — elements stack vertically (block) or horizontally (inline):

```css
p { display: block; }   /* Full width, line break before/after */
span { display: inline; }  /* Only as wide as content, no line break */
```

Block elements (`<div>`, `<p>`, `<h1>`) take full width by default and stack vertically. Inline elements (`<span>`, `<a>`, `<strong>`) flow horizontally within text and ignore width/height.

**`inline-block`** — hybrid that flows inline but respects width/height:

```css
.icon { display: inline-block; width: 24px; height: 24px; }
```

**Flexbox** — one-dimensional layout for distributing space along a row or column:

```css
.container {
    display: flex;
    justify-content: space-between;
    align-items: center;
}
```

Flexbox excels at centering content, distributing space evenly, and reordering items. It is the go-to for navigation bars, card rows, and centering.

**Grid** — two-dimensional layout with rows and columns:

```css
.container {
    display: grid;
    grid-template-columns: 1fr 2fr 1fr;
    gap: 1rem;
}
```

Grid handles both rows and columns simultaneously, making it ideal for page layouts, image galleries, and dashboards.

**Positioning** — precise placement relative to viewport, parent, or document flow:

```css
.fixed-header { position: fixed; top: 0; width: 100%; }
.relative-parent { position: relative; }
.absolute-child { position: absolute; top: 0; right: 0; }
.sticky-nav { position: sticky; top: 0; }
```

Each positioning type removes the element from normal flow differently:
- `relative` — offsets from its natural position (space preserved)
- `absolute` — offsets from nearest positioned ancestor (no space preserved)
- `fixed` — offsets from viewport (no space preserved)
- `sticky` — toggles between relative and fixed based on scroll

### Responsive Design

CSS enables responsive design — adapting layout to different screen sizes:

```css
/* Mobile-first: base styles for small screens */
.container {
    display: block;
    padding: 1rem;
}

/* Larger screens */
@media (min-width: 768px) {
    .container {
        display: flex;
        gap: 2rem;
    }
}

@media (min-width: 1200px) {
    .container {
        max-width: 1200px;
        margin: 0 auto;
    }
}
```

Modern CSS adds container queries for component-level responsiveness:

```css
@container (min-width: 400px) {
    .card { flex-direction: row; }
}
```

### CSS Today

Modern CSS (2020s) has matured into a powerful language:

- **Custom properties** — dynamic variables with cascade-aware scoping, can be changed at runtime
- **Container queries** — element-level responsive design based on container size, not viewport
- **CSS Grid** — true two-dimensional layout with named grid areas and auto-placement
- **`clamp()`, `min()`, `max()`** — responsive math without media queries
- **`:has()`** — parent selector that styles an element based on its children
- **Cascade layers** (`@layer`) — explicit control over cascade order without specificity hacking
- **Nesting** — like Sass but native, reducing repetition in selector chains
- **Trigonometric functions** — `sin()`, `cos()`, `tan()` for mathematical styling
- **Color spaces** — `oklch()`, `display-p3` for wider gamut colors
- **Scrolling** — `scroll-snap`, `scroll-behavior`, `overscroll-behavior`
- **`@scope`** — scoped styles limited to a subtree
- **View transitions** — smooth animated transitions between page states

CSS continues to evolve through the CSS Working Group, with new features shipping in browsers every year. The pace of innovation has accelerated significantly since 2018, with features that were once only possible in preprocessors now available natively.

### CSS Syntax Details

CSS syntax is simple but precise:

```css
/* Comment */
selector {
    property: value;  /* Declaration */
    /* Multi-word properties use hyphens: font-family, not fontFamily */
    background-color: white;
}
```

**At-rules** start with `@` and provide special instructions:

```css
@import url('reset.css');       /* Import another stylesheet */
@media (min-width: 768px) { }   /* Media query */
@font-face { }                  /* Custom font definition */
@keyframes slide { }            /* Animation keyframes */
@layer base { }                 /* Cascade layer */
@container (min-width: 400px) { }  /* Container query */
```

**Shorthand properties** set multiple values at once:

```css
/* Individual properties */
border-width: 1px;
border-style: solid;
border-color: black;

/* Shorthand — same effect */
border: 1px solid black;
```

Common shorthands: `background`, `font`, `margin`, `padding`, `border`, `animation`, `flex`.

**Vendor prefixes** were common before standardization. Modern CSS rarely needs them:

```css
-webkit-transform: rotate(45deg);   /* Old WebKit */
transform: rotate(45deg);           /* Standard */
```

Autoprefixer tools handle prefix insertion automatically.

### CSS is Not Programming

CSS is not a programming language — it has no variables (well, custom properties are close), no loops, no functions (well, it has some now), and no control flow. It is a **declarative styling language**.

This distinction matters:
- CSS cannot calculate complex logic
- CSS cannot handle user interactions (JavaScript does that)
- CSS cannot manage state
- CSS errors rarely break the page — the browser ignores invalid declarations

Treat CSS as a design tool expressed in code, not as a programming language.

### Browser Rendering Pipeline

Understanding how browsers process CSS helps with debugging and performance:

```
HTML ──→ DOM
           ↓
CSS  ──→ CSSOM ──→ Render Tree ──→ Layout ──→ Paint ──→ Composite
           ↑
      Style recalculation
```

1. **Style calculation** — the browser computes which CSS rules apply to each element
2. **Layout** — the browser calculates geometry (position, size) for every element
3. **Paint** — the browser fills pixels for each element
4. **Composite** — the browser layers elements together

For performance: avoid frequent style recalculations and layout thrashing. Batch DOM reads before writes.

## Glossary

| Term | Definition |
|---|---|
| CSS | Cascading Style Sheets — language for describing HTML presentation |
| Selector | Pattern that matches HTML elements to apply styles |
| Declaration | A property-value pair (`color: blue`) |
| Rule | A selector plus one or more declarations |
| Cascade | Algorithm that resolves conflicting CSS declarations |
| Specificity | Measure of how specific a selector is (inline > ID > class > element) |
| Inheritance | Automatic propagation of certain properties from parent to child |
| Box model | The system of content, padding, border, and margin around every element |
| Flexbox | One-dimensional layout model for distributing space |
| Grid | Two-dimensional layout model with rows and columns |
| Responsive design | Design approach that adapts layout to different screen sizes |
| CSSOM | CSS Object Model — the browser's internal representation of stylesheets |
| Declarative language | Language where you describe the result, not the algorithm |

## Quick References

- [MDN: CSS](https://developer.mozilla.org/en-US/docs/Web/CSS) — comprehensive CSS reference
- [CSS Working Group](https://www.w3.org/Style/CSS/) — official specifications
- [CSS Tricks Almanac](https://css-tricks.com/almanac/) — selector and property reference
- [Can I Use](https://caniuse.com/) — browser support for CSS features
- [CSS Specificity Calculator](https://specificity.keegan.st/) — visualize specificity

## Next Steps

- [History of CSS](history-of-css.md) — how CSS evolved from the mid-1990s
- [CSS Philosophy](css-philosophy.md) — design principles behind the language
- [Selectors](selectors.md) — targeting HTML elements with CSS