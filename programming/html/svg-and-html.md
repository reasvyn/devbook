# SVG in HTML

## Description

Scalable Vector Graphics (SVG) is an XML-based vector image format for two-dimensional graphics. SVGs scale infinitely, can be styled with CSS, animated, and made interactive — making them ideal for icons, logos, charts, diagrams, and data visualizations.

## Prerequisites

- [Elements, Tags & Attributes](elements-and-attributes.md) — fundamental HTML building blocks
- [Images & Responsive Images](images.md) — image formats and embedding

## Table of Contents

- [What is SVG?](#what-is-svg)
- [Embedding SVG in HTML](#embedding-svg-in-html)
- [The SVG Viewport and View Box](#the-svg-viewport-and-view-box)
- [Basic Shapes](#basic-shapes)
- [Styling SVG](#styling-svg)
- [Text in SVG](#text-in-svg)
- [Groups and Defs](#groups-and-defs)
- [Reusable Graphics with `<use>`](#reusable-graphics-with-use)
- [SVG Filters](#svg-filters)
- [SVG Animation](#svg-animation)
- [Interactive SVG](#interactive-svg)
- [Accessibility in SVG](#accessibility-in-svg)
- [SVG Best Practices](#svg-best-practices)
- [SVG vs Canvas](#svg-vs-canvas)
- [Common Mistakes](#common-mistakes)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What is SVG?

SVG (Scalable Vector Graphics) describes images using XML markup. Unlike raster formats (JPEG, PNG) that store pixel data, SVG stores mathematical descriptions of shapes, paths, and text.

```xml
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100">
    <circle cx="50" cy="50" r="40" fill="blue" />
</svg>
```

Properties of SVG:
- **Resolution independent** — scales to any size without quality loss
- **DOM accessible** — SVG elements are part of the DOM and can be scripted
- **CSS stylable** — colors, sizes, and effects can be changed via CSS
- **Small file size** — especially for geometric shapes and icons
- **Searchable** — text in SVGs is indexed by search engines

### Embedding SVG in HTML

**Inline SVG** — the most flexible approach:

```html
<svg viewBox="0 0 24 24" width="24" height="24" role="img" aria-label="Search icon">
    <path d="M15.5 14h-.79l-.28-.27A6.47 6.47 0 0 0 16 9.5 6.5 6.5 0 1 0 9.5 16c1.61 0 3.09-.59 4.23-1.57l.27.28v.79l5 4.99L20.49 19l-4.99-5zm-6 0C7.01 14 5 11.99 5 9.5S7.01 5 9.5 5 14 7.01 14 9.5 11.99 14 9.5 14z"/>
</svg>
```

Inline SVG can be styled with CSS, scripted with JavaScript, and benefits from the full power of the DOM.

**`<img>` tag** — for static SVG files:

```html
<img src="logo.svg" alt="Company logo" width="200" height="100">
```

CSS cannot style SVG elements when loaded via `<img>`. JavaScript cannot access the SVG DOM. Use this for static images that do not need interactivity.

**`<object>` tag** — for external SVG files with script access:

```html
<object data="chart.svg" type="image/svg+xml" width="600" height="400">
    <p>Fallback content for browsers that do not support SVG.</p>
</object>
```

The SVG DOM is accessible from the parent page via `objectElement.contentDocument`. CSS from the parent page does not apply inside the SVG.

**CSS background-image:**

```css
.icon {
    background-image: url('icon.svg');
    width: 24px;
    height: 24px;
}
```

No DOM access. Good for decorative icons.

### The SVG Viewport and View Box

**Viewport** is the visible area where SVG content is rendered, defined by `width` and `height`:

```svg
<svg width="400" height="300" xmlns="http://www.w3.org/2000/svg">
```

**View box** defines the coordinate system:

```svg
<svg viewBox="0 0 100 100" width="400" height="300">
```

`viewBox="minX minY width height"` — maps the internal coordinate system to the viewport. A `viewBox="0 0 100 100"` with `width="400"` means each unit in SVG space is 4 CSS pixels.

**`preserveAspectRatio`** controls how the viewBox scales:

```svg
<svg viewBox="0 0 100 100" width="400" height="300" preserveAspectRatio="xMidYMid meet">
```

| Value | Behavior |
|---|---|
| `xMidYMid meet` | Scale to fit while maintaining aspect ratio, centered (default) |
| `xMinYMin meet` | Align to top-left, scale to fit |
| `xMaxYMax meet` | Align to bottom-right, scale to fit |
| `xMidYMid slice` | Cover the viewport, may crop |
| `none` | Stretch to fill (distorts aspect ratio) |

### Basic Shapes

SVG provides built-in shape elements:

**Circle:**

```svg
<circle cx="50" cy="50" r="40" fill="red" stroke="black" stroke-width="2"/>
```

**Rectangle:**

```svg
<rect x="10" y="10" width="80" height="50" rx="5" ry="5" fill="blue"/>
```

`rx` and `ry` create rounded corners.

**Ellipse:**

```svg
<ellipse cx="50" cy="50" rx="40" ry="25" fill="green"/>
```

**Line:**

```svg
<line x1="10" y1="10" x2="90" y2="90" stroke="black" stroke-width="2"/>
```

**Polyline (connected lines):**

```svg
<polyline points="10,10 50,50 90,10" fill="none" stroke="red" stroke-width="2"/>
```

**Polygon (closed shape):**

```svg
<polygon points="50,10 90,90 10,90" fill="yellow" stroke="black" stroke-width="2"/>
```

**Path** — the most powerful and flexible SVG element:

```svg
<path d="M10 10 L50 50 L90 10 Z" fill="none" stroke="blue" stroke-width="2"/>
```

Path commands:

| Command | Name | Description |
|---|---|---|
| M x y | Move to | Move to (x, y) without drawing |
| L x y | Line to | Draw a line to (x, y) |
| H x | Horizontal line | Draw a horizontal line to x |
| V y | Vertical line | Draw a vertical line to y |
| A rx ry x-axis-rotation large-arc sweep x y | Arc | Draw an arc |
| Q cx cy x y | Quadratic curve | Quadratic Bezier curve |
| C cx1 cy1 cx2 cy2 x y | Cubic curve | Cubic Bezier curve |
| Z | Close path | Close back to the starting point |

Uppercase commands use absolute coordinates. Lowercase commands use relative coordinates.

```svg
<!-- A complex path: heart shape -->
<path d="M12 21.35l-1.45-1.32C5.4 15.36 2 12.28 2 8.5
         2 5.42 4.42 3 7.5 3c1.74 0 3.41.81 4.5 2.09
         C13.09 3.81 14.76 3 16.5 3 19.58 3 22 5.42 22 8.5
         c0 3.78-3.4 6.86-8.55 11.54L12 21.35z"
      fill="red"/>
```

### Styling SVG

SVG elements can be styled with inline attributes, CSS classes, or presentation attributes:

**Presentation attributes (inline on elements):**

```svg
<circle cx="50" cy="50" r="40" fill="#0056b3" stroke="#003366" stroke-width="2" opacity="0.8"/>
```

Common presentation attributes:

| Attribute | Purpose |
|---|---|
| `fill` | Interior color |
| `stroke` | Outline color |
| `stroke-width` | Outline thickness |
| `opacity` | Element opacity (0-1) |
| `fill-opacity` | Fill opacity |
| `stroke-opacity` | Stroke opacity |
| `stroke-linecap` | End of lines: butt, round, square |
| `stroke-linejoin` | Corners: miter, round, bevel |
| `stroke-dasharray` | Dash pattern |

**CSS classes:**

```html
<style>
    .icon { fill: currentColor; }
    .icon:hover { fill: #0056b3; }
    .highlight { stroke: red; stroke-width: 3; }
</style>

<svg viewBox="0 0 24 24">
    <circle cx="12" cy="12" r="10" class="icon highlight"/>
</svg>
```

CSS is more maintainable for repeated styling. Presentation attributes are useful for one-off values.

**`currentColor`** inherits the element's `color` property, making icons adapt to surrounding text color:

```svg
<svg viewBox="0 0 24 24" fill="currentColor">
    <path d="M12 2L2 7l10 5 10-5-10-5z"/>
</svg>
```

### Text in SVG

```svg
<text x="50" y="50" font-family="Arial" font-size="24" fill="black" text-anchor="middle">
    Hello SVG
</text>
```

Text properties:

| Attribute | Purpose |
|---|---|
| `font-family` | Font family (CSS-like) |
| `font-size` | Font size |
| `font-weight` | Bold, normal, etc. |
| `text-anchor` | Horizontal alignment: start, middle, end |
| `dominant-baseline` | Vertical alignment |
| `letter-spacing` | Character spacing |
| `fill` | Text color |

**Text along a path:**

```svg
<defs>
    <path id="textPath" d="M10 50 Q50 10 90 50"/>
</defs>

<text font-size="14" fill="blue">
    <textPath href="#textPath">This text follows a curved path</textPath>
</text>
```

**`<tspan>`** for multi-line or styled text:

```svg
<text x="10" y="30" font-family="Arial">
    <tspan font-weight="bold">Title:</tspan>
    <tspan x="10" dy="1.2em">This is the first line.</tspan>
    <tspan x="10" dy="1.2em">This is the second line.</tspan>
</text>
```

### Groups and Defs

**`<g>`** groups elements together:

```svg
<g fill="blue" stroke="black" stroke-width="1">
    <circle cx="20" cy="20" r="10"/>
    <circle cx="50" cy="20" r="10"/>
    <circle cx="80" cy="20" r="10"/>
</g>
```

Styles applied to `<g>` are inherited by its children.

**`<defs>`** defines reusable elements (not rendered directly):

```svg
<svg viewBox="0 0 100 100">
    <defs>
        <linearGradient id="grad1" x1="0%" y1="0%" x2="100%" y2="0%">
            <stop offset="0%" stop-color="red"/>
            <stop offset="100%" stop-color="blue"/>
        </linearGradient>

        <radialGradient id="grad2" cx="50%" cy="50%" r="50%">
            <stop offset="0%" stop-color="white"/>
            <stop offset="100%" stop-color="black"/>
        </radialGradient>
    </defs>

    <circle cx="50" cy="50" r="40" fill="url(#grad1)"/>
</svg>
```

**Gradients:**

| Type | Elements | Description |
|---|---|---|
| `linearGradient` | `stop` children | Linear gradient along x1/y1 to x2/y2 |
| `radialGradient` | `stop` children | Radial gradient from center to edge |

`stop` attributes:
- `offset` — position (0% to 100%)
- `stop-color` — color at this stop
- `stop-opacity` — opacity at this stop

### Reusable Graphics with `<use>`

The `<use>` element clones an existing SVG element:

```svg
<svg viewBox="0 0 50 50">
    <defs>
        <circle id="dot" cx="10" cy="10" r="5" fill="red"/>
    </defs>

    <use href="#dot" x="0" y="0"/>
    <use href="#dot" x="15" y="0" fill="blue"/>
    <use href="#dot" x="30" y="0" fill="green"/>
</svg>
```

Attributes on `<use>` override the cloned element's attributes when the cloned element does not explicitly set them.

**Icon sprite system:**

```svg
<!-- icons.svg — sprite sheet -->
<svg xmlns="http://www.w3.org/2000/svg" style="display:none">
    <defs>
        <g id="icon-home">
            <path d="M10 20v-6h4v6h5v-8h3L12 3 2 12h3v8z"/>
        </g>
        <g id="icon-search">
            <path d="M15.5 14h-.79l-.28-.27A6.47 6.47 0 0 0 16 9.5 6.5 6.5 0 1 0 9.5 16c1.61 0 3.09-.59 4.23-1.57l.27.28v.79l5 4.99L20.49 19l-4.99-5zm-6 0C7.01 14 5 11.99 5 9.5S7.01 5 9.5 5 14 7.01 14 9.5 11.99 14 9.5 14z"/>
        </g>
    </defs>
</svg>
```

```html
<!-- Using icons from sprite -->
<svg role="img" aria-label="Home" width="24" height="24">
    <use href="icons.svg#icon-home"/>
</svg>

<svg role="img" aria-label="Search" width="24" height="24">
    <use href="icons.svg#icon-search"/>
</svg>
```

### SVG Filters

SVG filters apply graphical effects like blur, shadow, and color manipulation:

```svg
<svg viewBox="0 0 100 100">
    <defs>
        <filter id="blur">
            <feGaussianBlur stdDeviation="3"/>
        </filter>

        <filter id="shadow">
            <feDropShadow dx="2" dy="2" stdDeviation="3" flood-color="rgba(0,0,0,0.5)"/>
        </filter>

        <filter id="grayscale">
            <feColorMatrix type="saturate" values="0"/>
        </filter>
    </defs>

    <circle cx="30" cy="50" r="20" fill="red" filter="url(#blur)"/>
    <rect x="50" y="30" width="40" height="40" fill="blue" filter="url(#shadow)"/>
    <circle cx="80" cy="50" r="20" fill="green" filter="url(#grayscale)"/>
</svg>
```

Common filters:

| Filter | Effect |
|---|---|
| `feGaussianBlur` | Blur effect |
| `feDropShadow` | Drop shadow |
| `feColorMatrix` | Color manipulation (grayscale, sepia, brightness) |
| `feOffset` | Shift the image |
| `feMerge` | Combine multiple filter outputs |
| `feBlend` | Blend two images |
| `feComposite` | Composite images with masking |

### SVG Animation

**CSS animations** work on inline SVGs:

```html
<style>
    @keyframes pulse {
        0%, 100% { transform: scale(1); opacity: 1; }
        50% { transform: scale(1.2); opacity: 0.7; }
    }

    .animated-circle {
        animation: pulse 2s ease-in-out infinite;
        transform-origin: center;
    }
</style>

<svg viewBox="0 0 100 100">
    <circle cx="50" cy="50" r="30" fill="blue" class="animated-circle"/>
</svg>
```

**SMIL animations** — native SVG animations:

```svg
<circle cx="50" cy="50" r="30" fill="blue">
    <animate attributeName="r" values="30;40;30" dur="2s" repeatCount="indefinite"/>
    <animate attributeName="fill" values="blue;red;blue" dur="4s" repeatCount="indefinite"/>
</circle>
```

SMIL elements:

| Element | Purpose |
|---|---|
| `<animate>` | Animate a single attribute |
| `<animateTransform>` | Animate transform (translate, rotate, scale, skew) |
| `<animateMotion>` | Animate along a path |
| `<set>` | Set a value at a specific time |

```svg
<!-- Moving along a path -->
<svg viewBox="0 0 200 100">
    <path d="M10 50 Q50 10 90 50 T170 50" fill="none" stroke="gray"/>
    <circle r="5" fill="red">
        <animateMotion dur="3s" repeatCount="indefinite">
            <mpath href="#path"/>
        </animateMotion>
    </circle>
</svg>
```

### Interactive SVG

Inline SVGs support all standard DOM events:

```html
<svg viewBox="0 0 100 100" width="200" height="200">
    <!-- Hidden text for screen readers -->
    <text class="sr-only">Click the circle to change its color.</text>
    <circle id="interactive-circle" cx="50" cy="50" r="40" fill="blue"
            role="button" tabindex="0"
            aria-label="Click to change color"
            style="cursor: pointer;"/>
</svg>

<script>
    const circle = document.getElementById('interactive-circle');
    const colors = ['blue', 'red', 'green', 'orange', 'purple'];
    let i = 0;

    function changeColor() {
        circle.setAttribute('fill', colors[i % colors.length]);
        i++;
    }

    circle.addEventListener('click', changeColor);
    circle.addEventListener('keydown', (e) => {
        if (e.key === 'Enter' || e.key === ' ') {
            e.preventDefault();
            changeColor();
        }
    });
</script>
```

### Accessibility in SVG

Inline SVGs are part of the DOM, so they can be made accessible:

**Provide a title:**

```svg
<svg viewBox="0 0 24 24" role="img" aria-labelledby="chartTitle">
    <title id="chartTitle">Sales chart showing 40% growth in Q2</title>
    <path d="M2 20 L10 15 L18 18 L22 10"/>
</svg>
```

**Use `role="img"`** to prevent screen readers from trying to navigate SVG elements individually:

```html
<svg role="img" aria-label="Company logo" viewBox="0 0 100 100">
    <circle cx="50" cy="50" r="40" fill="blue"/>
</svg>
```

**Provide descriptive text for interactive elements:**

```svg
<circle role="button" tabindex="0"
        aria-label="Increase volume" aria-describedby="vol-desc"
        cx="50" cy="50" r="40" fill="gray"/>
<span id="vol-desc" hidden>Click to increase the volume level by 10%.</span>
```

**Decorative SVGs** should be hidden from screen readers:

```html
<svg aria-hidden="true" focusable="false">
    <path d="M12 2L2 7l10 5 10-5-10-5z"/>
</svg>
```

**`focusable="false"`** prevents SVG elements from receiving focus in Internet Explorer.

### SVG Best Practices

**Optimize SVGs before deployment.** Tools like SVGO remove unnecessary metadata, unused defs, and redundant attributes:

```bash
npx svgo icon.svg -o icon.min.svg
```

**Use `viewBox` instead of fixed dimensions** for responsive SVGs:

```svg
<svg viewBox="0 0 24 24" width="100%" height="100%">
```

**Set `fill="currentColor"` on icons** so they inherit the text color:

```svg
<svg viewBox="0 0 24 24" fill="currentColor">
```

**Cache sprite sheets** in the browser by using external SVG files and `<use>`.

**Avoid inline SVGs in HTML for large files** — they bloat the HTML size and delay parsing. Use `<img>` or `<object>` for files over 5KB.

**Remove empty groups and redundant attributes** to keep file size small.

**Use semantic elements** (`<circle>`, `<path>`, `<text>`) instead of generic `<g>` with paths where possible.

**Set explicit `width` and `height` on `<img>` tags** loading SVGs to prevent layout shifts:

```html
<img src="logo.svg" alt="Logo" width="200" height="100">
```

### SVG vs Canvas

(See also the comparison table in Canvas Graphics.)

| Aspect | SVG | Canvas |
|---|---|---|
| Model | Vector (shapes) | Bitmap (pixels) |
| Scaling | Infinite, any resolution | Fixed, pixelates on zoom |
| DOM | Full DOM access | Single element |
| Styling | CSS and presentation attributes | JavaScript only |
| Events | Built-in per element | Manual hit detection |
| Animation | CSS, SMIL, JS | Frame-based |
| Accessibility | ARIA on elements | Manual only |
| Performance | Slower with many elements | Faster with many elements |
| Best for | Icons, logos, diagrams, charts | Games, image processing, real-time graphics |

### Common Mistakes

**Missing `viewBox`.** Without `viewBox`, SVGs do not scale properly. Always include it:

```svg
<svg width="100" height="100">
```

This renders at 100x100 but does not scale the coordinate system. Add `viewBox="0 0 100 100"`.

**Using fixed pixel values for responsive SVGs.** Set `width="100%"` and use `viewBox` to scale.

**Not handling focus for interactive elements.** Interactive SVGs (buttons, clickable regions) need `tabindex="0"`, `role`, and keyboard event handlers.

**Missing `role="img"` on icon SVGs.** Without it, screen readers may try to navigate individual SVG elements.

**Inline SVGs that are too large.** A 50KB inline SVG delays HTML parsing. Use `<object>` or `<img>` for large SVGs.

**Forgetting `<title>` in complex infographics.** Screen reader users need a text alternative.

## Study Cases

**Case 1: The icon that does not inherit text color.**

A developer uses inline SVGs for icons with hardcoded `fill` attributes:

```svg
<svg viewBox="0 0 24 24"><path d="..." fill="#333"/></svg>
```

When the parent text color changes (e.g., in a dark mode), the icon stays dark and becomes invisible.

The fix: use `currentColor`:

```svg
<svg viewBox="0 0 24 24" fill="currentColor"><path d="..."/></svg>
```

**Case 2: The SVG that does not scale in an `<img>` tag.**

A developer creates an SVG with fixed width and height but no viewBox:

```svg
<svg xmlns="http://www.w3.org/2000/svg" width="200" height="200">
    <circle cx="100" cy="100" r="80" fill="blue"/>
</svg>
```

When used in an `<img>` tag with `width="400"`, the SVG scales visually but the coordinate system breaks.

The fix:

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 200 200">
    <circle cx="100" cy="100" r="80" fill="blue"/>
</svg>
```

**Case 3: The infographic invisible to screen readers.**

A developer creates a complex data visualization as an inline SVG without any accessible annotations. Screen reader users hear nothing.

The fix:

```svg
<svg role="img" aria-labelledby="chart-title chart-desc" viewBox="0 0 500 300">
    <title id="chart-title">Quarterly Sales 2026</title>
    <desc id="chart-desc">Bar chart showing Q1: $10K, Q2: $15K, Q3: $12K, Q4: $18K.</desc>
    <!-- chart elements -->
</svg>
```

## Examples

**Example 1: Responsive logo.**

```svg
<svg viewBox="0 0 200 50" xmlns="http://www.w3.org/2000/svg"
     role="img" aria-label="DevBook logo">
    <rect width="50" height="50" rx="8" fill="#0056b3"/>
    <text x="62" y="35" font-family="Arial, sans-serif" font-size="28"
          font-weight="bold" fill="currentColor">DevBook</text>
</svg>
```

**Example 2: Loading spinner.**

```html
<svg viewBox="0 0 50 50" width="40" height="40" role="img" aria-label="Loading">
    <style>
        @keyframes spin { 100% { transform: rotate(360deg); } }
        @keyframes dash {
            0% { stroke-dasharray: 1, 150; stroke-dashoffset: 0; }
            50% { stroke-dasharray: 90, 150; stroke-dashoffset: -35; }
            100% { stroke-dasharray: 90, 150; stroke-dashoffset: -124; }
        }
        .spinner {
            animation: spin 2s linear infinite;
            transform-origin: center;
        }
        .spinner-path {
            stroke: #0056b3;
            stroke-linecap: round;
            animation: dash 1.5s ease-in-out infinite;
        }
    </style>
    <circle class="spinner" cx="25" cy="25" r="20" fill="none">
        <path class="spinner-path" d="M25 5 A20 20 0 0 1 45 25" fill="none" stroke-width="4"/>
    </circle>
</svg>
```

**Example 3: Interactive star rating.**

```html
<div id="star-rating" style="display: flex; gap: 4px;">
    <svg viewBox="0 0 24 24" width="32" height="32" data-star="1"
         role="button" tabindex="0" aria-label="Rate 1 star" style="cursor: pointer;">
        <path d="M12 17.27L18.18 21l-1.64-7.03L22 9.24l-7.19-.61L12 2 9.19 8.63 2 9.24l5.46 4.73L5.82 21z" fill="lightgray"/>
    </svg>
    <!-- repeat for 2-5 stars -->
</div>

<script>
    document.querySelectorAll('#star-rating svg').forEach(svg => {
        svg.addEventListener('click', function() {
            const rating = this.dataset.star;
            document.querySelectorAll('#star-rating svg').forEach((s, i) => {
                s.querySelector('path').setAttribute('fill', i < rating ? 'gold' : 'lightgray');
            });
        });
        svg.addEventListener('keydown', (e) => {
            if (e.key === 'Enter' || e.key === ' ') { e.preventDefault(); svg.click(); }
        });
    });
</script>
```

**Example 4: Responsive bar chart (inline SVG).**

```svg
<svg viewBox="0 0 400 250" role="img"
     aria-label="Bar chart: Product A 120, Product B 85, Product C 200, Product D 150">
    <style>
        .bar { fill: #0056b3; transition: fill 0.3s; }
        .bar:hover { fill: #003d80; }
        .label { font-family: Arial; font-size: 12px; fill: #333; text-anchor: middle; }
    </style>

    <rect class="bar" x="40" y="130" width="60" height="80" rx="4"/>
    <text class="label" x="70" y="225">Product A</text>

    <rect class="bar" x="130" y="80" width="60" height="130" rx="4"/>
    <text class="label" x="160" y="225">Product B</text>

    <rect class="bar" x="220" y="0" width="60" height="210" rx="4"/>
    <text class="label" x="250" y="225">Product C</text>

    <rect class="bar" x="310" y="55" width="60" height="155" rx="4"/>
    <text class="label" x="340" y="225">Product D</text>

    <text x="200" y="245" font-family="Arial" font-size="14"
          fill="#666" text-anchor="middle">Product Sales</text>
</svg>
```

## Glossary

| Term | Definition |
|------|------------|
| SVG | Scalable Vector Graphics — XML-based vector image format |
| ViewBox | Defines the internal coordinate system of an SVG |
| Viewport | The visible area where SVG content renders |
| `preserveAspectRatio` | Controls how the viewBox scales to fit the viewport |
| Path | The most powerful SVG shape element, defined by `d` attribute |
| `<g>` | Group element for organizing and styling SVG children |
| `<defs>` | Container for reusable definitions (not rendered directly) |
| `<use>` | Clones an existing SVG element |
| `fill` | Interior color of an SVG element |
| `stroke` | Outline color of an SVG element |
| `currentColor` | CSS keyword that inherits the parent's color value |
| SMIL | Synchronized Multimedia Integration Language — native SVG animation |
| `<tspan>` | Multi-line or styled text within `<text>` |
| `<textPath>` | Text that follows a path |
| Filter | Graphical effect (blur, shadow, color matrix) |
| `feGaussianBlur` | SVG filter element for blur |
| `feDropShadow` | SVG filter element for drop shadow |
| Sprite | A collection of SVG icons in one file, referenced by ID |
| SVGO | Node.js tool for optimizing SVG files |

## Quick References

- [MDN: SVG Guide](https://developer.mozilla.org/en-US/docs/Web/SVG) — complete reference
- [MDN: SVG Elements](https://developer.mozilla.org/en-US/docs/Web/SVG/Element) — element reference
- [MDN: SVG Attributes](https://developer.mozilla.org/en-US/docs/Web/SVG/Attribute) — attribute reference
- [MDN: SVG Filters](https://developer.mozilla.org/en-US/docs/Web/SVG/Element/filter) — filter reference
- [MDN: SVG Animation](https://developer.mozilla.org/en-US/docs/Web/SVG/SVG_animation_with_SMIL) — SMIL guide
- [HTML Spec: Embedded Content](https://html.spec.whatwg.org/multipage/embedded-content.html) — official specification
- [SVG 2 Specification](https://www.w3.org/TR/SVG2/) — W3C specification
- [SVGO](https://github.com/svg/svgo) — SVG optimization tool
- [SVG Icon Sprite Generator](https://svgsprit.es/) — create sprite sheets

## Next Steps

- [IFrames & Embeds](iframes-and-embeds.md) — embedding external content
- [Performance & SEO Basics](performance-seo.md) — optimizing page performance
- [Meta Tags & Social Sharing](meta-tags.md) — metadata for SEO and social media
