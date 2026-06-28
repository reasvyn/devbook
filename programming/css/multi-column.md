# Multi-Column Layout

## Description

Multi-column layout (CSS Columns) flows content into multiple columns like a newspaper or magazine. It is ideal for long-form text that benefits from shorter line lengths.

## Prerequisites

- [Display Types & Normal Flow](display-types.md) — block flow basics
- [Typography](typography.md) — line length and readability

## Table of Contents

- [The Column Container](#the-column-container)
- [Column Count](#column-count)
- [Column Width](#column-width)
- [Column Gap](#column-gap)
- [Column Rules](#column-rules)
- [Column Span](#column-span)
- [Column Fill](#column-fill)
- [Balancing Content](#balancing-content)
- [Fragmentation](#fragmentation)
- [Breaks Inside Elements](#breaks-inside-elements)
- [Browser Support](#browser-support)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Column Container

Create a multi-column layout with `column-count` or `column-width`:

```css
article {
    columns: 2;             /* Shorthand: column-count */
}
```

The content flows from top to bottom in the first column, then continues in the next.

### Column Count

Set a fixed number of columns:

```css
.text {
    column-count: 3;        /* Always 3 columns regardless of width */
}

@media (max-width: 600px) {
    .text {
        column-count: 1;    /* Single column on small screens */
    }
}
```

The browser divides available width evenly among the specified number of columns.

### Column Width

Set a preferred column width — columns then auto-count:

```css
.text {
    column-width: 300px;    /* Columns are ~300px wide, browser creates as many as fit */
}
```

With `column-width`, the browser creates as many columns as will fit at that width. On smaller screens, fewer columns are created.

The shorthand `columns` combines both:

```css
.text {
    columns: 300px 3;       /* Preferably 3 columns at 300px each */
    /* Equivalent to column-width: 300px; column-count: 3; */
}
```

Behavior:
- If the container is wider than `column-count × column-width`, more columns appear
- If the container is narrower, columns reduce in count to fit

### Column Gap

Space between columns:

```css
.text {
    column-gap: 2rem;       /* Default: 1em (varies by font) */
    column-gap: 40px;
    column-gap: normal;
}
```

Note: `column-gap` shares the same property as grid and flexbox `column-gap` in modern CSS.

### Column Rules

A vertical line between columns:

```css
.text {
    column-rule: 1px solid #ddd;
    /* Shorthand: column-rule-width, column-rule-style, column-rule-color */
}

.text {
    column-rule-width: 1px;
    column-rule-style: solid;
    column-rule-color: #ccc;
}
```

The rule appears in the middle of the column gap. It does not take space — it is drawn in the gap.

### Column Span

Allow an element to span across all columns:

```css
.text {
    columns: 3;
}

.text h2 {
    column-span: all;       /* Spans full width, breaks column flow */
}

.text .no-split {
    column-span: none;      /* Default — stays within column flow */
}
```

`column-span: all` is useful for headings that should break out of the column layout:

```css
article {
    columns: 300px 2;
}

article h2 {
    column-span: all;
    margin: 1em 0;
}
```

The spanning element creates a break in the column flow. Content after the span starts from the top of a new column.

### Column Fill

Controls how content is distributed into columns:

```css
.text {
    column-fill: balance;       /* Default — columns are balanced (equal height) */
    column-fill: auto;          /* Fill first column completely before moving to next */
}
```

`balance` ensures that columns are approximately the same height, which is ideal for most content. `auto` fills sequentially and is mainly useful for paginated content (e.g., printing).

```css
/* For paged media */
.print-article {
    column-count: 2;
    column-fill: auto;
    height: 100vh;
}
```

### Balancing Content

Content balancing is the default behavior — the browser tries to make all columns the same height:

```css
.balanced {
    columns: 3;
    /* Browser calculates column heights to be nearly equal */
}
```

Balancing may fail if content is too short (one column) or if tall elements force imbalances.

### Fragmentation

Fragmentation controls how elements break across columns:

```css
.item {
    break-inside: avoid;        /* Prevents splitting across columns */
}

.item {
    break-inside: auto;         /* Allow breaks (default) */
    break-inside: avoid;        /* Keep item in one column */
    break-inside: avoid-page;   /* Avoid page breaks (print) */
    break-inside: avoid-column; /* Avoid column breaks */
}
```

Control breaks before and after elements:

```css
h2 {
    break-after: avoid;         /* Keep heading with following content */
}

.image-group {
    break-before: column;       /* Start in a new column */
}
```

### Breaks Inside Elements

Prevent awkward breaks:

```css
.card {
    break-inside: avoid;        /* Card stays whole */
}

pre, code, blockquote {
    break-inside: avoid;        /* Code blocks stay together */
}

figure {
    break-inside: avoid;        /* Images and captions stay together */
}

li {
    break-inside: avoid;        /* List items stay together */
}
```

Without `break-inside: avoid`, elements split at column boundaries — a paragraph might start at the bottom of one column and continue in the next.

### Browser Support

Multi-column layout is supported in all modern browsers:

- Chrome 50+
- Firefox 52+
- Safari 9+
- Edge 12+

Some properties have quirks:
- Firefox supports `column-span: all` since 71
- Safari may have rendering glitches with `column-rule` and rounded corners
- `break-inside: avoid` is less reliable in Safari

## Study Cases

### Article Layout

```css
.article {
    max-width: 1200px;
    margin: 0 auto;
    padding: 2rem;
}

.article-body {
    columns: 300px 2;
    column-gap: 2rem;
    column-rule: 1px solid #e0e0e0;
}

.article-body h2 {
    column-span: all;
    margin: 1.5em 0 0.5em;
}

.article-body figure {
    break-inside: avoid;
    margin: 1em 0;
}

.article-body blockquote {
    break-inside: avoid;
    font-style: italic;
    color: #666;
    border-left: 4px solid #ddd;
    padding-left: 1rem;
}

@media (max-width: 768px) {
    .article-body {
        columns: 1;             /* Single column on mobile */
        column-rule: none;
    }
}
```

### FAQ Section

```css
.faq-grid {
    columns: 300px 2;
    column-gap: 2rem;
}

.faq-item {
    break-inside: avoid;
    margin-bottom: 1.5rem;
    padding: 1rem;
    background: #f8f9fa;
    border-radius: 8px;
}
```

Each FAQ item stays whole — no question in one column and answer in the next.

### Card Grid with Columns

Before CSS Grid was well-supported, columns were used for card layouts:

```css
.card-grid {
    columns: 250px 3;
    column-gap: 1.5rem;
}

.card {
    break-inside: avoid;
    margin-bottom: 1.5rem;
    border: 1px solid #ddd;
    border-radius: 8px;
    overflow: hidden;
}
```

This creates a masonry-like layout. Cards stack vertically within each column, creating different column heights (like Pinterest, but reading top-to-bottom, left-to-right).

## Examples

### Example 1: Two-column text

```css
.write-up {
    columns: 2;
    column-gap: 2rem;
    line-height: 1.75;
}
```

### Example 2: Three-column with rule

```css
.press-release {
    columns: 3;
    column-gap: 1.5rem;
    column-rule: 2px dotted #ccc;
}
```

### Example 3: Mixed column layouts

```css
.content {
    /* First section: 2 columns */
    columns: 2;
}

.content .full-width {
    column-span: all;
}

.content .three-col-section {
    columns: 3;
}
```

## Glossary

| Term | Definition |
|---|---|
| Column-count | Fixed number of columns |
| Column-width | Preferred column width (determines count) |
| Column-gap | Space between columns |
| Column-rule | Vertical line between columns |
| Column-span | Element spanning across all columns |
| Column-fill | Content distribution method (balance or sequential) |
| Fragmentation | Element breaking across columns/pages |
| break-inside | Control whether element splits across columns |

## Quick References

- [MDN: Multi-Column Layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_multicol_layout) — complete guide
- [CSS Multi-column Layout Level 1](https://www.w3.org/TR/css-multicol-1/) — specification
- [Caniuse: Multicolumn](https://caniuse.com/multicolumn) — browser support
- [MDN: Fragmentation](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_fragmentation) — break control
- [Smashing Magazine: Multi-column Guide](https://www.smashingmagazine.com/2019/01/css-multiple-column-layout-multicol/) — practical patterns

## Next Steps

- [Float & Clear](floats.md) — legacy layout technique