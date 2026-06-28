# Flexbox

## Description

Flexbox is a one-dimensional layout model for distributing space and aligning items in rows or columns. It excels at dynamic layouts where item sizes are unknown.

## Prerequisites

- [Display Types & Normal Flow](display-types.md) — flex display context
- [The Box Model](box-model.md) — flex item sizing

## Table of Contents

- [The Flex Container](#the-flex-container)
- [Main Axis and Cross Axis](#main-axis-and-cross-axis)
- [Flex Direction](#flex-direction)
- [Flex Wrap](#flex-wrap)
- [Justify Content](#justify-content)
- [Align Items](#align-items)
- [Align Content](#align-content)
- [Align Self](#align-self)
- [Gap](#gap)
- [Flex Grow](#flex-grow)
- [Flex Shrink](#flex-shrink)
- [Flex Basis](#flex-basis)
- [The Flex Shorthand](#the-flex-shorthand)
- [Order](#order)
- [Flex Item Margins](#flex-item-margins)
- [Nested Flexbox](#nested-flexbox)
- [Auto Margins in Flexbox](#auto-margins-in-flexbox)
- [Min-Max Sizing and Flex](#min-max-sizing-and-flex)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Flex Container

Flexbox requires a container with `display: flex`:

```css
.container {
    display: flex;          /* Block-level flex container */
    display: inline-flex;   /* Inline-level flex container */
}
```

All direct children become flex items. Grandchildren are not affected — flex only applies to children of the container.

### Main Axis and Cross Axis

Flexbox has two axes:

```
flex-direction: row (default)
    Main axis → (horizontal)
    Cross axis ↓ (vertical)

flex-direction: column
    Main axis ↓ (vertical)
    Cross axis → (horizontal)
```

- **Main axis** — defined by `flex-direction`. Items are placed along this axis.
- **Cross axis** — perpendicular to main axis. Alignment perpendicular to items.

### Flex Direction

Controls the direction items flow:

```css
.container {
    flex-direction: row;            /* Default — left to right */
    flex-direction: row-reverse;    /* Right to left */
    flex-direction: column;         /* Top to bottom */
    flex-direction: column-reverse; /* Bottom to top */
}
```

`row-reverse` and `column-reverse` change the start and end points. Items appear in reverse order visually (but not in DOM order — relevant for accessibility).

### Flex Wrap

Controls whether items wrap when they exceed the container:

```css
.container {
    flex-wrap: nowrap;          /* Default — items shrink to fit */
    flex-wrap: wrap;            /* Items wrap to new lines */
    flex-wrap: wrap-reverse;    /* Wrap in reverse cross-axis direction */
}
```

`flex-wrap: wrap` enables multi-line flex containers. Without it, items may overflow or shrink to fit.

Shorthand:

```css
.container {
    flex-flow: row wrap;        /* flex-direction + flex-wrap */
}
```

### Justify Content

Alignment along the main axis:

```css
.container {
    justify-content: flex-start;     /* Default — items at start */
    justify-content: flex-end;       /* Items at end */
    justify-content: center;         /* Items centered */
    justify-content: space-between;  /* Equal space between items */
    justify-content: space-around;   /* Equal space around each item */
    justify-content: space-evenly;   /* Equal space everywhere */
}
```

```
flex-start:    [item][item][item]
center:            [item][item][item]
space-between: [item]   [item]   [item]
space-around:  [item]   [item]   [item]
space-evenly:  [item]  [item]  [item]
```

### Align Items

Alignment along the cross axis (single line):

```css
.container {
    align-items: stretch;        /* Default — items fill cross-axis */
    align-items: flex-start;     /* Items at cross-axis start */
    align-items: flex-end;       /* Items at cross-axis end */
    align-items: center;         /* Items centered on cross-axis */
    align-items: baseline;       /* Items aligned by text baseline */
}
```

`stretch` only works when the item does not have an explicit size on the cross axis.

### Align Content

Multi-line alignment along the cross axis (only works with `flex-wrap: wrap`):

```css
.container {
    align-content: stretch;          /* Default — lines stretch */
    align-content: flex-start;       /* Lines at start */
    align-content: flex-end;         /* Lines at end */
    align-content: center;           /* Lines centered */
    align-content: space-between;    /* Equal space between lines */
    align-content: space-around;     /* Equal space around each line */
    align-content: space-evenly;     /* Equal space everywhere */
}
```

`align-content` distributes lines when there is extra space on the cross axis. It does nothing in a single-line flex container.

### Align Self

Override `align-items` for individual items:

```css
.item {
    align-self: auto;            /* Default — inherits align-items */
    align-self: stretch;
    align-self: flex-start;
    align-self: flex-end;
    align-self: center;
    align-self: baseline;
}
```

```css
.container { align-items: flex-start; }
.item.center { align-self: center; }       /* Override for this item */
.item.end { align-self: flex-end; }        /* Override for this item */
```

### Gap

Space between flex items (not around edges):

```css
.container {
    display: flex;
    gap: 1rem;                  /* Both row and column gap */
    gap: 1rem 2rem;             /* row-gap column-gap */
    row-gap: 1rem;
    column-gap: 2rem;
}
```

`gap` is the cleanest way to space flex items. It avoids margin complications on first/last children.

### Flex Grow

Controls how items expand to fill available space:

```css
.item {
    flex-grow: 0;               /* Default — does not grow */
    flex-grow: 1;               /* Takes up remaining space */
    flex-grow: 2;               /* Grows twice as much as flex-grow: 1 */
}
```

Distribution: remaining space is divided proportionally by flex-grow values.

```css
/* Three items, container 600px wide, items 100px each */
/* Remaining space: 600 - 300 = 300px */
.item { flex-grow: 1; }         /* Each gets 100px extra → 200px each */
.item.special { flex-grow: 2; } /* This gets 200px extra → 300px */
```

### Flex Shrink

Controls how items shrink when there is not enough space:

```css
.item {
    flex-shrink: 1;             /* Default — can shrink */
    flex-shrink: 0;             /* Cannot shrink — maintains flex-basis */
    flex-shrink: 2;             /* Shrinks twice as much as flex-shrink: 1 */
}
```

`flex-shrink: 0` is commonly used on elements that should not compress (e.g., icons, sidebars):

```css
.sidebar {
    flex: 0 0 250px;            /* Fixed 250px, does not shrink */
}

.content {
    flex: 1 1 auto;             /* Shrinks and grows as needed */
}
```

### Flex Basis

The initial size of a flex item before remaining space is distributed:

```css
.item {
    flex-basis: auto;           /* Default — based on content size */
    flex-basis: 200px;          /* Start at 200px */
    flex-basis: 50%;            /* 50% of container */
    flex-basis: content;        /* Based on content (limited support) */
    flex-basis: 0;              /* Equal sizing — content distributes space */
}
```

`flex-basis: 0` makes all items share space equally from zero, ignoring content size:

```css
/* Three items divided equally regardless of content */
.item { flex: 1 1 0; }
```

### The Flex Shorthand

`flex` combines `flex-grow`, `flex-shrink`, and `flex-basis`:

```css
.item {
    flex: 1;                    /* flex-grow: 1, flex-shrink: 1, flex-basis: 0 */
    flex: auto;                 /* flex-grow: 1, flex-shrink: 1, flex-basis: auto */
    flex: none;                 /* flex-grow: 0, flex-shrink: 0, flex-basis: auto */
    flex: 0 0 200px;            /* Fixed 200px, does not grow or shrink */
    flex: 1 1 300px;            /* Grow and shrink from 300px base */
}
```

Common patterns:

```css
/* Equal-width columns */
.column { flex: 1; }

/* Fixed sidebar + fluid content */
.sidebar { flex: 0 0 250px; }
.content { flex: 1; }

/* Auto-sized content */
.nav { flex: 0 0 auto; }      /* Size to content, no grow/shrink */

/* Holy grail layout */
.header { flex: 0 0 auto; }
.main { flex: 1; }
.footer { flex: 0 0 auto; }
```

### Order

Reorder flex items visually without changing HTML:

```css
.item:first-child {
    order: 2;                   /* Appears second */
}

.item:nth-child(2) {
    order: 1;                   /* Appears first */
}

.item:last-child {
    order: 3;                   /* Appears last (default or explicitly) */
}
```

Default `order: 0`. Items with lower order appear first. Items with the same order maintain DOM order.

**Accessibility note:** `order` changes visual order but NOT tab order or screen reader order. Use only for cosmetic reordering. Do not use `order` to rearrange focusable elements.

### Flex Item Margins

Margins on flex items have special behavior:

```css
/* Auto margin on flex items uses remaining space */
.item {
    margin-left: auto;          /* Pushes this item to the right */
}

.container {
    display: flex;
}

/* First item pushed right, second stays */
.item:nth-child(2) { margin-left: auto; }
```

```css
/* Centering a single item */
.container {
    display: flex;
}
.item {
    margin: auto;               /* Centers on both axes */
}
```

### Nested Flexbox

Flex containers can be nested for complex layouts:

```css
.page {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
}

.header { flex: 0 0 auto; }

.main {
    flex: 1;
    display: flex;              /* Nested flex container */
    gap: 1rem;
}

.sidebar { flex: 0 0 250px; }
.content { flex: 1; }

.footer { flex: 0 0 auto; }
```

### Auto Margins in Flexbox

Auto margins consume available space in flex containers:

```css
.nav {
    display: flex;
    align-items: center;
    gap: 1.5rem;
}

.nav__link { }

.nav__cta {
    margin-left: auto;          /* Pushes CTA to the right */
}
```

Auto margins on the cross axis also work:

```css
.container {
    display: flex;
    align-items: flex-start;    /* Items at top */
    height: 300px;
}

.item {
    margin-top: auto;           /* Pushes item to bottom */
    margin-bottom: auto;        /* Pushes item to center */
}
```

### Min-Max Sizing and Flex

Flex items have implicit `min-width: auto` and `min-height: auto`, which can prevent shrinking below content size:

```css
.item {
    flex: 1;
    min-width: 0;               /* Allow shrinking below content size */
    overflow: hidden;           /* Same effect — creates new formatting context */
}
```

This is especially important for flex items containing long text:

```css
.flex-group {
    display: flex;
}

.flex-group-item {
    flex: 1;
    min-width: 0;               /* Prevents overflow from long words */
    overflow-wrap: break-word;
}
```

## Study Cases

### Holy Grail Layout

```css
body {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
    margin: 0;
}

header, footer {
    flex: 0 0 auto;
    padding: 1rem;
    background: #333;
    color: white;
}

.main-layout {
    display: flex;
    flex: 1;
    gap: 1rem;
    padding: 1rem;
}

.sidebar {
    flex: 0 0 250px;
    background: #f0f0f0;
    padding: 1rem;
}

.content {
    flex: 1;
}

@media (max-width: 768px) {
    .main-layout {
        flex-direction: column;
    }
    .sidebar {
        flex: 0 0 auto;
    }
}
```

### Centering Everything

The classic "centering in the unknown" — vertical and horizontal:

```css
.center {
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 300px;
}
```

One line each for horizontal and vertical centering. The content determines its own size.

### Card Grid

```css
.card-grid {
    display: flex;
    flex-wrap: wrap;
    gap: 1.5rem;
}

.card {
    flex: 1 1 300px;            /* Grow, shrink, target 300px */
    /* Auto-fills the row, wraps when needed */
}
```

This creates a responsive grid without media queries. Cards are at least 300px wide and grow to fill rows.

## Examples

### Example 1: Sticky footer

```css
body {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
}

main {
    flex: 1;
}

footer {
    flex: 0 0 auto;
}
```

### Example 2: Header with centered logo and nav links

```css
.header {
    display: flex;
    align-items: center;
    padding: 1rem 2rem;
}

.logo {
    margin-right: auto;         /* Push nav to the right */
}

.nav {
    display: flex;
    gap: 1.5rem;
}

.cta {
    margin-left: 2rem;
}
```

### Example 3: Media object (image + text)

```css
.media {
    display: flex;
    gap: 1rem;
    align-items: flex-start;
}

.media__image {
    flex: 0 0 auto;             /* Image does not grow or shrink */
    width: 100px;
    height: 100px;
    border-radius: 50%;
    object-fit: cover;
}

.media__body {
    flex: 1;                    /* Text fills remaining space */
}
```

## Glossary

| Term | Definition |
|---|---|
| Flex container | Element with `display: flex` or `inline-flex` |
| Flex item | Direct child of a flex container |
| Main axis | Primary direction defined by `flex-direction` |
| Cross axis | Perpendicular axis to main |
| flex-grow | Ability to expand into available space |
| flex-shrink | Ability to contract when space is tight |
| flex-basis | Initial size before space distribution |
| gap | Space between flex items |
| order | Visual reordering (cosmetic only) |

## Quick References

- [MDN: Flexbox](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_flexible_box_layout) — complete guide
- [CSS Flexbox Guide](https://css-tricks.com/snippets/css/a-guide-to-flexbox/) — visual reference
- [Flexbox Froggy](https://flexboxfroggy.com/) — interactive learning game
- [Flexbox Spec Level 1](https://www.w3.org/TR/css-flexbox-1/) — specification
- [Caniuse: Flexbox](https://caniuse.com/flexbox) — browser support

## Next Steps

- [CSS Grid](grid.md) — two-dimensional layout
- [Positioning](positioning.md) — absolute and relative positioning