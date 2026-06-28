# The Box Model

## Description

Every element in CSS is a rectangular box. The box model defines how width, height, padding, border, and margin interact to determine the element's rendered size.

## Prerequisites

- [Cascade, Specificity & Inheritance](cascade-and-specificity.md) — basic CSS property application
- [HTML Elements & Attributes](../html/elements-and-attributes.md) — block vs. inline elements

## Table of Contents

- [The Four Zones](#the-four-zones)
- [Content Box](#content-box)
- [Padding](#padding)
- [Border](#border)
- [Margin](#margin)
- [Box Sizing](#box-sizing)
- [The Box Model in Practice](#the-box-model-in-practice)
- [Collapsing Margins](#collapsing-margins)
- [Negative Margins](#negative-margins)
- [Display Types and the Box Model](#display-types-and-the-box-model)
- [The Outline Property](#the-outline-property)
- [Overflow and the Box Model](#overflow-and-the-box-model)
- [Box Shadows](#box-shadows)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Four Zones

Every element generates a box with four concentric layers, from innermost to outermost:

```
┌───────────────────────────────────────┐  ← Margin edge
│  ┌─────────────────────────────────┐  │  ← Border edge
│  │  ┌───────────────────────────┐  │  │  ← Padding edge
│  │  │  ╔══════════════════════╗  │  │  │  ← Content edge
│  │  │  ║   Content area       ║  │  │  │
│  │  │  ║   (width × height)   ║  │  │  │
│  │  │  ╚══════════════════════╝  │  │  │
│  │  └───────────────────────────┘  │  │  ← Padding
│  └─────────────────────────────────┘  │  ← Border
└───────────────────────────────────────┘  ← Margin
```

- **Content** — where text and child elements appear
- **Padding** — space between content and border (inside the element)
- **Border** — edge line around the padding
- **Margin** — space outside the border (between this element and adjacent ones)

### Content Box

The content box holds the element's actual content (text, images, children). Its size is controlled by:

```css
.element {
    width: 300px;
    height: 200px;
    min-width: 200px;
    max-width: 500px;
    min-height: 100px;
    max-height: 400px;
}
```

Content size behavior:
- **Block elements** default width: 100% of parent (fill available space). Height: auto (wraps content).
- **Inline elements** ignore explicit width/height — sized by content.
- **Replaced elements** (`<img>`, `<video>`, `<canvas>`) have intrinsic dimensions from the media.

### Padding

Padding adds space inside the element, between content and border:

```css
/* All sides */
.element { padding: 20px; }

/* Individual sides */
.element {
    padding-top: 10px;
    padding-right: 20px;
    padding-bottom: 10px;
    padding-left: 20px;
}

/* Shorthand — top, right, bottom, left (clockwise) */
.element { padding: 10px 20px 10px 20px; }

/* Two values: top/bottom, left/right */
.element { padding: 10px 20px; }

/* Three values: top, left/right, bottom */
.element { padding: 10px 20px 15px; }
```

Padding is always inside the element. It has background-color and background-image — padding inherits the element's background.

Padding does not collapse (unlike margins). Two adjacent elements both keep their padding.

### Border

Border sits between padding and margin. It has width, style, and color:

```css
/* Width, style, color */
.element {
    border: 2px solid #333;
}

/* Individual sides */
.element {
    border-top: 1px dashed #ccc;
    border-right: 2px solid #666;
    border-bottom: 3px double #999;
    border-left: 4px groove #333;
}

/* Separate properties */
.element {
    border-width: 2px;
    border-style: solid;
    border-color: #333;
}

/* Border radius — corners */
.element {
    border-radius: 8px;          /* All corners */
    border-radius: 8px 4px;      /* top-left & bottom-right, top-right & bottom-left */
    border-radius: 8px 4px 2px 1px;  /* Clockwise from top-left */
    border-radius: 50%;           /* Circle (with equal width/height) */
}
```

Border styles available:

```css
.element {
    border-style: solid;    /* Solid line */
    border-style: dashed;   /* Dashed line */
    border-style: dotted;   /* Dotted line */
    border-style: double;   /* Double line */
    border-style: groove;   /* 3D groove (inset) */
    border-style: ridge;    /* 3D ridge (opposite of groove) */
    border-style: inset;    /* 3D inset */
    border-style: outset;   /* 3D outset */
    border-style: none;     /* No border (default) */
    border-style: hidden;   /* Same as none, but in table conflict resolution */
}
```

Border does not have background — the element's background shows behind borders, but transparent borders reveal the parent's background.

**Border vs. outline:**

```css
.element {
    outline: 2px solid blue;       /* Like border but outside the border edge */
    outline-offset: 2px;            /* Can be negative! */
    outline-style: auto;            /* Browser-native focus indicator */
}
```

Outlines do not take space — they draw over adjacent content. They follow the border shape but are not part of the box model. Use for focus indicators and debugging.

### Margin

Margin adds space outside the border. It pushes adjacent elements away:

```css
.element { margin: 20px; }

.element {
    margin-top: 10px;
    margin-right: 20px;
    margin-bottom: 10px;
    margin-left: 20px;
}

/* Shorthand (same as padding) */
.element { margin: 10px 20px; }
.element { margin: 10px 20px 15px; }
.element { margin: 10px 20px 15px 25px; }
```

**Auto margins:**

```css
/* Center a block element horizontally */
.element {
    width: 300px;       /* Must have explicit width */
    margin-left: auto;
    margin-right: auto;
}

/* Or shorthand */
.element {
    width: 300px;
    margin: 0 auto;
}
```

Auto margins fill the remaining space. Equal left/right auto margins center the element. This does not work on inline elements or replaced elements whose width is max-content.

Margin is always transparent — it shows the parent's background.

### Box Sizing

The `box-sizing` property changes how width/height are calculated:

```css
/* content-box (default) */
.element {
    box-sizing: content-box;
    width: 300px;
    padding: 20px;
    border: 2px solid;
    /* Total width = 300 + 20*2 + 2*2 = 344px */
}

/* border-box (intuitive) */
.element {
    box-sizing: border-box;
    width: 300px;
    padding: 20px;
    border: 2px solid;
    /* Total width = 300px (content area = 300 - 40 - 4 = 256px) */
}
```

The universal fix:

```css
*, *::before, *::after {
    box-sizing: border-box;
}
```

With `border-box`, setting `width: 100%` means "fill 100% of parent, including your padding and border." This prevents unexpected overflow from padding and border added to explicit widths.

Without this fix, `width: 50%; padding: 20px` would make elements wider than 50%.

### The Box Model in Practice

**Block-level box model:**
- Takes full available width by default
- Height wraps content
- Margins can collapse
- Respects all box properties

**Inline-level box model:**
- Ignores explicit width/height
- Padding and border applied but do not affect flow (can overlap other lines)
- Margins only work horizontally (left/right)
- Line height determines vertical space

```html
<span style="padding: 20px; margin: 20px;">
    <!-- Horizontal padding/margin push neighbors horizontally -->
    <!-- Vertical padding does not push lines above/below — it may overlap -->
</span>
```

**Inline-block box model:**

```css
.element {
    display: inline-block;   /* Block internally, inline externally */
    width: 200px;            /* Works! Unlike inline */
    height: 100px;           /* Works! */
    padding: 10px;           /* Works vertically without overlap */
    margin: 10px;            /* Works in all directions */
}
```

`inline-block` is the best of both worlds: the element flows inline with text, but its internal box respects all box model properties.

### Collapsing Margins

Adjacent vertical margins collapse — the larger margin wins:

```css
/* Two adjacent <p> elements */
p { margin: 20px 0; }

/* Vertical distance between them: 20px, NOT 40px */
/* The margins collapse to the larger value */
```

Collapsing rules:
- Only vertical margins collapse (top and bottom)
- Only in block layout (Flexbox and Grid do NOT collapse margins)
- Adjacent siblings, parent and first/last child, and empty blocks all collapse
- Negative margins collapse differently: positive + negative = sum, negative + negative = most negative

```css
/* Complex collapse */
.element { margin-top: 30px; }
.next { margin-top: 20px; }
/* Collapsed result: 30px (larger positive) */

.element { margin-top: -10px; }
.next { margin-top: -20px; }
/* Collapsed result: -20px (larger absolute negative) */

.element { margin-top: 30px; }
.next { margin-top: -10px; }
/* Collapsed result: 20px (sum: 30 + (-10)) */
```

**Preventing margin collapse:**
- Add `padding: 1px` to parent
- Add `border: 1px solid transparent` to parent
- Use `overflow: hidden` on parent
- Use Flexbox or Grid layout (margins do not collapse)
- Use `display: flow-root` on parent

### Negative Margins

Negative margins pull elements in the opposite direction:

```css
.element {
    margin-top: -20px;     /* Pulls element up */
    margin-left: -20px;    /* Pulls element left / expands width */
    margin-right: -20px;   /* Pulls right neighbor left / expands width */
    margin-bottom: -20px;  /* Pulls bottom neighbor up */
}
```

Use cases:
- Pulling elements outside their parent (e.g., full-width sections within a container)
- Overlapping elements deliberately
- Counteracting padding of a parent
- Creating hanging elements (pull left margin of first list item)

Negative margins can cause overflow — the element extends beyond its parent's bounds.

### Display Types and the Box Model

Different display values change box model behavior:

```css
/* Block — full box model */
display: block;

/* Inline — partial box model (no width/height, partial margin) */
display: inline;

/* Inline-block — full box model, inline flow */
display: inline-block;

/* Flex — block-level flex container */
display: flex;

/* Inline-flex — inline-level flex container */
display: inline-flex;

/* Grid — block-level grid container */
display: grid;

/* Inline-grid — inline-level grid container */
display: inline-grid;

/* Flow-root — establishes new block formatting context */
display: flow-root;

/* Table — behaves like <table> */
display: table;

/* List-item — behaves like <li> */
display: list-item;

/* Contents — box disappears, children inherit from next parent up */
display: contents;
```

The `display: contents` value is special — the element's box is not rendered, but its children are. This is useful for grid/flex sub-items where the parent element is only a semantic wrapper.

### The Outline Property

Outlines are like borders but do not affect layout:

```css
.element:focus-visible {
    outline: 2px solid #0066cc;
    outline-offset: 2px;        /* Space between border edge and outline */
}
```

Differences from border:
- Does not take space (draws over content)
- Can be offset inward (negative `outline-offset`)
- Always fully closed (not side-specific)
- Not part of the box model
- `outline: none` without providing a replacement breaks keyboard accessibility

### Overflow and the Box Model

When content exceeds the content box, overflow behavior applies:

```css
.element {
    width: 200px;
    height: 100px;
    overflow: visible;       /* Content overflows (default) */
    overflow: hidden;        /* Content clipped */
    overflow: scroll;        /* Always show scrollbars */
    overflow: auto;          /* Show scrollbars only when needed */
    overflow: clip;          /* Like hidden, but no programmatic scrolling */
}
```

Overflow properties:

```css
/* Separate axes */
.element {
    overflow-x: hidden;
    overflow-y: auto;
}

/* Text overflow */
.element {
    white-space: nowrap;            /* Prevent wrapping */
    overflow: hidden;
    text-overflow: ellipsis;        /* Show ... for overflowed text */
}
```

Overflow creates a new Block Formatting Context, which affects margin collapse behavior.

### Box Shadows

Box shadows are not part of the box model but visually extend beyond the border edge:

```css
.element {
    box-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
    /* offset-x, offset-y, blur-radius, color */
}

.element {
    box-shadow: 2px 2px 4px 2px rgba(0, 0, 0, 0.2);
    /* offset-x, offset-y, blur-radius, spread-radius, color */
}

/* Multiple shadows */
.element {
    box-shadow:
        0 2px 4px rgba(0, 0, 0, 0.1),
        0 4px 8px rgba(0, 0, 0, 0.05);
}

/* Inset shadow */
.element {
    box-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.2);
}
```

Spread radius (`spread-radius`) expands the shadow in all directions. Inset shadows appear inside the element.

## Study Cases

### Card Component Using Border-Box

```css
*, *::before, *::after {
    box-sizing: border-box;
}

.card {
    width: 100%;
    max-width: 320px;
    padding: 1.5rem;
    border: 1px solid #e0e0e0;
    border-radius: 12px;
    margin: 1rem;
}

.card__image {
    width: 100%;        /* Fills card width including padding */
    border-radius: 8px;
}
```

Without `border-box`, the card would be 320px + 2*1.5rem + 2*1px wide. With `border-box`, it is exactly 320px.

### Preventing Layout Shift with Explicit Sizing

```css
.card {
    width: 100%;
    max-width: 400px;
}

.card__image {
    width: 100%;
    aspect-ratio: 16 / 9;       /* Reserve space before image loads */
    object-fit: cover;
}
```

`aspect-ratio` combined with `width: 100%` ensures the image area has a known height before the image loads, preventing cumulative layout shift.

### Debugging Margin Collapse

```html
<div class="parent">
    <div class="child">Content</div>
</div>
```

```css
.parent {
    background: #f0f0f0;
    /* Problem: no border/padding/overflow — margin of .child collapses through */
}

.child {
    margin-top: 40px;    /* Collapses through .parent — parent does not grow */
}
```

Fix:

```css
.parent {
    display: flow-root;  /* Creates new BFC — prevents margin collapse */
    background: #f0f0f0;
}
```

## Examples

### Example 1: Centering with auto margins

```css
.centered {
    width: 300px;
    margin-left: auto;
    margin-right: auto;
}

/* In a flex container, auto margin on the first child pushes it right */
.flex-container {
    display: flex;
}

.push-right {
    margin-left: auto;
}
```

### Example 2: Box-sizing comparison

```css
/* content-box — total: 360px */
.content-box {
    box-sizing: content-box;
    width: 300px;
    padding: 20px;
    border: 10px solid;
}

/* border-box — total: 300px, content: 240px */
.border-box {
    box-sizing: border-box;
    width: 300px;
    padding: 20px;
    border: 10px solid;
}
```

### Example 3: Overflow ellipsis

```css
.truncate {
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis;
    max-width: 250px;
}

/* Multi-line truncation */
.truncate-2 {
    display: -webkit-box;
    -webkit-line-clamp: 2;
    -webkit-box-orient: vertical;
    overflow: hidden;
}
```

## Glossary

| Term | Definition |
|---|---|
| Content box | Inmost area containing text and child elements |
| Padding | Space between content and border (inside the element) |
| Border | Edge line around the padding |
| Margin | Space outside the border between elements |
| Box-sizing | Property controlling width/height calculation mode |
| Collapsing margins | Adjacent vertical margins merging into one |
| Border-radius | Rounding of border corners |
| Box-shadow | Visual shadow extending beyond border edge |
| Overflow | Behavior when content exceeds the content box |
| Outline | Non-layout-taking focus/border indicator |

## Quick References

- [MDN: Box Model](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_box_model) — visual explanation
- [MDN: box-sizing](https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing) — border-box vs content-box
- [MDN: Margin Collapsing](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_box_model/Mastering_margin_collapsing) — collapse rules in detail
- [CSS Box Model Level 3](https://www.w3.org/TR/css-box-3/) — W3C specification
- [Box Shadow Generator](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_backgrounds_and_borders/Box-shadow_generator) — browser tool

## Next Steps

- [Values & Units](values-and-units.md) — understanding CSS length and value types
- [Custom Properties](custom-properties.md) — CSS variables and dynamic theming