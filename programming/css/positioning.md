# Positioning

## Description

CSS positioning controls how elements are placed within the document — whether they stay in normal flow, are removed from flow, or stick to the viewport during scrolling.

## Prerequisites

- [Display Types & Normal Flow](display-types.md) — normal flow behavior
- [The Box Model](box-model.md) — position affects box dimensions

## Table of Contents

- [Positioned Elements](#positioned-elements)
- [Static Positioning](#static-positioning)
- [Relative Positioning](#relative-positioning)
- [Absolute Positioning](#absolute-positioning)
- [Fixed Positioning](#fixed-positioning)
- [Sticky Positioning](#sticky-positioning)
- [The Containing Block](#the-containing-block)
- [Offset Properties](#offset-properties)
- [Z-Index and Stacking Context](#z-index-and-stacking-context)
- [Position and Overflow](#position-and-overflow)
- [Position and Transform](#position-and-transform)
- [Absolute Centering](#absolute-centering)
- [Fixed vs Sticky](#fixed-vs-sticky)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Positioned Elements

Elements with a `position` value other than `static` are "positioned elements":

```css
position: static;        /* Default — normal flow */
position: relative;      /* Offset from normal position */
position: absolute;      /* Removed from flow, positioned relative to containing block */
position: fixed;         /* Removed from flow, positioned relative to viewport */
position: sticky;        /* Toggle between relative and fixed based on scroll */
```

Each position value changes how offset properties (`top`, `right`, `bottom`, `left`) work.

### Static Positioning

Default — element follows normal flow:

```css
.element {
    position: static;
    /* top, right, bottom, left have NO effect */
}
```

Offset properties are ignored for static elements.

### Relative Positioning

Element stays in normal flow but is offset from its natural position:

```css
.element {
    position: relative;
    top: 10px;
    left: 20px;
    /* Offset 10px down, 20px right from where it would normally be */
}
```

The element's original space is preserved — other elements do not move to fill the gap. The offset is visual only.

Use cases for relative:
- Creating a containing block for absolutely positioned children
- Fine-tuning element position without affecting layout
- Creating stacking contexts

```css
.container {
    position: relative;      /* Absolutely positioned children reference this */
}

.overlay {
    position: absolute;
    top: 0;
    left: 0;
}
```

### Absolute Positioning

Element is removed from normal flow and positioned relative to its containing block:

```css
.element {
    position: absolute;
    top: 20px;
    right: 20px;
}
```

Behavior:
- Removed from document flow (no space reserved)
- Positioned relative to the nearest positioned ancestor (non-static)
- If no positioned ancestor, uses the initial containing block (`<html>`)
- Shrinks to fit content (`width: auto` behaves like `fit-content`)
- Absolutely positioned elements establish their own stacking context

```css
.card {
    position: relative;          /* Containing block */
}

.badge {
    position: absolute;
    top: -8px;
    right: -8px;
    /* Floats outside the card's top-right corner */
}
```

### Fixed Positioning

Element is removed from flow and positioned relative to the viewport:

```css
.element {
    position: fixed;
    top: 0;
    width: 100%;
    /* Stays at the top of the viewport during scroll */
}
```

Behavior:
- Removed from document flow
- Positioned relative to the viewport (or the closest `transform` ancestor)
- Does not scroll with the page
- Creates a stacking context

Common uses: sticky headers, floating buttons, modals, cookie banners.

**Important:** A `transform` on any ancestor changes the containing block for `fixed` elements. This means `position: fixed` inside a transformed element does not behave as expected.

### Sticky Positioning

Hybrid between relative and fixed — toggles based on scroll position:

```css
.element {
    position: sticky;
    top: 0;
    /* Behaves as relative until scroll passes its position,
       then "sticks" 0px from the viewport top */
}
```

Requirements:
- Must specify at least one offset (`top`, `right`, `bottom`, `left`)
- The element's parent must be in the scroll container
- The parent should not have `overflow: hidden` (it clips sticky behavior)

```css
.nav {
    position: sticky;
    top: 0;
    z-index: 100;
    background: white;
}
```

Multiple sticky elements in the same container stack naturally if given different offset values:

```css
.section-header {
    position: sticky;
    top: 0;              /* First header sticks at top */
}

.subsection-header {
    position: sticky;
    top: 2rem;           /* Second header sticks below the first */
}
```

### The Containing Block

The containing block is the reference rectangle used for positioning and percentage sizing:

| Position | Containing Block |
|---|---|
| `static`, `relative`, `sticky` | Nearest block-level ancestor's content box |
| `absolute` | Nearest positioned ancestor's padding box |
| `fixed` | Viewport (or nearest `transform` ancestor) |

```css
/* For absolute positioning, the containing block is the padding box */
.parent {
    position: relative;
    padding: 20px;
}

.child {
    position: absolute;
    top: 0;
    left: 0;
    /* top-left of parent's padding edge, not content edge */
}
```

### Offset Properties

`top`, `right`, `bottom`, `left` move positioned elements:

```css
.element {
    position: absolute;
    top: 20px;           /* 20px from containing block's top edge */
    right: 10px;         /* 10px from containing block's right edge */
    bottom: auto;        /* Default — not set */
    left: auto;          /* Default */
}
```

When opposite offsets are both set on an absolutely positioned element, it stretches:

```css
.element {
    position: absolute;
    top: 20px;
    bottom: 20px;
    left: 20px;
    right: 20px;
    /* Fills the containing block with 20px margins on all sides */
    /* width and height are stretched, not content-determined */
}
```

This stretching is how full-screen overlays and cover elements work:

```css
.overlay {
    position: fixed;
    top: 0;
    right: 0;
    bottom: 0;
    left: 0;
    background: rgba(0, 0, 0, 0.5);
    /* Covers the entire viewport */
}
```

### Z-Index and Stacking Context

`z-index` controls stacking order along the Z-axis:

```css
.element {
    position: absolute;
    z-index: 10;             /* Higher value = on top */
}
```

`z-index` only works on positioned elements (not `static`):

```css
.element {
    position: relative;
    z-index: 1;
}

.overlay {
    position: fixed;
    z-index: 100;
}
```

**Stacking context** is a group of elements stacked together. A new context is created by:

```css
/* Creates stacking context */
.element {
    position: relative;
    z-index: 1;
    /* Or any non-auto z-index on positioned element */
}

.element {
    opacity: 0.9;            /* Creates stacking context */
}

.element {
    transform: scale(1);     /* Creates stacking context */
}

.element {
    filter: blur(0);         /* Creates stacking context */
}

.element {
    isolation: isolate;       /* Creates stacking context — cleanest */
}
```

`isolation: isolate` creates a new stacking context without side effects — it is the best way to contain Z-index issues in components.

### Position and Overflow

Positioned elements interact with overflow:

```css
.parent {
    position: relative;
    width: 200px;
    height: 200px;
    overflow: hidden;
}

.child {
    position: absolute;
    top: 150px;                /* Partially outside parent */
    /* Clipped by parent's overflow: hidden */
}
```

`position: fixed` elements cannot be clipped by `overflow: hidden` on ancestors (unless a `transform` changes the containing block).

### Position and Transform

Setting `transform` on an element makes it the containing block for its positioned descendants:

```css
.parent {
    transform: translate(0);       /* Now a containing block */
}

.child {
    position: fixed;
    top: 0;
    /* Fixed to parent's bounding box, not viewport */
}
```

This is a common source of bugs — if a modal's backdrop sets `transform: translateZ(0)` for GPU acceleration, any `position: fixed` children inside it will not be viewport-fixed.

### Absolute Centering

Perfect centering with absolute positioning:

```css
.center-me {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
    /* Center of containing block */
}
```

The `top: 50%` moves the element's top-left corner to the center. `transform: translate(-50%, -50%)` moves it back by half its own dimensions.

Alternatively, with a known size:

```css
.center-me {
    position: absolute;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    margin: auto;
    width: 200px;
    height: 100px;
}
```

### Fixed vs Sticky

| Behavior | Fixed | Sticky |
|---|---|---|
| Normal flow | Removed | Preserved (until threshold) |
| Positioning reference | Viewport (or transform ancestor) | Scroll container |
| Scrolling | Does not scroll | Scrolls until threshold, then sticks |
| Parent overflow: hidden | Not clipped (except with transform) | Clipped |
| Space in flow | No | Yes (until stick threshold) |

Use `fixed` for: modals, floating buttons, persistent headers that should always be visible.

Use `sticky` for: section headers in long lists, table headers, navigation that scrolls away with content.

## Glossary

| Term | Definition |
|---|---|
| Positioned element | Any element with `position` not `static` |
| Containing block | Reference rectangle for positioning and percentage sizing |
| Normal flow | Default document layout |
| Offset | `top`, `right`, `bottom`, `left` values |
| Stacking context | Group of elements sharing a Z-axis order |
| z-index | Z-axis position within a stacking context |
| Absolute centering | Centering via `position: absolute` + `transform` |

## Quick References

- [MDN: position](https://developer.mozilla.org/en-US/docs/Web/CSS/position) — complete reference
- [MDN: Stacking Context](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_positioned_layout/Understanding_z-index/Stacking_context) — stacking context guide
- [CSS Positioned Layout Level 3](https://www.w3.org/TR/css-position-3/) — specification
- [Caniuse: sticky](https://caniuse.com/css-sticky) — sticky support
- [Caniuse: position: fixed](https://caniuse.com/css-fixed) — fixed support

## Next Steps

- [Multi-Column Layout](multi-column.md) — newspaper-style columns