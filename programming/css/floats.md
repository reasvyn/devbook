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
- [Historical Context](#historical-context)
- [Clearfix Deep Dive](#clearfix-deep-dive)
- [Migration Guide: Floats to Flexbox and Grid](#migration-guide-floats-to-flexbox-and-grid)
- [Float in Responsive Design](#float-in-responsive-design)
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

**Shape-Outside Real-World Example: Magazine Article**

```css
.article {
    max-width: 800px;
    margin: 0 auto;
    font-size: 1.1rem;
    line-height: 1.8;
}

.article .portrait {
    float: left;
    width: 250px;
    height: 300px;
    margin-right: 1.5rem;
    margin-bottom: 0.5rem;
    shape-outside: inset(0 0 0 0 round 10px);
    clip-path: inset(0 0 0 0 round 10px);
    transition: shape-outside 0.3s ease;
}

/* Create a circular portrait */
.article .portrait-round {
    float: left;
    width: 200px;
    height: 200px;
    shape-outside: circle(50%);
    clip-path: circle(50%);
    margin-right: 1.5rem;
}

/* Wrap text around an L-shape for an infographic */
.article .info-box {
    float: right;
    width: 250px;
    height: 350px;
    shape-outside: polygon(
        0 0, 100% 0, 100% 75%,
        60% 75%, 60% 100%, 0 100%
    );
    background: #f0f4ff;
    padding: 1rem;
    border-radius: 8px 8px 0 0;
    margin-left: 1.5rem;
    margin-bottom: 1rem;
}

/* Float with drop shadow text wrap */
.article .highlight-box {
    float: left;
    width: 180px;
    height: 180px;
    shape-outside: margin-box;
    margin-right: 1.5rem;
    margin-bottom: 0.5rem;
    box-shadow: 0 4px 12px rgba(0,0,0,0.1);
}
```

The `margin-box` value is particularly useful — it makes text wrap around the element's margin box rather than its border box. Combined with `box-shadow`, this creates clean wrapping even when visual effects extend beyond the element bounds.

### Historical Context

Floats originated in print design, where text would flow around images. CSS adopted the concept from desktop publishing (QuarkXPress, PageMaker) and they became the unexpected foundation of web layout for nearly 15 years.

**The Float-Based Layout Era (2001-2015)**

Before Flexbox and Grid, floats were the only viable method for creating multi-column layouts:

```css
/* 2000s: Float-based page layout */
#header { ... }
#nav { float: left; width: 200px; }
#content { float: left; width: 580px; }
#sidebar { float: right; width: 200px; }
#footer { clear: both; }
```

**CSS Frameworks (2011-2019)**

Frameworks like Bootstrap 3 and Foundation built grid systems entirely on floats:

```css
/* Bootstrap 3-style grid */
.row::after {
    content: '';
    display: table;
    clear: both;
}

.col-md-4 {
    float: left;
    width: 33.3333%;
    padding: 0 15px;
}

.col-md-6 {
    float: left;
    width: 50%;
    padding: 0 15px;
}
```

These frameworks required extensive clearfix hacks and had limitations:
- No equal-height columns without JavaScript or table-cell hacks
- Limited vertical alignment options
- Complex clearing patterns for nested grids
- Source order mattered for layout (no reordering without hacks)

**The 960.gs Grid System (2009)**

```css
/* 960.gs 12-column grid */
.container_12 { margin: 0 auto; width: 960px; }
.grid_4 { float: left; width: 300px; margin: 0 10px; }
.grid_8 { float: left; width: 620px; margin: 0 10px; }
```

**The Float Declines (2015-2020)**

With Flexbox gaining full browser support and Grid following in 2017, float-based layouts became obsolete. The CSS Working Group officially recommends against using floats for layout. Modern CSS provides better tools for every layout use case floats once solved.

### Clearfix Deep Dive

The clearfix evolved through several generations as browser support and CSS capabilities improved:

**Generation 1: The Empty Div (2000-2006)**

```html
<div class="container">
    <div style="float: left;">...</div>
    <div style="float: left;">...</div>
    <div style="clear: both;"></div>
</div>
```

Problem: requires non-semantic empty elements in the markup.

**Generation 2: The Overflow Hack (2006-2011)**

```css
.container {
    overflow: hidden;   /* Creates BFC, but clips shadows/dropdowns */
    /* or */
    overflow: auto;     /* Creates BFC, may show scrollbars */
}
```

Problem: `overflow: hidden` clips absolutely positioned children and shadows; `overflow: auto` may introduce unwanted scrollbars.

**Generation 3: The Micro Clearfix (2011-2015)**

Developed by Nicolas Gallagher, this was the most widely used clearfix:

```css
/**
 * Micro Clearfix
 * http://nicolasgallagher.com/micro-clearfix-hack/
 */
.clearfix::before,
.clearfix::after {
    content: '';
    display: table;
}

.clearfix::after {
    clear: both;
}

/* IE 6/7 fallback */
.clearfix {
    *zoom: 1;
}
```

The `::before` pseudo-element prevents top-margin collapse. The `::after` pseudo-element clears both floats. `display: table` creates anonymous table cells that generate block formatting contexts.

**Generation 4: flow-root (2018-present)**

```css
.container {
    display: flow-root;   /* Creates a new BFC without side effects */
}
```

`display: flow-root` is the cleanest solution. It creates a new Block Formatting Context without clipping content, adding scrollbars, or requiring pseudo-elements. It is supported in all modern browsers (Chrome 58+, Firefox 53+, Safari 10.1+, Edge 79+).

**Clearfix Comparison**

| Method | Pros | Cons |
|---|---|---|
| Empty div | Simple, widely understood | Non-semantic markup |
| `overflow: hidden` | One property, creates BFC | Clips shadows and absolute children |
| `overflow: auto` | Creates BFC | May show unwanted scrollbars |
| Micro clearfix | No markup changes, works everywhere | Requires pseudo-elements, verbose |
| `display: flow-root` | Clean, no side effects | Requires modern browser |

```css
/* Modern approach: only clearfix when you must support legacy browsers */
.container {
    display: flow-root;  /* Contains all floats */
}

/* Fallback for older browsers */
@supports not (display: flow-root) {
    .container::after {
        content: '';
        display: table;
        clear: both;
    }
}
```

### Migration Guide: Floats to Flexbox and Grid

**Pattern 1: Two-Column Layout**

```css
/* Float approach */
.sidebar { float: left; width: 250px; }
.content { float: left; width: calc(100% - 250px); }
.container::after { content: ''; display: table; clear: both; }

/* Flexbox replacement */
.container { display: flex; }
.sidebar { flex: 0 0 250px; }
.content { flex: 1; }
```

**Pattern 2: Three-Column Equal-Width**

```css
/* Float approach */
.col { float: left; width: 33.33%; }
.row::after { content: ''; display: table; clear: both; }

/* Grid replacement */
.row { display: grid; grid-template-columns: 1fr 1fr 1fr; gap: 1rem; }
```

**Pattern 3: Horizontal Navigation**

```css
/* Float approach */
.nav li { float: left; }
.nav::after { content: ''; display: table; clear: both; }

/* Flexbox replacement */
.nav { display: flex; }
/* or */
.nav { display: flex; gap: 1rem; justify-content: space-around; }
```

**Pattern 4: Media Object (Image + Text)**

```css
/* Float approach */
.media img { float: left; margin-right: 1rem; }
.media-body { overflow: hidden; }

/* Flexbox replacement */
.media { display: flex; gap: 1rem; align-items: flex-start; }
.media img { flex-shrink: 0; }
```

**Pattern 5: Holy Grail Layout (Header + Nav + Content + Sidebar + Footer)**

```css
/* Float approach */
#header, #footer { width: 100%; clear: both; }
#nav { float: left; width: 200px; }
#content { float: left; width: calc(100% - 440px); }
#sidebar { float: right; width: 200px; }
#footer { clear: both; }

/* Grid replacement */
body {
    display: grid;
    grid-template-areas:
        "header header header"
        "nav    content sidebar"
        "footer footer footer";
    grid-template-columns: 200px 1fr 200px;
    grid-template-rows: auto 1fr auto;
    min-height: 100vh;
}
#header { grid-area: header; }
#nav { grid-area: nav; }
#content { grid-area: content; }
#sidebar { grid-area: sidebar; }
#footer { grid-area: footer; }
```

**Pattern 6: Drop Cap**

Drop caps remain a valid float use case in modern CSS:

```css
.dropcap::first-letter {
    float: left;
    font-size: 3em;
    line-height: 0.8;
    padding-right: 0.15em;
    padding-top: 0.05em;
    font-weight: bold;
    color: #8b0000;
}
```

### Float in Responsive Design

Floats behave differently across screen sizes and require careful responsive handling:

**Floats and Small Screens**

```css
/* Float works fine for text wrapping on all screens */
img.portrait {
    float: left;
    max-width: 50%;      /* Ensure image scales down */
    margin-right: 1rem;
    margin-bottom: 0.5rem;
}

/* On very small screens, remove the float for readability */
@media (max-width: 480px) {
    img.portrait {
        float: none;
        display: block;
        max-width: 100%;
        margin: 0 auto 1rem;
    }
}
```

**Float Clearing in Responsive Layouts**

```css
/* Float-based grid that stacks on mobile */
.row { display: flow-root; }
.col { float: left; width: 50%; }

@media (max-width: 600px) {
    .col {
        float: none;
        width: 100%;
    }
}
```

**Shape-Outside and Responsive Design**

```css
.shape-element {
    float: left;
    width: 200px;
    height: 200px;
    shape-outside: circle(50%);
    shape-margin: 0.5rem;
}

@media (max-width: 768px) {
    .shape-element {
        width: 120px;
        height: 120px;
        shape-margin: 0.25rem;
    }
}

@media (max-width: 480px) {
    .shape-element {
        float: none;
        width: 100%;
        height: auto;
        aspect-ratio: 1;
        margin-bottom: 1rem;
    }
}
```

**Common Responsive Float Patterns**

| Scenario | Approach |
|---|---|
| Image in article text | `float: left; max-width: 50%;` — remove float on mobile |
| Pull quote | `float: right; width: 40%; margin: 1rem 0 1rem 1.5rem;` — full width on mobile |
| Side-by-side stats | `float: left; width: 50%;` — full width on mobile via media query |
| Avatar + bio | Float avatar, overflow on text — use flexbox instead for modern sites |
| Form layouts | `float: left` on labels — use grid or flexbox instead |

## Learning Tips

- **Only use float for text wrapping.** Modern CSS (Flexbox, Grid) handles all layout cases. Reserve `float` for the one thing it does best: wrapping text around images and other inline elements.
- **Use `display: flow-root` for clearfix.** It is the cleanest, most predictable way to contain floats. Avoid `overflow: hidden` unless you understand its clipping behavior.
- **Combine `shape-outside` with `clip-path`.** `shape-outside` controls text wrapping; `clip-path` visually crops the element. Using both creates a consistent appearance.
- **Test floats on small screens.** Floated elements with fixed widths can overflow on mobile. Always use `max-width: 100%` and consider removing floats on small viewports.
- **Avoid float for multi-column layouts.** Even if you need to support older browsers, graceful degradation (single column) is often acceptable. Do not build new layouts with floats.
- **Debug with DevTools.** Chrome DevTools highlights floated elements and shows their shape-outside contours. Use the Layout panel to inspect float behavior.
- **Remember margin collapse rules.** Floats do not participate in margin collapse. The space between a float and subsequent elements is the sum of the margins.

## Glossary

| Term | Definition |
|---|---|
| Float | CSS property that moves an element left or right with text wrapping around it |
| Clear | Property that forces an element below preceding floating elements |
| Clearfix | Technique preventing parent container collapse when all children are floated |
| Shrink-wrap | Behavior where floated elements shrink to fit their content width |
| Drop cap | Large initial letter that drops below the baseline, created with `float: left` |
| shape-outside | CSS property that defines the wrapping contour around a floated element |
| Pull quote | Notable quotation floated within article text for visual emphasis |
| Clear space | Vertical area added above a cleared element to push it below floats |
| Float stack | Multiple floated elements grouped against the container edge |
| Containing float | Preventing a parent from collapsing due to floated children |
| Initial letter | Large first character styled with float (similar to drop cap) |
| BFC | Block Formatting Context — a rendering region that contains floats |
| flow-root | `display: flow-root` — modern way to create a BFC without side effects |
| Micro clearfix | Clearfix technique by Nicolas Gallagher using `::after` pseudo-element |
| shape-margin | Margin around the shape defined by `shape-outside` |
| shape-image-threshold | Alpha channel threshold for shapes derived from images |
| clip-path | CSS property that crops an element to a defined shape (used with shape-outside) |
| margin-box | Reference box for shape-outside that includes element margins |
| Float-based layout | Historical layout technique (2001-2015) using floats for column structures |
| 960.gs | Early CSS grid framework built entirely on floats |
| Bootstrap grid | Responsive float-based grid system popular in Bootstrap 3 |

## Quick References

- [MDN: float](https://developer.mozilla.org/en-US/docs/Web/CSS/float) — float reference
- [MDN: clear](https://developer.mozilla.org/en-US/docs/Web/CSS/clear) — clear reference
- [CSS Shapes Level 1](https://www.w3.org/TR/css-shapes-1/) — shape-outside specification
- [Logical Properties](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Logical_Properties) — modern float alternatives
- [CSS Shapes Generator](https://css-shape.com/) — interactive shape-outside tool
- [Float Layout Examples](https://developer.mozilla.org/en-US/docs/Web/CSS/Layout_cookbook/Media_objects) — media object pattern
- [W3C CSS Floats Primer](https://www.w3.org/TR/css-floats-3/) — specification overview
- [CSS Tricks: All About Floats](https://css-tricks.com/all-about-floats/) — comprehensive guide
- [Nicolas Gallagher: Micro Clearfix](https://nicolasgallagher.com/micro-clearfix-hack/) — original micro clearfix article
- [MDN: display: flow-root](https://developer.mozilla.org/en-US/docs/Web/CSS/display#flow-root) — flow-root reference
- [Bootstrap 3 Grid System](https://getbootstrap.com/docs/3.4/css/#grid) — historic float-based grid
- [Smashing Magazine: Float Layout History](https://www.smashingmagazine.com/2019/07/history-of-css-grid-layouts/) — evolution of web layout
- [960 Grid System](https://960.gs/) — classic grid framework
- [CSS Workshop: Float and Clear](https://css-workshop.com/examples/float-and-clear/) — interactive float examples

## Next Steps

- [Media Queries](media-queries.md) — responsive design with breakpoints