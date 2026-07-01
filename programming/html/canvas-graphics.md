# Canvas Graphics

## Description

The `<canvas>` element provides a bitmap drawing surface controlled entirely by JavaScript. It is ideal for real-time graphics, game rendering, data visualization, image processing, and generative art.

## Prerequisites

- [Elements, Tags & Attributes](elements-and-attributes.md) — fundamental HTML building blocks
- [Script Loading & Integration](scripts-and-defer.md) — running JavaScript

## Table of Contents

- [The `<canvas>` Element](#the-canvas-element)
- [The 2D Drawing Context](#the-2d-drawing-context)
- [Drawing Shapes](#drawing-shapes)
- [Colors and Styles](#colors-and-styles)
- [Text](#text)
- [Images](#images)
- [Transformations](#transformations)
- [Animation](#animation)
- [Pixel Manipulation](#pixel-manipulation)
- [Canvas vs SVG](#canvas-vs-svg)
- [Performance Considerations](#performance-considerations)
- [Accessibility](#accessibility)
- [Common Mistakes](#common-mistakes)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The `<canvas>` Element

The `<canvas>` element defines a rectangular drawing area:

```html
<canvas id="myCanvas" width="400" height="300">
    Your browser does not support the canvas element.
</canvas>
```

- `width` and `height` — dimensions of the drawing surface in pixels
- Content between the tags is fallback text for non-supporting browsers

The canvas starts blank. All drawing is performed via JavaScript.

### The 2D Drawing Context

The rendering context is the API for drawing:

```javascript
const canvas = document.getElementById('myCanvas');
const ctx = canvas.getContext('2d');
```

`getContext('2d')` returns a 2D rendering context. Other contexts include `'webgl'` (3D) and `'webgl2'`.

The coordinate system starts at (0, 0) in the top-left corner. The x-axis increases to the right. The y-axis increases downward.

### Drawing Shapes

Canvas uses a path-based drawing model — define a path, then stroke or fill it.

**Rectangles:**

```javascript
// Filled rectangle
ctx.fillStyle = 'blue';
ctx.fillRect(10, 10, 100, 50);   // (x, y, width, height)

// Outlined rectangle
ctx.strokeStyle = 'red';
ctx.strokeRect(130, 10, 100, 50);

// Clear a rectangular area (transparent)
ctx.clearRect(20, 20, 80, 30);   // Erase part of the filled rectangle
```

**Paths:**

```javascript
ctx.beginPath();
ctx.moveTo(50, 50);       // Move to starting point
ctx.lineTo(150, 50);      // Draw a line
ctx.lineTo(100, 150);     // Draw another line
ctx.closePath();          // Close back to start
ctx.fillStyle = 'green';
ctx.fill();               // Fill the triangle
ctx.strokeStyle = 'black';
ctx.stroke();             // Stroke the outline
```

**Circles and arcs:**

```javascript
ctx.beginPath();
ctx.arc(100, 100, 50, 0, Math.PI * 2);   // (x, y, radius, startAngle, endAngle)
ctx.fillStyle = 'orange';
ctx.fill();
ctx.strokeStyle = 'black';
ctx.stroke();
```

Angles are measured in radians. `0` is the rightmost point. The arc goes clockwise.

**Quadratic and cubic curves:**

```javascript
// Quadratic curve (one control point)
ctx.beginPath();
ctx.moveTo(20, 100);
ctx.quadraticCurveTo(80, 20, 140, 100);
ctx.stroke();

// Cubic Bezier curve (two control points)
ctx.beginPath();
ctx.moveTo(20, 150);
ctx.bezierCurveTo(60, 30, 120, 30, 180, 150);
ctx.stroke();
```

### Colors and Styles

**Fill and stroke colors:**

```javascript
ctx.fillStyle = 'red';                            // Named color
ctx.fillStyle = '#ff0000';                        // Hex
ctx.fillStyle = 'rgb(255, 0, 0)';                 // RGB
ctx.fillStyle = 'rgba(255, 0, 0, 0.5)';           // RGBA with alpha
ctx.fillStyle = 'hsl(0, 100%, 50%)';              // HSL
ctx.fillStyle = 'hsla(0, 100%, 50%, 0.5)';        // HSLA with alpha

ctx.strokeStyle = 'blue';                         // Same options for strokes
```

**Line styles:**

```javascript
ctx.lineWidth = 5;                // Thickness in pixels
ctx.lineCap = 'round';           // 'butt' (default), 'round', 'square'
ctx.lineJoin = 'round';          // 'miter' (default), 'round', 'bevel'
ctx.setLineDash([10, 5]);        // Dash pattern: 10px dash, 5px gap
ctx.lineDashOffset = 2;          // Where the dash pattern starts
```

**Gradients:**

```javascript
// Linear gradient
const gradient = ctx.createLinearGradient(0, 0, 200, 0);
gradient.addColorStop(0, 'red');
gradient.addColorStop(0.5, 'yellow');
gradient.addColorStop(1, 'green');
ctx.fillStyle = gradient;
ctx.fillRect(0, 0, 200, 100);

// Radial gradient
const radialGradient = ctx.createRadialGradient(100, 100, 10, 100, 100, 50);
radialGradient.addColorStop(0, 'white');
radialGradient.addColorStop(1, 'blue');
ctx.fillStyle = radialGradient;
ctx.beginPath();
ctx.arc(100, 100, 50, 0, Math.PI * 2);
ctx.fill();
```

**Patterns:**

```javascript
const img = new Image();
img.src = 'pattern.png';
img.onload = function() {
    const pattern = ctx.createPattern(img, 'repeat');
    ctx.fillStyle = pattern;
    ctx.fillRect(0, 0, canvas.width, canvas.height);
};
```

The second argument controls repetition: `'repeat'`, `'repeat-x'`, `'repeat-y'`, or `'no-repeat'`.

**Shadows:**

```javascript
ctx.shadowColor = 'rgba(0, 0, 0, 0.5)';
ctx.shadowBlur = 10;
ctx.shadowOffsetX = 5;
ctx.shadowOffsetY = 5;

ctx.fillStyle = 'red';
ctx.fillRect(50, 50, 100, 100);
```

### Text

```javascript
ctx.font = '24px Arial, sans-serif';
ctx.fillStyle = 'black';
ctx.fillText('Hello Canvas', 50, 100);          // Filled text
ctx.strokeText('Hello Canvas', 50, 150);        // Outlined text
```

**Text properties:**

```javascript
ctx.font = 'bold italic 24px "Helvetica Neue", Arial, sans-serif';
ctx.textAlign = 'center';           // 'start', 'end', 'left', 'right', 'center'
ctx.textBaseline = 'middle';        // 'top', 'hanging', 'middle', 'alphabetic', 'ideographic', 'bottom'
ctx.direction = 'ltr';              // 'ltr', 'rtl', 'inherit'
```

**Measuring text:**

```javascript
const metrics = ctx.measureText('Hello Canvas');
console.log(metrics.width);         // Width in pixels
console.log(metrics.actualBoundingBoxAscent);
console.log(metrics.actualBoundingBoxDescent);
```

### Images

```javascript
const img = new Image();
img.src = 'photo.jpg';
img.onload = function() {
    // Draw image at (x, y)
    ctx.drawImage(img, 0, 0);

    // Draw image scaled to fit (width, height)
    ctx.drawImage(img, 0, 0, 200, 150);

    // Draw a portion of the image:
    // source (sx, sy, sWidth, sHeight) → destination (dx, dy, dWidth, dHeight)
    ctx.drawImage(img, 50, 50, 100, 100, 0, 0, 200, 200);
};
```

**Cross-origin images:**

```javascript
const img = new Image();
img.crossOrigin = 'anonymous';      // Required for CORS-enabled images
img.src = 'https://example.com/photo.jpg';
```

Cross-origin images taint the canvas. Once tainted, `toDataURL()` and `getImageData()` throw security errors.

**Exporting canvas to image:**

```javascript
const dataUrl = canvas.toDataURL('image/png');    // PNG data URL
const jpegUrl = canvas.toDataURL('image/jpeg', 0.8); // JPEG at 80% quality

// Create a downloadable link
const link = document.createElement('a');
link.download = 'drawing.png';
link.href = dataUrl;
link.click();
```

### Transformations

Transformations affect all subsequent drawing operations:

```javascript
// Save the current state
ctx.save();

// Translate origin to (100, 100)
ctx.translate(100, 100);

// Rotate by 45 degrees (radians)
ctx.rotate(Math.PI / 4);

// Scale by 2x horizontally, 0.5x vertically
ctx.scale(2, 0.5);

// Draw — these operations are in the transformed coordinate system
ctx.fillStyle = 'red';
ctx.fillRect(-25, -25, 50, 50);

// Restore to the previously saved state
ctx.restore();
```

**The transformation matrix:**

```javascript
// Direct matrix manipulation
ctx.transform(a, b, c, d, e, f);   // Apply additional transform
ctx.setTransform(a, b, c, d, e, f); // Reset and set transform
ctx.resetTransform();               // Reset to identity matrix
```

The matrix: `a c e` / `b d f` / `0 0 1`

- `a` — scale x
- `b` — skew y
- `c` — skew x
- `d` — scale y
- `e` — translate x
- `f` — translate y

### Animation

Canvas animation uses `requestAnimationFrame` for smooth, efficient rendering:

```javascript
let x = 0;
let y = 150;
let dx = 2;

function draw() {
    // Clear the entire canvas
    ctx.clearRect(0, 0, canvas.width, canvas.height);

    // Update state
    x += dx;
    if (x > canvas.width - 25 || x < 0) {
        dx = -dx;
    }

    // Draw
    ctx.fillStyle = 'red';
    ctx.fillRect(x, y, 50, 50);

    // Request next frame
    requestAnimationFrame(draw);
}

draw();
```

**requestAnimationFrame** is superior to `setInterval` for animation because it:
- Syncs with the display refresh rate (typically 60fps)
- Pauses when the tab is not visible (saves battery/CPU)
- Provides a timestamp for delta-time calculations

**Delta-time animation:**

```javascript
let lastTime = 0;
let xPos = 0;
const speed = 200; // pixels per second

function animate(timestamp) {
    const deltaTime = (timestamp - lastTime) / 1000; // seconds
    lastTime = timestamp;

    ctx.clearRect(0, 0, canvas.width, canvas.height);

    xPos += speed * deltaTime;
    if (xPos > canvas.width) xPos = 0;

    ctx.fillStyle = 'blue';
    ctx.fillRect(xPos, 100, 30, 30);

    requestAnimationFrame(animate);
}

requestAnimationFrame(animate);
```

Delta-time ensures consistent animation speed regardless of frame rate.

**Controlling animation:**

```javascript
let animationId;

function start() {
    function loop() {
        update();
        render();
        animationId = requestAnimationFrame(loop);
    }
    loop();
}

function stop() {
    cancelAnimationFrame(animationId);
}
```

### Pixel Manipulation

Canvas provides direct read/write access to pixel data:

```javascript
// Read pixel data from a region
const imageData = ctx.getImageData(0, 0, canvas.width, canvas.height);
const pixels = imageData.data;  // Uint8ClampedArray — RGBA per pixel

// Modify pixels (invert colors)
for (let i = 0; i < pixels.length; i += 4) {
    pixels[i]     = 255 - pixels[i];       // Red
    pixels[i + 1] = 255 - pixels[i + 1];   // Green
    pixels[i + 2] = 255 - pixels[i + 2];   // Blue
    // pixels[i + 3] — alpha (unchanged)
}

// Write modified pixels back
ctx.putImageData(imageData, 0, 0);
```

**Creating pixel data:**

```javascript
const imageData = ctx.createImageData(width, height);
// Fill with red pixels
for (let i = 0; i < imageData.data.length; i += 4) {
    imageData.data[i]     = 255;    // Red
    imageData.data[i + 1] = 0;      // Green
    imageData.data[i + 2] = 0;      // Blue
    imageData.data[i + 3] = 255;    // Alpha
}
ctx.putImageData(imageData, 0, 0);
```

**Note:** `getImageData` does not work on tainted canvases (those that have drawn cross-origin images without CORS headers).

### Canvas vs SVG

| Aspect | Canvas | SVG |
|---|---|---|
| Rendering model | Pixel-based (bitmap) | Vector-based (shapes) |
| Resolution | Fixed — pixelates at higher zoom | Infinite — scales to any size |
| Performance | Fast for many elements | Slower with many elements |
| Interactivity | Manual hit detection | Built-in event handling |
| Accessibility | No built-in accessibility | Elements can have ARIA |
| Animation | Frame-based (redraw each frame) | CSS or SMIL animation |
| Use case | Games, image processing, charts, real-time graphics | Icons, logos, illustrations, interactive diagrams |
| API | JavaScript only | HTML + CSS + JavaScript |
| Export | `toDataURL()` / `toBlob()` | Inline in HTML (no export needed) |

Choose canvas when you need:
- Real-time rendering (games, video processing)
- Many elements (thousands of particles)
- Pixel-level manipulation
- Continuous animation at high frame rates

Choose SVG when you need:
- Resolution-independent graphics (responsive design)
- Interactive individual elements
- Accessibility (text and ARIA on elements)
- CSS styling and animation

### Performance Considerations

**Minimize state changes.** Group drawing operations by style:

```javascript
// Slow: changing fillStyle for every shape
ctx.fillStyle = 'red';   ctx.fillRect(0, 0, 10, 10);
ctx.fillStyle = 'blue';  ctx.fillRect(20, 0, 10, 10);
ctx.fillStyle = 'green'; ctx.fillRect(40, 0, 10, 10);

// Fast: batch by style
ctx.fillStyle = 'red';
ctx.fillRect(0, 0, 10, 10);
ctx.fillRect(0, 20, 10, 10);
ctx.fillStyle = 'blue';
ctx.fillRect(20, 0, 10, 10);
ctx.fillRect(20, 20, 10, 10);
```

**Use `save()` and `restore()` sparingly.** Each call saves the full state stack.

**Use off-screen canvases for pre-rendering:**

```javascript
const offscreen = document.createElement('canvas');
offscreen.width = 200;
offscreen.height = 200;
const offCtx = offscreen.getContext('2d');
// Pre-render complex shapes once
offCtx.fillStyle = 'complexGradient';
offCtx.fillRect(0, 0, 200, 200);

// In the main render loop, just copy the off-screen canvas
ctx.drawImage(offscreen, 0, 0);
```

**Limit `getImageData` calls.** Reading pixel data is expensive. Cache it when possible.

**Use integer coordinates.** Sub-pixel coordinates trigger anti-aliasing and are slower to render.

**Reduce canvas size.** A 1920x1080 canvas has 8 million pixels. Use the minimum canvas size needed for the visual quality.

### Accessibility

Canvas is inherently inaccessible — its content is not part of the DOM and is invisible to screen readers.

**Provide a text fallback:**

```html
<canvas id="chart" width="400" height="300">
    <p>Sales chart: Q1 $10K, Q2 $15K, Q3 $12K, Q4 $18K.</p>
</canvas>
```

**Use ARIA roles:**

```html
<canvas id="game" width="800" height="600"
        role="img" aria-label="Space invaders game. Score: 1500. Level: 3.">
</canvas>
```

**Provide keyboard controls for interactive canvas elements.** Canvas does not receive focus by default — add `tabindex` and handle keyboard events:

```html
<canvas id="drawing" width="400" height="300" tabindex="0"
        role="application" aria-label="Drawing application">
</canvas>
```

```javascript
canvas.addEventListener('keydown', (event) => {
    if (event.key === 'ArrowRight') moveSelection(1, 0);
    if (event.key === 'ArrowLeft') moveSelection(-1, 0);
});
```

**Announce changes:**

```javascript
const liveRegion = document.getElementById('canvas-announcements');

function announce(message) {
    liveRegion.textContent = message;
}
```

```html
<div id="canvas-announcements" aria-live="polite"></div>
```

### Common Mistakes

**Forgetting to clear the canvas between frames.** Without `clearRect`, previous frames remain visible during animation.

**Using `setInterval` for animation instead of `requestAnimationFrame`.** `requestAnimationFrame` is more efficient, syncs with display refresh, and pauses in background tabs.

**Drawing images before they load.** Always use the `load` event before drawing images.

**Not handling `devicePixelRatio` for sharp rendering on Retina displays:**

```javascript
const dpr = window.devicePixelRatio || 1;
canvas.width = displayWidth * dpr;
canvas.height = displayHeight * dpr;
canvas.style.width = displayWidth + 'px';
canvas.style.height = displayHeight + 'px';
ctx.scale(dpr, dpr);
```

**Cross-origin images without CORS.** Drawing an image from another origin without CORS headers taints the canvas and prevents `toDataURL()`.

**Creating large canvases.** A canvas that is too large for the GPU will fall back to software rendering, which is much slower.

## Glossary

| Term | Definition |
|------|------------|
| `<canvas>` | HTML element providing a bitmap drawing surface |
| Context | The API object returned by `getContext()` (e.g., '2d', 'webgl') |
| Path | A sequence of drawing operations defining a shape |
| `beginPath()` | Starts a new path |
| `fill()` | Fills the current path with the active fillStyle |
| `stroke()` | Strokes the current path with the active strokeStyle |
| `requestAnimationFrame` | Browser API for efficient frame-based animation |
| `devicePixelRatio` | Ratio of physical pixels to CSS pixels on Retina displays |
| `getImageData` | Reads raw pixel data from the canvas |
| `putImageData` | Writes raw pixel data to the canvas |
| `toDataURL` | Exports canvas content as an image data URL |
| `save()` / `restore()` | Saves and restores the canvas state stack |
| WebGL | 3D rendering context for canvas |
| Tainted canvas | Canvas that has drawn cross-origin images without CORS |
| Off-screen canvas | A canvas not attached to the DOM, used for pre-rendering |
| `createLinearGradient` | Creates a linear gradient fill |
| `createRadialGradient` | Creates a radial gradient fill |

## Quick References

- [MDN: Canvas API](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API) — complete reference
- [MDN: Canvas Tutorial](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial) — step-by-step guide
- [MDN: CanvasRenderingContext2D](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D) — 2D context reference
- [HTML Spec: Canvas](https://html.spec.whatwg.org/multipage/canvas.html) — official specification
- [HTML5 Canvas Cheat Sheet](https://simon.html5.org/dump/html5-canvas-cheat-sheet.html) — quick reference
- [MDN: WebGL](https://developer.mozilla.org/en-US/docs/Web/API/WebGL_API) — 3D canvas reference

## Next Steps

- [SVG in HTML](svg-and-html.md) — scalable vector graphics
- [IFrames & Embeds](iframes-and-embeds.md) — embedding external content
- [Performance & SEO Basics](performance-seo.md) — optimizing page performance
