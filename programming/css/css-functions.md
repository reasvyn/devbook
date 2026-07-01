# CSS Functions

## Description

CSS includes dozens of built-in functions for calculations, color manipulation, filtering, gradients, transforms, and more. Understanding the function landscape helps you write expressive, concise styles.

## Prerequisites

- [Values & Units](values-and-units.md) — function inputs and outputs
- [Custom Properties](custom-properties.md) — var() in context

## Table of Contents

- [Mathematical Functions](#mathematical-functions)
- [Comparison Functions](#comparison-functions)
- [Color Functions](#color-functions)
- [Gradient Functions](#gradient-functions)
- [Filter Functions](#filter-functions)
- [Transform Functions](#transform-functions)
- [Animation Functions](#animation-functions)
- [Shape Functions](#shape-functions)
- [Image Functions](#image-functions)
- [Reference Functions](#reference-functions)
- [Font Functions](#font-functions)
- [Grid Functions](#grid-functions)
- [Selector Functions](#selector-functions)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Mathematical Functions

**`calc()`** — arithmetic on any length values:

```css
.element {
    width: calc(100% - 2rem);
    height: calc(100vh - 60px - 2rem);
    font-size: calc(1rem + 0.5vw);
    padding: calc(1rem + ((100vw - 768px) / 1000));
}
```

Whitespace around `+` and `-` is required. `*` and `/` are more forgiving but add spaces for readability.

**`abs()`** — absolute value:

```css
.element {
    margin: abs(-2rem);          /* 2rem */
    translate: abs(var(--offset));
}
```

**`sign()`** — sign of a value (-1, 0, 1):

```css
.element {
    rotate: calc(45deg * sign(var(--direction)));
}
```

**`round()`** — round to nearest multiple:

```css
.element {
    width: round(100vw, 100px);  /* Round viewport width to nearest 100px */
    font-size: round(1.37rem, 0.25rem);  /* Round to nearest 0.25rem */
}
```

**`mod()`** and **`rem()`** — modulo and remainder:

```css
.element {
    margin: mod(100%, 3);        /* Useful for grid offsets */
}
```

### Comparison Functions

**`min()`** — smallest value:

```css
.element {
    width: min(100%, 400px);     /* Responsive: caps at 400px */
    font-size: min(2rem, 5vw);   /* Prefer vw but cap at 2rem */
}
```

**`max()`** — largest value:

```css
.element {
    width: max(50%, 300px);      /* At least 300px */
    padding: max(0.5rem, 1vw);   /* Minimum 0.5rem */
}
```

**`clamp()`** — value within range:

```css
.element {
    font-size: clamp(1rem, 2.5vw, 2rem);
    width: clamp(300px, 80%, 1200px);
    padding: clamp(0.5rem, 2vw, 1.5rem);
}
```

### Color Functions

**RGB family:**

```css
.element {
    color: rgb(255 0 0);                 /* Legacy: rgb(255, 0, 0) */
    color: rgba(255 0 0 / 0.5);          /* With alpha */
    color: rgb(from #0066cc r g b / 0.8); /* Relative syntax */
}
```

**HSL family:**

```css
.element {
    color: hsl(0 100% 50%);              /* Hue, saturation, lightness */
    color: hsla(0 100% 50% / 0.5);       /* With alpha */
    color: hsl(from var(--primary) h s l); /* Derive from variable */
}
```

**HWB:**

```css
.element {
    color: hwb(0 0% 0%);                 /* Hue, whiteness, blackness */
    color: hwb(210 30% 20% / 0.5);       /* With alpha */
}
```

**LAB and OKLAB:**

```css
.element {
    color: lab(50% 20 30);               /* Lightness, a-axis, b-axis */
    color: oklab(0.5 0.05 0.1);          /* Perceptually uniform LAB */
}
```

**LCH and OKLCH:**

```css
.element {
    color: lch(50% 30 300deg);           /* Lightness, chroma, hue */
    color: oklch(0.5 0.2 250deg);        /* Perceptually uniform LCH */
}
```

OKLCH is preferred for design systems because perceptual lightness matches human vision — a `lightness` of 70% looks similarly bright across all hues.

**`color()`** — any color space:

```css
.element {
    color: color(display-p3 1 0 0);      /* Display P3 gamut */
    color: color(srgb 0.5 0.5 0.5);      /* Explicit sRGB */
    color: color(srgb-linear 0.5 0.5 0.5);
}
```

**`color-mix()`** — mix two colors:

```css
.element {
    color: color-mix(in srgb, red, blue);
    color: color-mix(in oklch, var(--primary) 30%, white);
    background: color-mix(in oklch, var(--bg) 80%, var(--accent));
}
```

**`light-dark()`** — theme-adaptive color (2024+):

```css
.element {
    color: light-dark(#333, #e0e0e0);    /* Light: #333, Dark: #e0e0e0 */
    background: light-dark(white, #1a1a1a);
}
```

### Gradient Functions

**Linear gradient:**

```css
.element {
    background: linear-gradient(red, blue);
    background: linear-gradient(45deg, red, blue);
    background: linear-gradient(to right, red 0%, blue 50%, green 100%);
    background: repeating-linear-gradient(45deg, red, blue 10px);
}
```

**Radial gradient:**

```css
.element {
    background: radial-gradient(circle, red, blue);
    background: radial-gradient(ellipse at center, red 0%, blue 50%, transparent 100%);
    background: repeating-radial-gradient(circle, red, blue 10px);
}
```

**Conic gradient:**

```css
.element {
    background: conic-gradient(red, yellow, green, blue, red);
    background: conic-gradient(from 0deg, red, blue, red);
}
```

**`image-set()`** — resolution-aware images:

```css
.element {
    background-image: image-set(
        url('bg.avif') type('image/avif'),
        url('bg.webp') type('image/webp'),
        url('bg.jpg') type('image/jpeg')
    );
    background-image: image-set(
        'bg.png' 1x,
        'bg@2x.png' 2x
    );
}
```

**`cross-fade()`** — blend two images:

```css
.element {
    background: cross-fade(url(a.jpg) 50%, url(b.jpg) 50%);
}
```

### Filter Functions

Filters apply visual effects to elements:

```css
.element {
    filter: blur(4px);
    filter: brightness(1.5);            /* 1.5 = 150% brightness */
    filter: contrast(200%);
    filter: drop-shadow(2px 4px 6px rgba(0,0,0,0.3));
    filter: grayscale(100%);
    filter: hue-rotate(90deg);
    filter: invert(100%);
    filter: opacity(50%);
    filter: saturate(200%);
    filter: sepia(80%);
}

/* Multiple filters */
.element {
    filter: brightness(1.2) contrast(1.1) saturate(1.1);
}

/* Backdrop filter — applies to background behind element */
.element {
    backdrop-filter: blur(8px) brightness(0.8);
}
```

The `backdrop-filter` applies to the area behind the element, creating frosted glass effects.

### Transform Functions

**2D transforms:**

```css
.element {
    transform: translate(10px, 20px);
    transform: translateX(50%);
    transform: translateY(-1rem);
    transform: scale(1.5);
    transform: scaleX(0.5);
    transform: scaleY(2);
    transform: rotate(45deg);
    transform: skew(10deg, 5deg);
    transform: skewX(10deg);
}

/* Multiple transforms */
.element {
    transform: translate(10px) rotate(45deg) scale(1.2);
}
```

**3D transforms:**

```css
.element {
    transform: perspective(800px) translateZ(50px);
    transform: rotateX(45deg);
    transform: rotateY(180deg);
    transform: rotateZ(45deg);
    transform: translate3d(10px, 20px, 30px);
    transform: scale3d(1.5, 0.5, 2);
    transform: matrix3d(...);           /* 4×4 matrix — complex */
}
```

**`translate()` with percentage:** Percentage is relative to the element's own dimensions:

```css
/* Perfect centering with position: absolute */
.centered {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}
```

**Individual transform properties (2024+):**

```css
.element {
    translate: 10px 20px;
    rotate: 45deg;
    scale: 1.2;
    /* Cleaner than transform: translate() rotate() scale() */
}
```

Individual transforms avoid ordering issues (e.g., rotate then translate vs translate then rotate produce different results).

### Animation Functions

**Cubic bezier:**

```css
.element {
    transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);
    animation-timing-function: cubic-bezier(0.68, -0.55, 0.27, 1.55); /* Bounce */
}
```

**Steps:**

```css
.element {
    animation-timing-function: steps(4);        /* 4 discrete frames */
    animation-timing-function: steps(4, start); /* Jump at start of each step */
    animation-timing-function: steps(4, end);   /* Jump at end of each step */
    animation-timing-function: step-start;       /* Equivalent to steps(1, start) */
    animation-timing-function: step-end;         /* Equivalent to steps(1, end) */
}
```

**Path for motion:**

```css
.element {
    offset-path: path('M 0 0 L 100 100 C 200 0 300 100 400 0');
    animation: move 3s linear infinite;
}

@keyframes move {
    from { offset-distance: 0%; }
    to { offset-distance: 100%; }
}
```

### Shape Functions

**`clip-path()`** — define visible area:

```css
.element {
    clip-path: circle(50%);
    clip-path: ellipse(50% 30%);
    clip-path: polygon(50% 0%, 100% 50%, 50% 100%, 0% 50%);
    clip-path: inset(10% 20% 30% 40% round 8px);
}
```

**`path()`** — in clip-path:

```css
.element {
    clip-path: path('M0 0 L100 0 L100 100 L0 100 Z');
}
```

**`shape-outside()`** — text wrapping shape:

```css
.element {
    float: left;
    shape-outside: circle(50%);
    shape-outside: polygon(0 0, 100% 0, 80% 100%, 0 100%);
}
```

### Image Functions

**`url()`** — reference external resource:

```css
.element {
    background-image: url('bg.png');
    background-image: url('/images/bg.png');
    background-image: url(https://example.com/bg.png);
    cursor: url('cursor.cur'), auto;
    list-style-image: url('bullet.png');
}
```

**`image()`** — advanced image with fallbacks:

```css
.element {
    background: image('bg.webp', 'bg.png', #ccc);
    background: image('bg.svg', 'bg.png');
}
```

**`element()`** — use another element as an image (Firefox only):

```css
.element {
    background: element(#source);
}
```

### Reference Functions

**`var()`** — custom property reference:

```css
.element {
    color: var(--primary, #0066cc);
    margin: var(--space, 1rem);
}
```

**`attr()`** — HTML attribute value as CSS:

```css
.element::after {
    content: attr(data-label);              /* String */
    content: attr(data-count, number, 0);   /* Number with default */
}

/* CSS3 attr() limited to content property */
/* CSS4+ attr() may work on all properties */
```

**`env()`** — environment variables:

```css
/* Safe area insets (notch, home indicator) */
.element {
    padding-top: env(safe-area-inset-top, 0px);
    padding-right: env(safe-area-inset-right, 0px);
    padding-bottom: env(safe-area-inset-bottom, 0px);
    padding-left: env(safe-area-inset-left, 0px);
}

/* Viewport segments (foldable devices) */
.element {
    margin-left: env(viewport-segment-left, 0px);
}
```

### Font Functions

**`format()`** — font format hint in `@font-face`:

```css
@font-face {
    font-family: 'Inter';
    src: url('Inter.woff2') format('woff2'),
         url('Inter.woff') format('woff');
}
```

**`tech()`** — font technology hint:

```css
@font-face {
    font-family: 'Inter';
    src: url('Inter.woff2') format('woff2 supports variations'),
         url('Inter-static.woff2') format('woff2');
}
```

### Grid Functions

**`repeat()`** — repeat grid tracks:

```css
.grid {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
    grid-template-rows: repeat(2, auto);
}
```

**`minmax()`** — track size range:

```css
.grid {
    grid-template-columns: minmax(200px, 1fr) minmax(300px, 2fr);
    grid-template-columns: repeat(3, minmax(0, 1fr));
}
```

**`fit-content()`** — size between min and max:

```css
.grid {
    grid-template-columns: fit-content(300px) 1fr 1fr;
}
```

### Selector Functions

**`:is()`** — group selectors with specificity of most specific argument:

```css
:is(section, article, aside) :is(h1, h2) { }
/* Specificity: most specific argument wins */
```

**`:where()`** — group selectors with zero specificity:

```css
:where(section, article, aside) :where(h1, h2) { }
/* Specificity: 0,0,0,0 */
```

**`:has()`** — parent selector:

```css
.figure:has(img) { }
.form:has(:invalid) { }
/* Selector function — matches parent based on children */
```

**`:not()`** — negate selector:

```css
p:not(.intro) { }
input:not([type="checkbox"]):not([type="radio"]) { }
```

**`:nth-child()` with An+B syntax:**

```css
li:nth-child(2n+1) { }      /* Odd */
li:nth-child(2n) { }        /* Even */
li:nth-child(-n+3) { }      /* First 3 */
li:nth-child(n+4) { }       /* Fourth onward */
li:nth-child(3n+1) { }      /* 1st, 4th, 7th... */
```

## Glossary

| Term | Definition |
|---|---|
| calc() | Arithmetic on CSS length values |
| clamp() | Value restricted to a min/max range |
| color-mix() | Blend two colors in a given color space |
| env() | Access environment variables (safe area, etc.) |
| filter() | Apply visual effects like blur, brightness |
| backdrop-filter | Filter applied behind the element |
| var() | Retrieve custom property value |
| image-set() | Resolution/format-aware image selection |
| minmax() | Grid track size range (min to max) |
| fit-content() | Grid track sizing between min-content and max |

## Quick References

- [MDN: CSS Functions](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Functions) — complete reference
- [CSS Values Level 4](https://www.w3.org/TR/css-values-4/) — all mathematical functions
- [CSS Color Level 4](https://www.w3.org/TR/css-color-4/) — color functions
- [CSS Image Values Level 4](https://www.w3.org/TR/css-images-4/) — image functions
- [Caniuse: CSS Functions](https://caniuse.com/?search=css%20functions) — browser support matrix

## Next Steps

- [Display Types](display-types.md) — how display values affect layout behavior