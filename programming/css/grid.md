# CSS Grid

## Description

CSS Grid is a two-dimensional layout system for arranging elements in rows and columns simultaneously. It is the most powerful layout tool in CSS.

## Prerequisites

- [Flexbox](flexbox.md) — one-dimensional layout (comparison context)
- [Display Types & Normal Flow](display-types.md) — grid display context

## Table of Contents

- [The Grid Container](#the-grid-container)
- [Grid Lines](#grid-lines)
- [Grid Tracks](#grid-tracks)
- [Grid Gaps](#grid-gaps)
- [Grid Template Columns and Rows](#grid-template-columns-and-rows)
- [The fr Unit](#the-fr-unit)
- [Repeat Function](#repeat-function)
- [Minmax Function](#minmax-function)
- [Auto-Fill and Auto-Fit](#auto-fill-and-auto-fit)
- [Grid Template Areas](#grid-template-areas)
- [Grid Item Placement](#grid-item-placement)
- [Grid Row and Column Shorthands](#grid-row-and-column-shorthands)
- [Span](#span)
- [Implicit Grid](#implicit-grid)
- [Auto Rows and Columns](#auto-rows-and-columns)
- [Aligning the Grid](#aligning-the-grid)
- [Aligning Grid Items](#aligning-grid-items)
- [Order in Grid](#order-in-grid)
- [Subgrid](#subgrid)
- [Masonry](#masonry)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Grid Container

Define a grid container:

```css
.container {
    display: grid;              /* Block-level grid container */
    display: inline-grid;       /* Inline-level grid container */
}
```

All direct children become grid items. Grid items can be positioned into cells defined by the grid.

### Grid Lines

A grid is divided by lines — horizontal and vertical:

```
Column lines:  1 ──── 2 ──── 3 ──── 4
Row lines:     1 ──── 2 ──── 3 ──── 4
```

Lines are numbered starting from 1. They can also be named:

```css
.container {
    grid-template-columns: [sidebar] 1fr [content] 2fr [end];
    grid-template-rows: [header] auto [main] 1fr [footer] auto [end];
}

.item {
    grid-column: sidebar / content;     /* Use named lines */
    grid-row: header / footer;
}
```

Lines are referenced by items for placement. Each track is bounded by two lines.

### Grid Tracks

Tracks are the rows and columns of the grid:

```css
.container {
    display: grid;
    grid-template-columns: 200px 1fr 1fr;   /* Three column tracks */
    grid-template-rows: auto 1fr auto;       /* Three row tracks */
}
```

Track sizes can be:
- Fixed: `200px`, `10rem`
- Flexible: `1fr`, `2fr`
- Content-based: `auto`, `min-content`, `max-content`, `fit-content(300px)`
- Range: `minmax(200px, 1fr)`

### Grid Gaps

Space between grid cells (not around edges):

```css
.container {
    display: grid;
    gap: 1rem;                      /* Both row and column gaps */
    row-gap: 1rem;
    column-gap: 2rem;
    gap: 1rem 2rem;                 /* row column */
}
```

Gaps are applied between tracks. They are distinct from margins on items.

### Grid Template Columns and Rows

Define the track structure explicitly:

```css
.container {
    grid-template-columns: 200px 1fr 1fr;
    /* Three columns: 200px fixed, two equal flexible columns */
}

.container {
    grid-template-columns: 1fr 2fr 1fr;
    /* Three columns: middle is twice as wide as sides */
}
```

```css
.container {
    grid-template-columns:
        minmax(200px, 1fr)
        minmax(300px, 2fr)
        minmax(200px, 1fr);
}
```

### The fr Unit

`fr` distributes available space after fixed-size items and gaps:

```css
.container {
    grid-template-columns: 200px 1fr 1fr;
    /* 200px fixed, then remaining space split equally */
}

.container {
    grid-template-columns: 1fr 2fr;
    /* 2fr column is twice as wide as 1fr column */
}
```

`fr` only works in grid track sizing. It is not a length — it resolves based on available space.

**Important:** `fr` includes gap calculations. `grid-template-columns: 1fr 1fr` with `gap: 1rem` splits remaining space after the gap is accounted for.

### Repeat Function

Repeat track patterns:

```css
.container {
    grid-template-columns: repeat(3, 1fr);
    /* Same as: 1fr 1fr 1fr */
}

.container {
    grid-template-columns: repeat(2, 1fr 2fr);
    /* Same as: 1fr 2fr 1fr 2fr */
}

.container {
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    /* Responsive — fills row with as many 250px tracks as fit */
}
```

`repeat()` can combine fixed and flexible:

```css
.container {
    grid-template-columns: 200px repeat(3, 1fr) 200px;
}
```

### Minmax Function

Define a track size range:

```css
.container {
    grid-template-columns: minmax(200px, 1fr) minmax(300px, 2fr);
    /* First column: at least 200px, at most 1fr
       Second column: at least 300px, at most 2fr */
}

.container {
    grid-template-columns: repeat(3, minmax(0, 1fr));
    /* Equal columns that can shrink to zero */
}
```

Common patterns:
- `minmax(200px, 1fr)` — column has a minimum width but grows
- `minmax(0, 1fr)` — equal columns that ignore content size
- `minmax(auto, max-content)` — no smaller than content, no larger than needed

### Auto-Fill and Auto-Fit

Responsive grids without media queries:

```css
/* auto-fill — create as many tracks as possible */
.container {
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
}

/* auto-fit — same but collapse empty tracks */
.container {
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
}
```

Difference:
- `auto-fill` preserves track space even if there are no items (empty tracks keep their space)
- `auto-fit` collapses empty tracks to zero width, allowing remaining items to expand

```
Container 800px, items: 3, minmax(250px, 1fr)

auto-fill: [250px] [250px] [250px] [50px empty] — tracks preserve space
auto-fit: [266px] [267px] [267px] — empty tracks collapse
```

### Grid Template Areas

Define named grid areas for high-level layout:

```css
.container {
    display: grid;
    grid-template-columns: 250px 1fr;
    grid-template-rows: auto 1fr auto;
    grid-template-areas:
        "header  header"
        "sidebar main"
        "footer  footer";
    gap: 1rem;
    min-height: 100vh;
}

header { grid-area: header; }
aside  { grid-area: sidebar; }
main   { grid-area: main; }
footer { grid-area: footer; }
```

Rules for `grid-template-areas`:
- Each row has the same number of columns
- Area names must be connected — no L-shapes
- Use `.` (dot) for empty cells
- A name repeated means spanning multiple cells

```css
.container {
    grid-template-areas:
        "header header header"
        ".      main   sidebar"
        "footer footer footer";
}
```

### Grid Item Placement

Place items explicitly:

```css
.item {
    grid-column-start: 2;
    grid-column-end: 4;
    grid-row-start: 1;
    grid-row-end: 3;
}
```

```css
/* Item spans from column line 2 to 4, row line 1 to 3 */
/* Occupies 2 columns and 2 rows */
```

Using named grid lines:

```css
.container {
    grid-template-columns: [start] 1fr [middle] 1fr [end];
    grid-template-rows: [top] auto [bottom] auto;
}

.item {
    grid-column: start / end;     /* Spans full width */
    grid-row: top / bottom;       /* Spans full height */
}
```

### Grid Row and Column Shorthands

```css
.item {
    grid-column: 1 / 3;           /* grid-column-start: 1; grid-column-end: 3 */
    grid-row: 1 / 3;

    /* Span keywords */
    grid-column: 1 / span 2;      /* Start at 1, span 2 columns */
    grid-row: 1 / span 2;

    /* Single value means start at that line, span 1 track */
    grid-column: 2;               /* Same as: grid-column: 2 / span 1 (or auto) */

    /* Auto placement */
    grid-column: auto;            /* Let grid place it */
}
```

### Span

Span across tracks without knowing the end line:

```css
.item {
    grid-column: 1 / span 2;      /* Start at line 1, span 2 tracks */
    grid-row: span 2;             /* Span 2 rows (start placed by auto) */
}
```

`span` can be used with named lines too:

```css
.item {
    grid-column: sidebar / span 2;
}
```

### Implicit Grid

When items are placed outside the defined tracks, the grid creates implicit tracks:

```css
.container {
    grid-template-columns: repeat(3, 1fr);   /* 3 explicit columns */
    /* If there are 10 items, rows 2, 3, and 4 are implicit */
}
```

Implicit track sizing:

```css
.container {
    grid-auto-rows: auto;           /* Default — size to content */
    grid-auto-rows: 200px;          /* All implicit rows 200px */
    grid-auto-rows: minmax(100px, auto);
}

.container {
    grid-auto-columns: 1fr;         /* Size implicit columns */
}
```

### Auto Rows and Columns

Control implicit track sizes:

```css
.container {
    grid-template-columns: repeat(3, 1fr);
    grid-auto-rows: minmax(100px, auto);
    /* Explicit columns: 3 tracks
       Implicit rows: at least 100px, growing with content */
}
```

The `grid-auto-flow` property controls auto-placement direction:

```css
.container {
    grid-auto-flow: row;             /* Default — fill rows first */
    grid-auto-flow: column;          /* Fill columns first */
    grid-auto-flow: dense;           /* Fill gaps by reordering */
    grid-auto-flow: row dense;
    grid-auto-flow: column dense;
}
```

`dense` reorders items to fill gaps. Use carefully — it changes visual order (but not DOM order).

### Aligning the Grid

Align the entire grid within its container:

```css
.container {
    justify-content: start;          /* Horizontal alignment of grid tracks */
    justify-content: end;
    justify-content: center;
    justify-content: stretch;        /* Default — tracks fill width */
    justify-content: space-between;
    justify-content: space-around;
    justify-content: space-evenly;
}

.container {
    align-content: start;            /* Vertical alignment of grid tracks */
    align-content: end;
    align-content: center;
    align-content: stretch;
    align-content: space-between;
    align-content: space-around;
    align-content: space-evenly;
}
```

These only have effect when the container is larger than the sum of grid tracks.

### Aligning Grid Items

Align items within their cells:

```css
.container {
    justify-items: stretch;      /* Default — fill cell width */
    justify-items: start;
    justify-items: end;
    justify-items: center;
}

.container {
    align-items: stretch;        /* Default — fill cell height */
    align-items: start;
    align-items: end;
    align-items: center;
}
```

Per-item overrides:

```css
.item {
    justify-self: start;         /* Override justify-items */
    align-self: center;          /* Override align-items */
    place-self: center;          /* Shorthand: align-self justify-self */
}
```

### Order in Grid

Like flexbox, `order` changes visual order:

```css
.item:nth-child(3) {
    order: -1;                   /* Appears first (before default order 0) */
}
```

Same accessibility caution as flexbox — `order` is visual only.

### Subgrid

Subgrid allows grid items to inherit track definitions from their parent grid:

```css
.parent {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 1rem;
}

.child {
    display: grid;
    grid-column: 1 / -1;          /* Span all columns */
    grid-template-columns: subgrid;  /* Inherit parent's track definitions */
    /* Now child's children align with parent's grid lines */
}
```

Benefits of subgrid:
- Nested grid aligns with parent grid lines
- Consistent row heights across grid sections
- Avoids nested-grid misalignment

Without subgrid, each grid container creates independent tracks.

### Masonry

CSS Grid Level 3 includes masonry layout (experimental):

```css
.container {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-rows: masonry;
}
```

Masonry lays out items in a packed arrangement like Pinterest, ignoring row alignment. Items flow into the next available column at the top.

Browser support is limited (Firefox behind a flag). For production, use JavaScript-based masonry or a simple column layout:

```css
.masonry-fallback {
    columns: 3;
    column-gap: 1rem;
}

.masonry-fallback > * {
    break-inside: avoid;
    margin-bottom: 1rem;
}
```

## Study Cases

### Page Layout — Holy Grail

```css
.page {
    display: grid;
    grid-template-columns: 250px 1fr;
    grid-template-rows: auto 1fr auto;
    grid-template-areas:
        "header header"
        "nav    main"
        "footer footer";
    min-height: 100vh;
    gap: 0;
}

@media (max-width: 768px) {
    .page {
        grid-template-columns: 1fr;
        grid-template-areas:
            "header"
            "nav"
            "main"
            "footer";
    }
}

header { grid-area: header; }
nav    { grid-area: nav; }
main   { grid-area: main; }
footer { grid-area: footer; }
```

### Card Grid with Auto-Fit

```css
.card-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    gap: 1.5rem;
    padding: 1.5rem;
}

.card {
    display: grid;
    grid-template-rows: subgrid;
    grid-row: span 3;
    gap: 0;
    border: 1px solid #ddd;
    border-radius: 8px;
    overflow: hidden;
}
```

### Form Layout with Grid

```css
.form {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 1rem;
}

.form .full-width {
    grid-column: 1 / -1;           /* Span both columns */
}

.form label {
    grid-column: 1;
}

.form input, .form select {
    grid-column: 2;
}

@media (max-width: 600px) {
    .form {
        grid-template-columns: 1fr;
    }
}
```

## Examples

### Example 1: Equal-height columns

```css
.grid-3 {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 1rem;
}
/* All columns are equal width AND height — implicit! */
```

### Example 2: Spanning layout

```css
.featured-grid {
    display: grid;
    grid-template-columns: repeat(4, 1fr);
    gap: 1rem;
}

.featured-grid .main {
    grid-column: 1 / 3;
    grid-row: 1 / 3;
}
```

### Example 3: Named grid areas for documentation layout

```css
.doc-layout {
    display: grid;
    grid-template-columns: 250px 1fr 200px;
    grid-template-areas:
        "nav    content  toc"
        "nav    content  toc";
    gap: 2rem;
    max-width: 1400px;
    margin: 0 auto;
}

.doc-nav  { grid-area: nav; }
.doc-body { grid-area: content; }
.doc-toc  { grid-area: toc; }
```

## Glossary

| Term | Definition |
|---|---|
| Grid container | Element with `display: grid` |
| Grid item | Direct child of grid container |
| Grid line | Horizontal or vertical dividing line |
| Grid track | Space between two adjacent grid lines (row or column) |
| Grid cell | Intersection of a row and column |
| Grid area | Rectangular group of grid cells |
| Explicit grid | Tracks defined by `grid-template-*` |
| Implicit grid | Auto-generated tracks for items outside explicit grid |
| fr unit | Fraction of available space |
| Subgrid | Grid that inherits parent's track definitions |

## Quick References

- [MDN: CSS Grid](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_grid_layout) — complete guide
- [CSS Grid Garden](https://cssgridgarden.com/) — interactive learning
- [CSS Grid Generator](https://cssgrid-generator.netlify.app/) — visual tool
- [Grid by Example](https://gridbyexample.com/) — patterns and examples
- [CSS Grid Level 1 Spec](https://www.w3.org/TR/css-grid-1/) — specification
- [Caniuse: Subgrid](https://caniuse.com/css-subgrid) — subgrid support

## Next Steps

- [Positioning](positioning.md) — static, relative, absolute, fixed, sticky