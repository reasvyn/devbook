# CSS Values & Units

## Description

CSS has a rich set of value types and units for sizing, spacing, color, timing, and more. Understanding what each value represents and when to use it is essential for building predictable layouts.

## Prerequisites

- [The Box Model](box-model.md) — how sizing units affect layout
- [Cascade, Specificity & Inheritance](cascade-and-specificity.md) — how computed values resolve

## Table of Contents

- [Value Categories](#value-categories)
- [Length Units](#length-units)
- [Absolute vs. Relative](#absolute-vs-relative)
- [Percentages](#percentages)
- [em and rem](#em-and-rem)
- [Viewport Units](#viewport-units)
- [Container Query Units](#container-query-units)
- [ch and ex](#ch-and-ex)
- [lh and rlh](#lh-and-rlh)
- [Color Values](#color-values)
- [Numeric Values](#numeric-values)
- [Time Values](#time-values)
- [Angle Values](#angle-values)
- [Resolution Values](#resolution-values)
- [String Values](#string-values)
- [Custom Identifiers](#custom-identifiers)
- [The calc() Function](#the-calc-function)
- [The min(), max(), clamp() Functions](#the-min-max-clamp-functions)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Value Categories

Every CSS property accepts values of a specific type. The major categories:

| Category | Examples | Properties |
|---|---|---|
| `<length>` | `16px`, `1.5rem`, `100%` | `width`, `margin`, `font-size` |
| `<color>` | `red`, `#ff0000`, `rgb(255 0 0)` | `color`, `background`, `border-color` |
| `<number>` | `1`, `1.5`, `0.8` | `opacity`, `line-height`, `flex` |
| `<percentage>` | `50%`, `100%` | `width`, `translate`, `background-size` |
| `<time>` | `0.3s`, `2s`, `300ms` | `transition-duration`, `animation-duration` |
| `<angle>` | `45deg`, `0.5turn` | `rotate`, `transform`, `linear-gradient` |
| `<string>` | `'Hello'`, `"caption-1"` | `content`, `font-family` |
| `<custom-ident>` | `my-animation`, `grid-area-name` | `animation-name`, `grid-area` |

### Length Units

CSS length units divide into absolute and relative:

**Absolute units:**

| Unit | Name | Equivalent |
|---|---|---|
| `cm` | Centimeters | 1cm = 37.8px |
| `mm` | Millimeters | 1mm = 1/10cm |
| `Q` | Quarter-millimeters | 1Q = 1/40cm |
| `in` | Inches | 1in = 96px |
| `pc` | Picas | 1pc = 16px |
| `pt` | Points | 1pt = 1/72in |
| `px` | Pixels | 1px = 1/96in |

In practice, `px` is the only absolute unit used in web development. The others are for print stylesheets.

**Relative units:**

| Unit | Relative to |
|---|---|
| `em` | Font size of the element |
| `rem` | Font size of the root element (`<html>`) |
| `vw` | 1% of viewport width |
| `vh` | 1% of viewport height |
| `vmin` | 1% of the smaller viewport dimension |
| `vmax` | 1% of the larger viewport dimension |
| `svw` / `svh` | 1% of small viewport (address bar visible) |
| `lvw` / `lvh` | 1% of large viewport (address bar hidden) |
| `dvw` / `dvh` | 1% of dynamic viewport (changes with toolbar) |
| `cqw` / `cqh` | 1% of container query container |
| `cqi` | 1% of container's inline size |
| `cqb` | 1% of container's block size |
| `cqmin` / `cqmax` | Smaller/larger of cqi and cqb |
| `ch` | Width of the "0" glyph in the element's font |
| `ex` | Height of the "x" glyph in the element's font |
| `lh` | Line height of the element |
| `rlh` | Line height of the root element |

### Absolute vs. Relative

Rule of thumb:
- **Use relative units** for most work — they adapt to user preferences and screen sizes.
- **Use absolute units** (`px`) for borders, shadows, and fine details where consistent physical size matters.
- **Avoid `pt`, `pc`, `in`, `cm`, `mm`** on screens — they are for print.

```css
/* Good */
.card {
    padding: 1.5rem;     /* Adapts to root font-size */
    border: 1px solid;   /* Fine detail — px is fine */
    border-radius: 8px;  /* Fine detail */
    font-size: 1rem;     /* Respects user's default */
    margin-bottom: 1em;  /* Relative to element's font-size */
}

/* Avoid */
.card {
    padding: 24px;       /* Does not scale with user preferences */
    font-size: 16px;     /* Does not respect user's default */
    margin-bottom: 12px; /* Recalculate everywhere on font change */
}
```

### Percentages

Percentages are relative to another value. What they are relative to depends on the property:

```css
.element {
    width: 50%;          /* Relative to parent's width */
    margin-left: 10%;    /* Relative to parent's width */
    padding: 5%;         /* Relative to parent's width */
    font-size: 150%;     /* Relative to parent's font-size */
    line-height: 150%;   /* Relative to the element's font-size */
    bottom: 20%;         /* Relative to parent's height */
    translate: 50%;      /* Relative to element's own width */
}
```

Key points:
- `width` percentage is relative to the containing block's width.
- `height` percentage is relative to the containing block's height, but only if the parent has an explicit height. If the parent's height is `auto`, percentage height behaves as `auto`.
- `padding` and `margin` percentages are always relative to the **width** of the containing block (not height, even for top/bottom padding).
- `transform: translate(50%, 50%)` is relative to the element's own dimensions.
- `font-size` percentage is relative to the parent's `font-size`.

### em and rem

**The `rem` unit** — relative to root font-size:

```css
html { font-size: 16px; }      /* Default, but user may override */

h1 { font-size: 2.5rem; }      /* 40px if root is 16px */
p { font-size: 1rem; }         /* 16px */
small { font-size: 0.875rem; } /* 14px */

.card { padding: 1.5rem; }     /* 24px — consistent regardless of nesting */
```

`rem` provides consistency — the same value always means the same size.

**The `em` unit** — relative to the element's own font-size:

```css
.card {
    font-size: 1.25rem;       /* 20px if root is 16px */
    padding: 1em;             /* 20px — matches the card's font-size */
    margin-bottom: 1.5em;     /* 30px */
}

.card h2 {
    font-size: 1.2em;         /* 24px — 1.2 * parent (card) font-size = 1.2 * 20px */
}
```

`em` compounds with nesting:

```css
/* Nested em compounding */
ul { font-size: 0.9em; }     /* Compounding problem */
```

```html
<ul>
    <li>Level 1 — 0.9em of parent (14.4px)
        <ul>
            <li>Level 2 — 0.9em of 14.4px (12.96px)
                <ul>
                    <li>Level 3 — 0.9em of 12.96px (11.66px)</li>
                </ul>
            </li>
        </ul>
    </li>
</ul>
```

Use `rem` for font sizes and spacing to avoid compounding. Use `em` when you want a value to scale with the element's font-size (e.g., padding around a button with varying text size).

### Viewport Units

Viewport units are relative to the browser's viewport:

```css
/* Classic viewport units */
.hero {
    height: 100vh;              /* Full viewport height */
}

.full-width {
    width: 100vw;               /* Full viewport width — may cause horizontal scrollbar */
}

/* Modern viewport variants */
.hero {
    height: 100dvh;             /* Dynamic viewport height — adapts to toolbar hide/show */
}

.sticky-bottom {
    height: 100svh;             /* Small viewport — shortest possible */
    /* Content is visible even with address bar showing */
}

.vmin-demo {
    width: 50vmin;              /* 50% of the smaller dimension */
}

.vmax-demo {
    width: 50vmax;              /* 50% of the larger dimension */
}
```

**Important caveat:** `100vw` includes the scrollbar width. If the page has a scrollbar, `100vw` is wider than the visible area. Fix:

```css
body {
    width: 100%;                /* Better — excludes scrollbar */
    overflow-x: hidden;
}
```

### Container Query Units

Container query units are relative to the nearest query container:

```css
/* Define a container */
.card-container {
    container-type: inline-size;
}

/* Inside the container */
.card {
    /* All relative to the container's inline size */
    padding: 2cqi;              /* 2% of container's inline size */
    font-size: clamp(1rem, 3cqi, 2rem);    /* Responsive font based on container */
    margin: 1cqb;               /* 1% of container's block size */
}
```

Use container units for truly responsive components that adapt to their parent, not the viewport.

### ch and ex

These typographic units:

```css
/* Measure line width for readability */
p {
    max-width: 65ch;            /* ~65 characters wide — ideal for reading */
}

/* Use ex for vertical sizing relative to font's x-height */
.small-caps {
    font-size: 0.8ex;           /* Roughly x-height of parent */
}
```

The `ch` unit is particularly useful for setting readable line lengths. Research suggests 45–75 characters per line for optimal readability.

### lh and rlh

Line-relative units:

```css
p {
    margin-bottom: 1lh;         /* One line height of this element */
}

article {
    line-height: 1.5rlh;        /* Relative to root's line height */
}
```

Useful for rhythm and baseline alignment.

### Color Values

CSS offers several color notations:

**Named colors:**

```css
.element {
    color: red;
    background: rebeccapurple;
    border-color: transparent;
    outline-color: currentColor;   /* Inherits from color property */
}
```

There are 148 named colors in CSS. Use them sparingly — hex or functional notations give more control.

**Hex notation (sRGB):**

```css
.element {
    color: #ff0000;             /* Red — 6-digit */
    color: #f00;                /* Red — short form */
    color: #ff000088;           /* Red with 50% alpha — 8-digit */
    color: #f008;               /* Red with 50% alpha — short 4-digit */
}
```

**Functional RGB:**

```css
.element {
    color: rgb(255 0 0);            /* Red — space-separated (modern) */
    color: rgb(255, 0, 0);          /* Red — comma-separated (legacy) */
    color: rgba(255 0 0 / 0.5);     /* Red 50% opacity — modern */
    color: rgba(255, 0, 0, 0.5);    /* Red 50% opacity — legacy */
}
```

**Functional HSL:**

```css
.element {
    color: hsl(0 100% 50%);         /* Red */
    color: hsl(0 100% 50% / 0.5);   /* Red 50% opacity */
    color: hsla(0 100% 50% / 0.5);
}
```

HSL is the most intuitive for humans — hue (0–360), saturation (0–100%), lightness (0–100%).

**Modern color spaces (CSS Color Level 4+):**

```css
.element {
    /* HWB — Hue, Whiteness, Blackness */
    color: hwb(0 0% 0%);            /* Red */

    /* LAB — device-independent perceptual lightness */
    color: lab(50% 50 50);

    /* LCH — perceptual lightness, chroma, hue */
    color: lch(50% 50 300deg);

    /* OKLCH — improved perceptual uniformity */
    color: oklch(0.5 0.2 300deg);

    /* OKLAB — uniform color space */
    color: oklab(0.5 0.02 0.02);

    /* Display P3 — wider gamut */
    color: color(display-p3 1 0 0);

    /* Relative color syntax — derive from existing color */
    --primary: #0066cc;
    color: rgb(from var(--primary) r g b / 0.8);    /* Same color, 80% opacity */
    --lighter: hsl(from var(--primary) h s calc(l + 20%));  /* Lighter version */
}
```

OKLCH is becoming popular for design systems because perceptual lightness matches human vision — `lch(70% ...)` looks the same brightness across all hues.

**The `currentColor` keyword:**

```css
.button {
    color: #333;
    border: 1px solid currentColor;  /* Matches #333 */
}

.button--danger {
    color: #dc3545;
    /* Border automatically matches — no need to redefine */
}
```

`currentColor` inherits from the `color` property. Use it for SVG icons, borders, and shadows that should match text color.

### Numeric Values

```css
/* Integers */
z-index: 10;
column-count: 3;

/* Decimals */
opacity: 0.5;                  /* Can omit leading zero: .5 */
line-height: 1.6;
flex: 0.5;

/* Percentages */
width: 50%;

/* Unitless zero works anywhere */
margin: 0;                      /* OK */
padding: 0;                     /* OK */
border: 0;                      /* OK */
/* 0px, 0em, 0rem all resolve to the same — use 0 */
```

CSS3+: All numeric values can specify `<number>` which includes decimals.

### Time Values

Used in animations and transitions:

```css
.transition {
    transition-duration: 0.3s;      /* Seconds */
    transition-delay: 150ms;        /* Milliseconds */
}

.animation {
    animation-duration: 2s;
    animation-delay: 500ms;
}
```

`1s = 1000ms`. Use `ms` for small values (<100ms) and `s` for anything larger.

### Angle Values

Used in transforms, gradients, and conic gradients:

```css
.element {
    transform: rotate(45deg);       /* Degrees — 360deg = full circle */
    transform: rotate(0.125turn);   /* Turns — 1turn = 360deg */
    transform: rotate(0.5rad);      /* Radians — 2π rad = 360deg */
    transform: rotate(400grad);     /* Gradians — 400grad = 360deg */
}

/* Gradients */
.background {
    background: linear-gradient(45deg, red, blue);
    background: conic-gradient(from 0.25turn, red, blue);
}
```

`deg` is the most common. `turn` is useful for intuitive fractions (0.5turn = 180deg).

### Resolution Values

Used in media queries for device resolution:

```css
@media (min-resolution: 96dpi) { }          /* Dots per inch */
@media (min-resolution: 2dppx) { }          /* Dots per pixel — 2x displays */
@media (min-resolution: 300dpi) { }
```

`dppx` is the most practical — 1dppx = 96dpi = standard display, 2dppx = Retina.

### String Values

Strings are used primarily for generated content:

```css
.element::before {
    content: "→ ";                  /* Double quotes */
    content: 'Read more: ';         /* Single quotes */
}

.element[data-label]::before {
    content: attr(data-label);      /* Attribute value as string */
}
```

Escape characters in strings with backslash: `"He said \"Hello\""`.

### Custom Identifiers

Custom identifiers are names you define for animations, grid areas, and `@` rules:

```css
@keyframes slide-in { }             /* "slide-in" is a custom ident */

.element {
    animation-name: slide-in;
    grid-area: sidebar;             /* "sidebar" is a custom ident */
}

/* Custom idents must not be CSS keywords */
/* Invalid: animation-name: none; — "none" is a keyword */
/* Valid: animation-name: none-value; — "none-value" is a custom ident */
```

### The calc() Function

`calc()` performs arithmetic in CSS — anywhere a length value is accepted:

```css
.element {
    width: calc(100% - 2rem);               /* Subtraction */
    height: calc(100vh - 60px);             /* Mixed units */
    font-size: calc(1rem + 0.5vw);          /* Fluid typography */
    padding: calc(1em + 1px);               /* Tiny adjustments */
}

/* Complex calculations */
.element {
    --spacing: 1rem;
    width: calc(100% / 3 - var(--spacing) * 2);
}

/* Nested calc (unnecessary — calc handles nesting automatically) */
.element {
    width: calc(calc(100% - 2rem) / 2);     /* Works but */
    width: calc((100% - 2rem) / 2);         /* Cleaner */
}
```

Rules:
- Operators must be surrounded by whitespace: `calc(100%-2rem)` is invalid.
- `+` and `-` require spaces. `*` and `/` do not, but add them for readability.
- Mixed unit types are allowed: `calc(100vh - 60px)`.
- Division by zero is not allowed.

### The min(), max(), clamp() Functions

Comparison functions give responsive sizing without media queries:

**`min()`** — selects the smallest value:

```css
.element {
    width: min(100%, 400px);        /* Responsive: at most 400px, at least... */
    /* If parent is 600px: 400px (400px is smaller than 600px)
       If parent is 300px: 300px (300px is smaller than 400px) */
}
```

`min()` behaves like an upper bound — the value never exceeds the largest argument.

**`max()`** — selects the largest value:

```css
.element {
    width: max(50%, 300px);         /* At least 300px */
    /* If parent is 800px: 400px (50% = 400px > 300px)
       If parent is 400px: 300px (50% = 200px < 300px) */
}
```

`max()` behaves like a lower bound — the value never drops below the smallest argument.

**`clamp()`** — restricts a value to a range:

```css
.element {
    font-size: clamp(1rem, 2.5vw, 2rem);   /* Fluid typography */
    /* Minimum 1rem, preferred 2.5vw, maximum 2rem */
}

.layout {
    width: clamp(300px, 80%, 1200px);      /* Fluid container */
    margin: 0 auto;
}
```

`clamp(MIN, PREFERRED, MAX)` ensures the value stays within bounds. Preferred value uses the middle argument.

## Study Cases

### Fluid Typography Without Media Queries

```css
/* Base: 16px, scales at 0.5vw, capped at 24px */
h1 {
    font-size: clamp(2rem, 1.5rem + 1.5vw, 3rem);
}

p {
    font-size: clamp(1rem, 0.9rem + 0.4vw, 1.25rem);
}
```

This pattern creates text that scales smoothly between screen sizes without breakpoint jumps.

### Spacing Scale with calc and custom properties

```css
:root {
    --space-unit: 0.25rem;
}

.card {
    padding: calc(var(--space-unit) * 6);  /* 1.5rem = 24px */
    margin-bottom: calc(var(--space-unit) * 4); /* 1rem = 16px */
    gap: calc(var(--space-unit) * 3);       /* 0.75rem = 12px */
}
```

### Full-Width Section Inside Contained Layout

```css
.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 1rem;
}

.full-width {
    width: 100vw;
    margin-left: calc(-50vw + 50%);
}
```

This pushes the element to the edge of the viewport while its parent is centered.

## Examples

### Example 1: Intrinsic sizing with fit-content

```css
/* Size based on content, with a maximum */
.tag {
    width: fit-content;             /* Width matches content */
    max-width: 100%;
    padding: 0.25em 0.75em;
}

/* Available space */
.element {
    width: stretch;                 /* Fill available space (like block default) */
}

/* Minimum content size */
.element {
    width: min-content;             /* Narrowest possible without overflow */
}

/* Maximum content size */
.element {
    width: max-content;             /* Wide as non-wrapped content */
}
```

### Example 2: Relative color syntax

```css
:root {
    --brand: oklch(0.5 0.2 250deg);
    --brand-light: oklch(from var(--brand) l 0.05 h);      /* Reduced chroma */
    --brand-dark: oklch(from var(--brand) 0.3 c h);        /* Darker */
    --brand-subtle: oklch(from var(--brand) 0.9 0.03 h);   /* Very light */
}
```

### Example 3: Typographic rhythm with lh

```css
article {
    line-height: 1.6;
}

article p {
    margin-bottom: 1lh;             /* Matches current line-height */
}

article h2 {
    margin-top: 2lh;
}
```

## Glossary

| Term | Definition |
|---|---|
| Absolute unit | Fixed-size unit (px, cm, in) |
| Relative unit | Unit relative to another value (rem, em, vw) |
| Viewport unit | Unit relative to browser viewport dimensions |
| Container unit | Unit relative to a query container's size |
| calc() | Function for arithmetic on CSS values |
| clamp() | Function restricting value to a min/max range |
| min() / max() | Functions selecting smallest/largest value |
| HSL | Hue, Saturation, Lightness color model |
| LCH | Lightness, Chroma, Hue — perceptual color model |
| OKLCH | Improved perceptual color model |
| currentColor | Keyword that resolves to element's color value |

## Quick References

- [MDN: CSS Values and Units](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Values_and_Units) — complete reference
- [MDN: calc()](https://developer.mozilla.org/en-US/docs/Web/CSS/calc) — calculation function
- [MDN: color()](https://developer.mozilla.org/en-US/docs/Web/CSS/color_value/color) — modern color function
- [CSS Values Level 4](https://www.w3.org/TR/css-values-4/) — official specification
- [Viewport Units Guide](https://web.dev/viewport-units/) — choosing between svh/lvh/dvh
- [Caniuse: clamp()](https://caniuse.com/css-math-functions) - browser support

## Next Steps

- [Custom Properties](custom-properties.md) — CSS variables and dynamic theming
- [CSS Functions](css-functions.md) — comprehensive function reference