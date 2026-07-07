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
- [Use Cases Beyond Text](#use-cases-beyond-text)
- [Advanced Column Patterns](#advanced-column-patterns)
- [Fragmentation Deep Dive](#fragmentation-deep-dive)
- [Multi-Column vs Grid and Flexbox](#multi-column-vs-grid-and-flexbox)
- [Browser Support and Quirks](#browser-support-and-quirks)
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

### Use Cases Beyond Text

Multi-column is not limited to long-form text. It works well for several other patterns:

**Card and Grid Lists**

```css
.card-grid {
    columns: 280px 3;
    column-gap: 1.5rem;
}

.card-grid .card {
    break-inside: avoid;
    margin-bottom: 1.5rem;
    padding: 1rem;
    border: 1px solid #e0e0e0;
    border-radius: 8px;
}
```

Cards flow top-to-bottom in columns. Unlike CSS Grid, items fill column 1 completely before moving to column 2. This creates a masonry-like effect without JavaScript.

**Checklist or Feature Lists**

```css
.feature-list {
    columns: 2;
    column-gap: 2rem;
    list-style: none;
    padding: 0;
}

.feature-list li {
    break-inside: avoid;
    padding: 0.5rem 0;
    padding-left: 1.5rem;
    background: url('check.svg') left center no-repeat;
}
```

**Tag Clouds and Metadata Lists**

```css
.tag-cloud {
    columns: 150px 4;
    column-gap: 1rem;
}

.tag-cloud a {
    display: block;
    break-inside: avoid;
    padding: 0.25rem 0;
    text-decoration: none;
}

.tag-cloud a:hover { text-decoration: underline; }
```

**Data Definition Lists**

```css
dl.metadata {
    columns: 2;
    column-gap: 2rem;
}

dl.metadata dt {
    font-weight: bold;
    color: #666;
    font-size: 0.8rem;
    text-transform: uppercase;
    break-after: avoid;
}

dl.metadata dd {
    margin-left: 0;
    margin-bottom: 1rem;
    break-inside: avoid;
}
```

**Image Galleries**

```css
.gallery {
    columns: 200px 4;
    column-gap: 0.5rem;
}

.gallery img {
    width: 100%;
    height: auto;
    display: block;
    margin-bottom: 0.5rem;
    border-radius: 4px;
}
```

This creates a masonry-type gallery where images flow into columns. Unlike a true masonry layout, images within a column are stacked vertically with no horizontal gaps.

### Advanced Column Patterns

**Mixed Column Layout**

Combine fixed and spanned elements for magazine-style layouts:

```css
.magazine-layout {
    columns: 300px 3;
    column-gap: 2rem;
    column-rule: 1px solid #ccc;
}

.magazine-layout h2 {
    column-span: all;
    font-size: 1.5rem;
    margin: 1.5rem 0 1rem;
    padding-bottom: 0.5rem;
    border-bottom: 2px solid #333;
}

.magazine-layout .pull-quote {
    column-span: all;
    font-size: 1.25rem;
    font-style: italic;
    text-align: center;
    padding: 1.5rem 3rem;
    margin: 1rem 0;
    border-top: 2px solid #ddd;
    border-bottom: 2px solid #ddd;
}

.magazine-layout .featured-image {
    column-span: all;
    width: 100%;
    margin: 1rem 0;
}
```

**Nested Multi-column**

Multi-column can be nested within other layout methods:

```css
.page {
    display: grid;
    grid-template-columns: 250px 1fr;
}

.sidebar {
    /* Single column sidebar */
}

.content {
    columns: 300px 2;
    column-gap: 1.5rem;
}

.content .aside-note {
    break-inside: avoid;
    background: #f5f5f5;
    padding: 1rem;
    border-left: 3px solid #007bff;
}
```

**Column-based Image + Text Pairs**

```css
.media-grid {
    columns: 2;
    column-gap: 2rem;
}

.media-item {
    break-inside: avoid;
    margin-bottom: 1.5rem;
    display: flex;
    flex-direction: column;
}

.media-item img {
    width: 100%;
    height: 200px;
    object-fit: cover;
}

.media-item .caption {
    font-size: 0.875rem;
    color: #666;
    padding: 0.5rem;
}
```

### Fragmentation Deep Dive

Fragmentation controls how content breaks across columns, pages, or regions. The `break-*` properties (formerly `page-break-*` and `column-break-*`) provide fine-grained control:

**break-before** — controls breaks before an element:

```css
/* Start a new column before this heading */
h2 {
    break-before: column;
}

/* Start a new page before this section (print) */
.chapter {
    break-before: page;
}

/* Avoid a break before this element */
.intro {
    break-before: avoid;
}

/* Auto: allow breaks (default) */
.normal {
    break-before: auto;
}
```

**break-after** — controls breaks after an element:

```css
/* End column after this element */
.section-title {
    break-after: column;
}

/* Avoid break after (keep heading with next paragraph) */
h3 {
    break-after: avoid;
}

/* Always page break after (print) */
.end-of-chapter {
    break-after: page;
}
```

**break-inside** — controls breaks within an element:

```css
/* Prevent element from splitting */
.card {
    break-inside: avoid;
}

/* Allow natural breaks */
.long-text {
    break-inside: auto;
}

/* Avoid page breaks inside (print) */
.table-container {
    break-inside: avoid-page;
}
```

**Widows and Orphans**

The `widows` and `orphans` properties control how many lines of a paragraph must appear at the top or bottom of a column:

```css
p {
    widows: 2;    /* Minimum 2 lines at top of new column */
    orphans: 2;   /* Minimum 2 lines at bottom of current column */
}
```

- **Orphans** — lines left alone at the bottom of a column when the rest moves to the next column
- **Widows** — lines left alone at the top of a new column when the rest stayed in the previous column

Default values are `2` for both. Setting them to `1` allows single-line orphans/widows but can look awkward.

**Avoiding Common Fragmentation Problems**

| Problem | Solution |
|---|---|
| Heading separated from its paragraph | `h2 { break-after: avoid; }` |
| Figure split across columns | `figure { break-inside: avoid; }` |
| Table row broken across columns | `tr { break-inside: avoid; }` or use `display: inline-table` |
| List item split across columns | `li { break-inside: avoid; }` |
| Code blocks split mid-line | `pre { break-inside: avoid; }` |
| Last column too short | Use `column-fill: balance` (default) |
| Blockquote starts at column bottom | `blockquote { break-inside: avoid; }` |

### Multi-Column vs Grid and Flexbox

Multi-column, Grid, and Flexbox serve different layout purposes:

| Aspect | Multi-Column | CSS Grid | Flexbox |
|---|---|---|---|
| Content flow | Top-to-bottom, then next column | Explicit row/column placement | Single axis (row or column) |
| Item order | Sequential (fill first column) | Define exact positions | Sequential by source order |
| Equal height items | Columns balanced by default | Items stretch to grid cell | Stretch on cross-axis |
| Wrap behavior | Automatic columns based on width | Explicit tracks | `flex-wrap: wrap` |
| Span across tracks | `column-span: all` | `grid-column: span N` | Not applicable |
| Content-aware heights | Yes (columns balance) | Requires explicit rows | Yes (grow/shrink) |
| Best for | Reading text, lists, lightweight masonry | Page layout, dashboards, app shells | Navigation, toolbars, small components |
| Source order independence | No | Yes (via grid placement) | No |

Use multi-column when:
- Content is primarily text that benefits from shorter line lengths
- You want content to flow naturally from one column to the next
- You need a quick, CSS-only masonry-like layout for a list of items
- Line-length optimization for readability (50-75 characters per line)

Use CSS Grid when:
- Items need to align in both rows and columns
- You need explicit control over placement
- Items should fill left-to-right rather than top-to-bottom

Use Flexbox when:
- Content aligns along a single axis
- Items need to grow, shrink, or wrap
- You need distribution of space (justify-content, align-items)

### Browser Support and Quirks

| Browser | Multi-Column Support | Notable Issues |
|---|---|---|
| Chrome 50+ | Full | No significant issues |
| Firefox 52+ | Full | `column-span: all` since Firefox 71; some `<details>` rendering bugs |
| Safari 9+ | Good | `column-rule` rendering glitches with `border-radius`; `break-inside` less reliable |
| Edge 12+ (Chromium) | Full | No significant issues |
| Edge Legacy (12-18) | Limited | No `column-span: all`; inconsistent column balancing |

**Known Quirks and Workarounds**

1. **Safari and column-rule**: Column rules may not render correctly when columns have background or border-radius. Workaround: avoid `column-rule` in Safari or use a simulated rule with column gap background.

2. **Safari break-inside**: `break-inside: avoid` works inconsistently in Safari. Items may split across columns despite the rule. Use `display: inline-block` on items as a workaround:
   ```css
   .card {
       display: inline-block;
       width: 100%;
   }
   ```

3. **Column height overflow**: When content in one column exceeds the container height, columns may not balance correctly. Set an explicit height or `max-height` on the container to force balancing.

4. **Column-span and print**: `column-span: all` works in browsers for screen but may be ignored in print media. Add print-specific rules:
   ```css
   @media print {
       .multi-col {
           column-span: none;
       }
   }
   ```

5. **First-letter and column-span**: The `::first-letter` pseudo-element may not work inside multi-column layouts in some browsers. Apply first-letter styling to individual paragraphs instead.

6. **Scrolling columns**: To create horizontal scrollable columns (like a digital magazine):
   ```css
   .scroll-columns {
       columns: 300px;
       column-gap: 2rem;
       height: 80vh;
       overflow-x: auto;
       white-space: nowrap;  /* Prevent content wrapping */
   }
   .scroll-columns > * {
       white-space: normal;  /* Reset for child elements */
   }
   ```

## Learning Tips

- **Prefer `column-width` over `column-count`.** Using `column-width` creates responsive columns automatically — fewer columns on narrow screens, more on wide screens. Reserve `column-count` for designs where a precise number of columns is required.
- **Always use `break-inside: avoid` on cards and figures.** Without it, items split across columns in ugly ways. Apply it to cards, images, blockquotes, code blocks, and list items.
- **Test in multiple browsers.** Safari has known `break-inside` and `column-rule` quirks. Always check your multi-column layout in Chrome, Firefox, and Safari.
- **Use `column-span: all` for headings.** Spanning headings creates clear visual breaks and improves readability. Apply it to `h2`, `h3`, and any section headings.
- **Balance performance.** Multi-column layout with very long content can be computationally expensive because the browser must calculate column heights. For documents longer than 50 pages of print output, consider page-level breaking instead.
- **Combine with print styles.** Multi-column is excellent for print stylesheets (`@media print`). Use narrower columns and tighter gaps for printed output.
- **Use DevTools to inspect columns.** Chrome DevTools shows column boundaries in the Layout panel. Check that content is distributing evenly and no columns are overfilled.

## Glossary

| Term | Definition |
|---|---|
| Column-count | Fixed number of columns (`column-count: 3`) |
| Column-width | Preferred column width — browser calculates count (`column-width: 300px`) |
| Column-gap | Space between adjacent columns |
| Column-rule | Vertical line drawn in the column gap |
| Column-span | Element spanning across all columns (`column-span: all`) |
| Column-fill | Content distribution method: `balance` (default) or `auto` (sequential) |
| Fragmentation | How content breaks across columns, pages, or regions |
| break-inside | Controls whether an element can split across a column break |
| break-before | Forces or avoids a break before an element |
| break-after | Forces or avoids a break after an element |
| Orphans | Minimum lines at the bottom of a column before a break |
| Widows | Minimum lines at the top of a column after a break |
| Multi-column container | Element with `columns`, `column-count`, or `column-width` set |
| Column box | Individual column within a multi-column container (not a DOM element) |
| Column rule | Visual separator line in the column gap (`column-rule`) |
| Column spanning | Element that breaks the column flow to span full width |
| Content balancing | Process of distributing content evenly across columns |
| Paged media | Content divided into pages (print, PDF) — multi-col works with `@page` |
| Masonry layout | Grid with uneven item heights (multi-col approximates this effect) |

## Quick References

- [MDN: Multi-Column Layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_multicol_layout) — complete guide
- [CSS Multi-column Layout Level 1](https://www.w3.org/TR/css-multicol-1/) — specification
- [Caniuse: Multicolumn](https://caniuse.com/multicolumn) — browser support
- [MDN: Fragmentation](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_fragmentation) — break control
- [Smashing Magazine: Multi-column Guide](https://www.smashingmagazine.com/2019/01/css-multiple-column-layout-multicol/) — practical patterns
- [Rachel Andrew: The Multi-column Layout Model](https://rachelandrew.co.uk/archives/2017/04/07/the-multi-column-layout-model/) — deep dive into how multi-col works
- [CSS Fragmentation Specification](https://www.w3.org/TR/css-break-3/) — official fragmentation spec
- [MDN: break-inside](https://developer.mozilla.org/en-US/docs/Web/CSS/break-inside) — break-inside reference
- [MDN: orphans and widows](https://developer.mozilla.org/en-US/docs/Web/CSS/orphans) — orphans and widows reference
- [web.dev: Multi-column Layout](https://web.dev/learn/css/multicolumn-layout/) — Google's multi-column guide

## Next Steps

- [Float & Clear](floats.md) — legacy layout technique