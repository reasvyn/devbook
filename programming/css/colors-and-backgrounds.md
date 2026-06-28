# Colors & Backgrounds

## Description

CSS provides rich controls for color and background styling, from simple solid fills to complex gradients, patterns, and layering.

## Prerequisites

- [Values & Units](values-and-units.md) — color values, length units
- [The Box Model](box-model.md) — background extends through padding area

## Table of Contents

- [The Color Property](#the-color-property)
- [Background Color](#background-color)
- [Background Image](#background-image)
- [Background Repeat](#background-repeat)
- [Background Position](#background-position)
- [Background Size](#background-size)
- [Background Attachment](#background-attachment)
- [Background Clip](#background-clip)
- [Background Origin](#background-origin)
- [The Background Shorthand](#the-background-shorthand)
- [Multiple Backgrounds](#multiple-backgrounds)
- [Gradients as Backgrounds](#gradients-as-backgrounds)
- [Background Blend Modes](#background-blend-modes)
- [Opacity and Alpha](#opacity-and-alpha)
- [Color Schemes](#color-schemes)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Color Property

The `color` property sets the foreground color of text and affects `currentColor`:

```css
body {
    color: #1a1a1a;
}

a {
    color: #0066cc;
}

.muted {
    color: #6b7280;
}

.highlight {
    color: oklch(0.5 0.2 250deg);   /* Perceptual color */
}
```

The value of `color` is inherited and serves as the base for `currentColor` used in borders, shadows, and SVG fills.

### Background Color

Solid background fills:

```css
.element {
    background-color: #f8f9fa;
    background-color: rgb(248, 249, 250);
    background-color: hsl(210, 10%, 95%);
    background-color: rgba(0, 0, 0, 0.05);
    background-color: transparent;
    background-color: currentColor;
}
```

Background color fills the padding area in addition to the content area. It does NOT extend into margin.

### Background Image

Image and gradient backgrounds:

```css
.element {
    background-image: url('pattern.png');
    background-image: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    background-image: none;         /* Remove background */
}
```

Multiple images stack (first on top):

```css
.element {
    background-image:
        url('overlay.png'),
        linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}
```

### Background Repeat

Control how background images tile:

```css
.element {
    background-repeat: repeat;          /* Tile both directions (default) */
    background-repeat: repeat-x;        /* Tile horizontally only */
    background-repeat: repeat-y;        /* Tile vertically only */
    background-repeat: no-repeat;       /* Single instance */
    background-repeat: space;           /* Tile with even spacing */
    background-repeat: round;           /* Tile with scaling to fit */
}

/* Two-value syntax (horizontal, vertical) */
.element {
    background-repeat: repeat no-repeat;  /* Horizontal: repeat, vertical: no-repeat */
    background-repeat: space round;
}
```

`space` adds gap between tiles to fill the container. `round` scales tiles so an integer number fits exactly.

### Background Position

Place a non-repeating image:

```css
.element {
    background-position: center;
    background-position: top left;
    background-position: 50% 50%;         /* X% Y% */
    background-position: 20px 40px;       /* Xpx Ypx */
    background-position: right 20px bottom 10px;  /* Offset from edges */
}
```

Two-value syntax — first is horizontal, second is vertical:

```css
.element {
    background-position: 30% 70%;
    background-position: left center;       /* left = horizontal, center = vertical */
    background-position: center 10%;        /* center = horizontal, 10% = vertical */
}
```

Four-value syntax (edge + offset):

```css
.element {
    background-position: right 20px top 10px;  /* 20px from right, 10px from top */
    background-position: left 2rem bottom 1rem;
}
```

### Background Size

Control the dimensions of background images:

```css
.element {
    background-size: auto;               /* Natural size */
    background-size: cover;              /* Fill container (may crop) */
    background-size: contain;            /* Fit entirely (may leave gaps) */
    background-size: 200px 100px;        /* Explicit width/height */
    background-size: 50% 50%;            /* Percentage of background positioning area */
    background-size: 100% auto;          /* Full width, auto height */
}
```

`cover` ensures the image fills the entire container; parts may be clipped. `contain` ensures the entire image is visible; there may be empty space.

Multiple backgrounds with different sizes:

```css
.element {
    background-image: url('pattern.png'), url('hero.jpg');
    background-size: 50px 50px, cover;
}
```

### Background Attachment

Control if the background scrolls with the content:

```css
.element {
    background-attachment: scroll;       /* Scrolls with element (default) */
    background-attachment: fixed;        /* Fixed relative to viewport */
    background-attachment: local;        /* Scrolls with element's content */
}
```

`fixed` creates a parallax effect — the background stays in place while the content scrolls over it. Performance cost: fixed backgrounds prevent some GPU compositing optimizations.

### Background Clip

Defines how far the background extends:

```css
.element {
    background-clip: border-box;        /* Extends under border (default) */
    background-clip: padding-box;       /* Extends to padding edge */
    background-clip: content-box;       /* Only under content area */
    background-clip: text;              /* Clips to text glyphs */
}
```

`background-clip: text` makes the background visible only through the text:

```css
.gradient-text {
    background: linear-gradient(135deg, #667eea, #764ba2);
    background-clip: text;
    -webkit-background-clip: text;      /* Safari support */
    color: transparent;
    font-weight: 800;
    font-size: 3rem;
}
```

### Background Origin

Defines the positioning area origin:

```css
.element {
    background-origin: padding-box;     /* Position relative to padding edge (default) */
    background-origin: border-box;      /* Position relative to border edge */
    background-origin: content-box;     /* Position relative to content edge */
}
```

Difference from `background-clip`: origin determines where the coordinate system starts (top-left of what area), clip determines where the background paints.

`background-origin` and `background-clip` often pair:
- `no-repeat` image positioned at corner: origin matters (border-box starts at border corner)
- Pattern tiling: clip determines how close to the edge the pattern fills

### The Background Shorthand

The `background` shorthand combines all background sub-properties:

```css
.element {
    background: #f8f9fa url('hero.jpg') no-repeat center / cover;
}

/* Order: color image repeat attachment position / size origin clip */
.element {
    background: #1a1a1a url('bg.png') no-repeat scroll center / auto content-box;
}
```

Not all values are required — omitted sub-properties reset to defaults:

```css
.element {
    background: url('img.png');   /* Resets all other bg props to default */
}
```

If you only want to set background-color, use the longhand `background-color` to avoid resetting other values.

### Multiple Backgrounds

Layer multiple backgrounds (comma-separated):

```css
.hero {
    background:
        linear-gradient(180deg, rgba(0,0,0,0.5) 0%, transparent 50%),
        url('hero.webp') center / cover no-repeat;
    background-color: #1a1a1a;           /* Shows where gradients are transparent */
}

.card {
    background:
        radial-gradient(circle at 20% 30%, rgba(255,255,255,0.1) 0%, transparent 50%),
        url('texture.png') repeat,
        linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}
```

First listed is painted last (on top), last listed is painted first (at the bottom).

### Gradients as Backgrounds

**Linear gradients:**

```css
.element {
    background: linear-gradient(red, blue);
    background: linear-gradient(45deg, red, blue);
    background: linear-gradient(to bottom right, #f00, #00f);
    background: repeating-linear-gradient(
        45deg,
        #fff 0px,
        #fff 10px,
        #ccc 10px,
        #ccc 20px
    );
}
```

Color stops:

```css
.element {
    background: linear-gradient(
        to right,
        red 0%,
        red 25%,
        yellow 25%,
        yellow 50%,
        green 50%,
        green 75%,
        blue 75%,
        blue 100%
    );
}

/* Hard stops for stripes */
.element {
    background: linear-gradient(
        90deg,
        #0066cc 50%,
        #004499 50%
    );
    background-size: 20px 100%;
}
```

**Radial gradients:**

```css
.element {
    background: radial-gradient(circle, red, blue);
    background: radial-gradient(ellipse at center, #667eea 0%, transparent 70%);
    background: repeating-radial-gradient(circle, red 0px, red 10px, blue 10px, blue 20px);
}
```

**Conic gradients (color wheels, sunbursts):**

```css
.element {
    background: conic-gradient(red, yellow, lime, aqua, blue, magenta, red);
    background: conic-gradient(from 0deg at 50% 50%, red, blue, red);
    background: conic-gradient(
        red 0deg,
        red 90deg,
        blue 90deg,
        blue 180deg,
        green 180deg,
        green 270deg,
        yellow 270deg,
        yellow 360deg
    );
}
```

### Background Blend Modes

Blend background layers together:

```css
.element {
    background:
        url('photo.jpg') center / cover,
        linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    background-blend-mode: multiply;      /* Blend the two backgrounds */
    background-blend-mode: overlay;
    background-blend-mode: screen;
    background-blend-mode: color;
    background-blend-mode: luminosity;
}
```

Common blend modes and effects:

| Blend Mode | Effect |
|---|---|
| `multiply` | Darken — good for overlaying gradients on photos |
| `screen` | Lighten |
| `overlay` | Increase contrast |
| `color` | Tint image with color |
| `luminosity` | Apply luminosity of top layer, color of bottom |
| `soft-light` | Subtle contrast boost |

### Opacity and Alpha

Opacity controls the entire element's transparency (including children):

```css
.element {
    opacity: 1;              /* Fully opaque */
    opacity: 0.5;            /* 50% transparent */
    opacity: 0;              /* Fully transparent (still interactive) */
    opacity: 0; visibility: hidden;  /* Hidden and non-interactive */
}
```

Alpha channel applies transparency to individual colors:

```css
.element {
    color: rgba(0, 0, 0, 0.5);          /* 50% transparent text */
    background-color: rgba(255, 0, 0, 0.3);
    border-color: hsla(0, 0%, 0%, 0.2);
}
```

Difference between `opacity: 0` and `visibility: hidden`:
- `opacity: 0` — element is invisible but still in layout, still receives clicks, still in accessibility tree
- `visibility: hidden` — invisible, not interactive, hidden from accessibility tree, still takes space
- `display: none` — removed from layout, not interactive, hidden from accessibility

### Color Schemes

The `color-scheme` property tells the browser which theme the page supports:

```css
:root {
    color-scheme: light dark;           /* Both themes supported */
}

.dark-mode-only {
    color-scheme: dark;                 /* Only dark */
}

.light-mode-only {
    color-scheme: light;                /* Only light */
}
```

When `color-scheme` is set, the browser adjusts form controls, scrollbars, and other UI elements to match the scheme.

The `light-dark()` function produces different colors per scheme:

```css
.element {
    color: light-dark(#333, #e0e0e0);
    background: light-dark(white, #1a1a1a);
}
```

Combined with `color-scheme`, this enables theme adaptation without media queries.

## Study Cases

### Hero Section with Gradient Overlay

```css
.hero {
    position: relative;
    min-height: 60vh;
    display: flex;
    align-items: center;
    justify-content: center;

    background:
        linear-gradient(
            180deg,
            rgba(0, 0, 0, 0.6) 0%,
            rgba(0, 0, 0, 0.2) 40%,
            transparent 60%,
            rgba(0, 0, 0, 0.4) 100%
        ),
        url('hero.webp') center / cover no-repeat;
}

.hero-content {
    color: white;
    text-align: center;
}
```

### Themed Card with Color Blending

```css
.card {
    background:
        radial-gradient(circle at 30% 20%, rgba(255,255,255,0.1) 0%, transparent 50%),
        var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 1.5rem;
    transition: background 0.3s;
}

.card:hover {
    background:
        radial-gradient(circle at 30% 20%, rgba(255,255,255,0.2) 0%, transparent 50%),
        var(--surface-hover);
}
```

### Checkerboard Pattern

```css
.checkerboard {
    background:
        conic-gradient(#ccc 25%, white 0deg 50%, #ccc 0deg 75%, white 0deg) 0 0 / 20px 20px;
}
```

## Examples

### Example 1: Text gradient

```css
.gradient-heading {
    background: linear-gradient(135deg, #667eea, #764ba2, #f093fb);
    background-clip: text;
    -webkit-background-clip: text;
    color: transparent;
    font-size: 3rem;
    font-weight: 800;
}
```

### Example 2: Grainy texture overlay

```css
.textured {
    background:
        url('noise.png') repeat,
        linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    background-blend-mode: overlay;
}
```

The noise image (a transparent PNG with grain) blends with the gradient to create a rich textured surface.

### Example 3: Shimmer loading animation

```css
.shimmer {
    background:
        linear-gradient(
            90deg,
            #f0f0f0 25%,
            #e0e0e0 37%,
            #f0f0f0 63%
        );
    background-size: 200% 100%;
    animation: shimmer 1.5s ease-in-out infinite;
}

@keyframes shimmer {
    0% { background-position: 200% 0; }
    100% { background-position: -200% 0; }
}
```

## Glossary

| Term | Definition |
|---|---|
| Alpha | Transparency channel (0 = transparent, 1 = opaque) |
| Gradient | Smooth transition between colors |
| Color stop | Specific position in a gradient |
| Blend mode | Mathematical color compositing algorithm |
| Background clip | Area the background paints within |
| Background origin | Coordinate origin for background positioning |
| Cover | Scale image to fill container (may crop) |
| Contain | Scale image to fit entirely (may gap) |
| Color scheme | Light/dark preference indicator |
| currentColor | Keyword resolving to the element's color value |

## Quick References

- [MDN: Color](https://developer.mozilla.org/en-US/docs/Web/CSS/color) — color property reference
- [MDN: Background](https://developer.mozilla.org/en-US/docs/Web/CSS/background) — background property reference
- [CSS Color Level 4](https://www.w3.org/TR/css-color-4/) — color specification
- [CSS Backgrounds Level 3](https://www.w3.org/TR/css-backgrounds-3/) — backgrounds specification
- [Gradient Generator](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_images/Using_CSS_gradients) — browser tool

## Next Steps

- [Web Fonts](web-fonts.md) — custom fonts with @font-face