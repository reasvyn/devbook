# Float & Clear

## Description

Float was originally designed for text wrapping around images but became the primary layout tool before Flexbox and Grid. Today it is used almost exclusively for wrapping text around floated elements.

## Prerequisites

- [Display Types & Normal Flow](display-types.md) — how floats affect flow
- [Block Formatting Context](display-types.md#block-formatting-context) — containing floats

## Table of Contents

- [How Float Works](#how-float-works)
- [Float Values](#float-values)
- [Float Behavior Details](#float-behavior-details)
- [Clear](#clear)
- [Clearfix](#clearfix)
- [Floats and Column Layout](#floats-and-column-layout)
- [Floats and Display](#floats-and-display)
- [Floats and Margin Collapse](#floats-and-margin-collapse)
- [Modern Alternatives](#modern-alternatives)
- [Float and Shape-Outside](#float-and-shape-outside)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### How Float Works

When an element is floated:
1. It is removed from normal flow
2. It shifts left or right as far as possible
3. Content below it wraps around the floated element
4. It stays within its containing block (cannot float outside)

```css
img {
    float: left;
    margin-right: 1rem;
    margin-bottom: 0.5rem;
}
```

```html
<p>
    <img src="photo.jpg" alt="">
    Text wraps around the floated image, filling the available
    space beside it. The image stays on the left and text
    flows around its right and bottom edges.
</p>
```

### Float Values

```css
.element {
    float: none;        /* Default — no float */
    float: left;        /* Float to the left side */
    float: right;       /* Float to the right side */
}
```

`float: left` and `float: right` are the only useful values. `inline-start` and `inline-end` exist for logical properties but have limited support.

### Float Behavior Details

**Shrink-wrap effect:** Floated elements shrink to fit their content:

```css
.float {
    float: left;
    /* width defaults to fit-content */
}

.float-with-width {
    float: left;
    width: 200px;       /* Explicit width */
}
```

**Float stacking:** Multiple floats stack against the container edge:

```css
.float {
    float: left;
    width: 150px;
    height: 100px;
}
/* If three .float elements, they sit side by side
   until they exceed the container width, then wrap */
```

**Float and vertical margins:** Vertical margins do not collapse between a float and other elements.

**Float and backgrounds:** Backgrounds of parent elements do not extend behind floated children (this is why clearfix is needed — the parent collapses to zero height).

### Clear

`clear` forces an element below floating elements:

```css
.element {
    clear: none;        /* Default — no clearance */
    clear: left;        /* Below any left floats */
    clear: right;       /* Below any right floats */
    clear: both;        /* Below both left and right floats */
}
```

```css
p {
    clear: both;        /* Paragraph starts below any floats above it */
}
```

The cleared element adds space above itself equal to the bottom margin of the tallest float it clears.

### Clearfix

The clearfix hack prevents a parent container from collapsing when all children are floated:

**Historic clearfix (pre-flexbox era):**

```css
.clearfix::after {
    content: '';
    display: table;
    clear: both;
}
```

This inserts a pseudo-element after all children that clears both floats, forcing the parent to encompass its floated children.

**Modern alternative:** Instead of a clearfix, create a Block Formatting Context:

```css
.container {
    display: flow-root;     /* Contains floats, no pseudo-element needed */
    /* Or: overflow: hidden; (but may clip content) */
}
```

### Floats and Column Layout

Before Flexbox and Grid, float-based layouts were standard:

```css
/* Two-column layout with floats */
.sidebar {
    float: left;
    width: 250px;
}

.main-content {
    float: left;
    width: calc(100% - 250px);
}

/* Three-column */
.column {
    float: left;
    width: 33.33%;
}

/* Clearfix on container */
.row::after {
    content: '';
    display: table;
    clear: both;
}
```

This is now obsolete for layout. Use Flexbox or Grid instead.

### Floats and Display

Setting `float` on an element changes its computed `display` value:

| Declared | Computed |
|---|---|
| `display: inline` | `display: block` |
| `display: inline-block` | `display: block` |
| `display: table-*` | `display: block` |
| `display: flex` | `display: flex` (unchanged) |
| `display: grid` | `display: grid` (unchanged) |

Inline elements become block-level when floated, which means they accept explicit width and height.

### Floats and Margin Collapse

Floated elements do NOT participate in margin collapse:

```css
.float {
    float: left;
    margin-bottom: 2rem;
}

.next-element {
    margin-top: 1rem;
    /* Margin does NOT collapse with the float's margin.
       Total space: 2rem + 1rem = 3rem */
}
```

This is important when combining floats with subsequent content — the space adds up.

### Modern Alternatives

| Float Use Case | Modern Replacement |
|---|---|
| Text wrapping around image | `float` (still the only option) |
| Multi-column layout | `display: grid` or `display: flex` |
| Horizontal navigation | `display: flex` |
| Equal-height columns | `display: flex` (auto) or `display: grid` |
| Wrapping elements | `flex-wrap: wrap` |
| Centering | `margin: 0 auto` or Flexbox |
| Containing children | `display: flow-root` |

The only scenario where `float` remains the best tool is text wrapping around images.

### Float and Shape-Outside

`shape-outside` changes the wrapping contour around a floated element (CSS Shapes Level 1):

```css
img {
    float: left;
    shape-outside: circle(50%);      /* Text wraps around a circle */
    clip-path: circle(50%);          /* Crop image to match */
}

.float {
    float: left;
    width: 200px;
    height: 200px;
    shape-outside: polygon(0 0, 100% 0, 80% 100%, 0 100%);
    /* Text wraps in a trapezoid shape */
}
```

Shape values:
- `circle(radius)` — circular wrap
- `ellipse(rx ry)` — elliptical wrap
- `polygon(x1 y1, x2 y2, ...)` — custom polygon
- `inset(top right bottom left round radius)` — inset rectangle
- `url(image.png)` — derive shape from image alpha channel
- `margin-box`, `border-box`, `padding-box`, `content-box` — use element's own box

`shape-margin` adds margin to the shape:

```css
img {
    float: left;
    shape-outside: circle(50%);
    shape-margin: 1rem;
}
```

## Glossary

| Term | Definition |
|---|---|
| Float | Move element left/right with text wrapping around it |
| Clear | Force an element below preceding floats |
| Clearfix | Technique preventing parent collapse when all children are floated |
| Shrink-wrap | Floats shrink to fit their content width |
| drop cap | Large initial letter that drops below the baseline |
| shape-outside | CSS property controlling text wrapping shape |
| Pull quote | Notable quotation floated within article text |
| Clear space | Area added above a cleared element |
| Float stack | Multiple floats grouped against container edge |
| Containing float | Preventing parent collapse from floated children |
| Initial letter | Large first character styled with float |
| BFC | Block Formatting Context (used to contain floats) |

## Quick References

- [MDN: float](https://developer.mozilla.org/en-US/docs/Web/CSS/float) — float reference
- [MDN: clear](https://developer.mozilla.org/en-US/docs/Web/CSS/clear) — clear reference
- [CSS Shapes Level 1](https://www.w3.org/TR/css-shapes-1/) — shape-outside specification
- [Logical Properties](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Logical_Properties) — modern float alternatives
- [CSS Shapes Generator](https://css-shape.com/) — interactive shape-outside tool
- [Float Layout Examples](https://developer.mozilla.org/en-US/docs/Web/CSS/Layout_cookbook/Media_objects) — media object pattern
- [W3C CSS Floats Primer](https://www.w3.org/TR/css-floats-3/) — specification overview
- [CSS Tricks: All About Floats](https://css-tricks.com/all-about-floats/) — comprehensive guide

## Next Steps

- [Media Queries](media-queries.md) — responsive design with breakpoints