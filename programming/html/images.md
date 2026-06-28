# Images & Responsive Images

## Description

Images bring the web to life, but they also account for over 50% of a typical page's bandwidth. HTML provides elements and attributes that let developers serve the right image at the right size for every device, preserving visual quality while minimizing load time.

## Prerequisites

- [Elements, Tags & Attributes](elements-and-attributes.md) — fundamental HTML building blocks
- [Links & Navigation](links.md) — how links work

## Table of Contents

- [The `<img>` Element](#the-img-element)
- [The `alt` Attribute](#the-alt-attribute)
- [Image Formats](#image-formats)
- [Image Dimensions](#image-dimensions)
- [Responsive Images with `srcset`](#responsive-images-with-srcset)
- [The `<picture>` Element](#the-picture-element)
- [Art Direction with `<picture>`](#art-direction-with-picture)
- [Lazy Loading](#lazy-loading)
- [Figure and Figcaption](#figure-and-figcaption)
- [Image Maps](#image-maps)
- [Favicons](#favicons)
- [Performance Considerations](#performance-considerations)
- [Common Mistakes](#common-mistakes)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The `<img>` Element

The `<img>` element embeds an image into the page. It is a void element (no closing tag) and a replaced element (its content comes from an external file).

```html
<img src="photo.jpg" alt="A landscape photograph">
```

Required attributes:

- **`src`** — the image URL (absolute or relative)
- **`alt`** — alternative text describing the image

The `<img>` element is inline by default but behaves like an inline-block element — it respects `width` and `height`. Images that are not wrapped in a block-level container will flow with surrounding text.

### The `alt` Attribute

The `alt` attribute provides a text alternative for the image. It is displayed when the image fails to load and read aloud by screen readers.

**Informative images** need descriptive alt text:

```html
<img src="chart.png" alt="Bar chart showing quarterly revenue: Q1 $1M, Q2 $1.5M, Q3 $2M, Q4 $2.5M">
```

**Decorative images** should have empty alt text:

```html
<img src="divider-line.png" alt="">
```

Empty `alt=""` tells screen readers to ignore the image entirely. Do not omit `alt` — if you do, some screen readers announce the filename instead.

**Functional images** (images inside links or buttons) need alt text describing the action:

```html
<a href="/products">
    <img src="products-icon.png" alt="View our products">
</a>
```

**Images with captions** may have shorter alt text if the caption provides full context:

```html
<figure>
    <img src="team-photo.jpg" alt="The DevBook team">
    <figcaption>The DevBook team at the 2026 annual conference, standing in front of the office building.</figcaption>
</figure>
```

Guidelines for writing alt text:

- Be concise but descriptive. Aim for 5-15 words for most images
- Do not start with "Image of" or "Picture of" — screen readers announce it as an image
- Include relevant information that is visible in the image
- For complex images (charts, diagrams), summarize the key takeaway and link to a longer description
- For text in images, include the text in the alt attribute (but avoid putting text in images when possible)

### Image Formats

Choosing the right image format balances quality and file size:

| Format | Use case | Compression | Transparency | Animation |
|---|---|---|---|---|
| JPEG | Photographs, complex scenes | Lossy | No | No |
| PNG | Graphics, screenshots, transparency | Lossless | Yes | No |
| GIF | Simple animations, low-color graphics | Lossless (limited) | Yes (binary) | Yes |
| WebP | Modern replacement for JPEG and PNG | Lossy and lossless | Yes | Yes |
| AVIF | Next-gen compression | Lossy and lossless | Yes | Yes |
| SVG | Icons, logos, illustrations, diagrams | Vector (scales infinitely) | Yes | Yes (via CSS/JS) |

**JPEG** — best for photographs and images with many colors. Quality can be tuned (0-100) to balance size and quality. Does not support transparency.

**PNG** — best for screenshots, graphics with text, and images requiring transparency. File sizes are larger than JPEG for photographs. Supports 256 levels of transparency (alpha channel).

**WebP** — developed by Google, provides 25-35% smaller files than JPEG at equivalent quality. Supports both lossy and lossless compression, transparency, and animation. Supported in all modern browsers.

**AVIF** — based on the AV1 video codec. Provides 50% smaller files than JPEG at equivalent quality. Supported in Chrome, Firefox, and Opera. Not yet supported in Safari (as of 2026).

**SVG** — vector format using XML. Scales to any size without quality loss. Ideal for icons, logos, and illustrations. Can be styled and animated with CSS and JavaScript.

```html
<img src="logo.svg" alt="Company logo" width="200" height="100">
```

SVG can also be inlined directly in HTML for styling and animation:

```html
<svg viewBox="0 0 100 100" xmlns="http://www.w3.org/2000/svg">
    <circle cx="50" cy="50" r="40" fill="blue" />
</svg>
```

The `<img>` tag approach is simpler for static SVGs, but inlining gives you full control over SVG elements with CSS.

### Image Dimensions

Always specify image dimensions (`width` and `height`) to prevent layout shifts:

```html
<img src="photo.jpg" alt="Description" width="800" height="600">
```

Without dimensions, the browser does not know the image size until it downloads and decodes it. When the image loads, it pushes content down, causing a layout shift — this is a Core Web Vital called Cumulative Layout Shift (CLS).

Dimensions can be set in HTML attributes (pixels) or CSS:

```html
<!-- HTML attributes (browser uses these to reserve space) -->
<img src="photo.jpg" alt="Description" width="800" height="600">

<!-- CSS can override display size while preserving aspect ratio -->
<img src="photo.jpg" alt="Description" style="width: 100%; height: auto;">
```

When using CSS to make images responsive, set `max-width: 100%` to prevent overflow:

```css
img {
    max-width: 100%;
    height: auto;
}
```

This ensures images scale down on small screens without exceeding their container.

### Responsive Images with `srcset`

The `srcset` attribute lets the browser choose the best image size based on the device's viewport width and pixel density.

**Using `w` descriptors (width-based):**

```html
<img
    src="photo-800.jpg"
    srcset="photo-400.jpg 400w, photo-800.jpg 800w, photo-1200.jpg 1200w"
    sizes="(max-width: 600px) 100vw, (max-width: 1200px) 50vw, 800px"
    alt="Landscape photograph"
>
```

- `srcset` lists image files with their actual pixel widths (the `w` unit)
- `sizes` tells the browser how large the image will display at different viewport widths
- The browser chooses which image to download based on the device's resolution and viewport

The `sizes` attribute uses media queries:

```
sizes="
    (max-width: 600px) 100vw,   ← Full viewport width on small screens
    (max-width: 1200px) 50vw,   ← Half viewport width on medium screens
    800px                        ← 800px on large screens
"
```

**Using `x` descriptors (density-based):**

```html
<img
    src="photo-1x.jpg"
    srcset="photo-1x.jpg 1x, photo-2x.jpg 2x, photo-3x.jpg 3x"
    alt="Landscape photograph"
>
```

- `1x` = standard resolution display
- `2x` = Retina display (iPhone, high-resolution laptop)
- `3x` = Ultra-high resolution (some Android and newer displays)

Use `w` descriptors for responsive layouts and `x` descriptors for fixed-width images.

### The `<picture>` Element

The `<picture>` element provides more control than `srcset`. It lets you serve different image formats and apply art direction (different crops or compositions for different screen sizes).

```html
<picture>
    <source srcset="photo.avif" type="image/avif">
    <source srcset="photo.webp" type="image/webp">
    <img src="photo.jpg" alt="Landscape photograph">
</picture>
```

The browser picks the first `<source>` with a supported format. If AVIF is supported, it downloads the smallest file. Otherwise, it falls back to WebP. If neither is supported, it uses the `<img>` fallback.

Always include the `<img>` element as the last child — it provides the fallback and is required for the `<picture>` element to work.

Each `<source>` can have its own `srcset`, `sizes`, `media`, and `type` attributes:

```html
<picture>
    <source srcset="photo-wide.avif" type="image/avif" media="(min-width: 800px)">
    <source srcset="photo-wide.webp" type="image/webp" media="(min-width: 800px)">
    <source srcset="photo-square.avif" type="image/avif">
    <source srcset="photo-square.webp" type="image/webp">
    <img src="photo-square.jpg" alt="Landscape photograph">
</picture>
```

This serves a wide crop on large screens and a square crop on small screens, in the most efficient format available.

### Art Direction with `<picture>`

Art direction means showing different image compositions for different screen sizes. A photo that looks great on desktop may have important subjects too small on mobile.

```html
<picture>
    <source
        srcset="hero-desktop.jpg"
        media="(min-width: 1024px)"
        width="1200"
        height="600"
    >
    <source
        srcset="hero-tablet.jpg"
        media="(min-width: 600px)"
        width="800"
        height="600"
    >
    <img
        src="hero-mobile.jpg"
        alt="Team working on a project"
        width="400"
        height="600"
    >
</picture>
```

Art direction is different from resolution switching. Resolution switching (with `srcset`) serves the same image at different sizes. Art direction serves entirely different images (different crops, different compositions).

Use art direction sparingly — it requires creating and maintaining multiple image variants, which increases development and storage costs.

### Lazy Loading

The `loading` attribute controls when the browser loads the image:

```html
<img src="photo.jpg" alt="Description" loading="lazy">
```

| Value | Behavior |
|---|---|
| `eager` | Load the image immediately (default) |
| `lazy` | Defer loading until the image is close to the viewport |

Lazy loading saves bandwidth by not downloading images that the user never scrolls to see. It is supported in all modern browsers.

When to use lazy loading:

- Images below the fold (not visible on initial page load)
- Images in long articles or galleries
- Any image the user must scroll to reach

When to use eager loading:

- Images in the initial viewport (above the fold)
- The page's Largest Contentful Paint (LCP) image — do not lazy load this
- Images that are likely to be needed immediately

The browser's lazy loading threshold is implementation-specific. Chrome loads images approximately 1250px before they enter the viewport, while other browsers may differ.

### Figure and Figcaption

The `<figure>` element associates an image (or other content) with a caption:

```html
<figure>
    <img src="chart.png" alt="Quarterly revenue chart">
    <figcaption>Quarterly revenue for fiscal year 2026, showing steady growth across all quarters.</figcaption>
</figure>
```

- `<figure>` is a semantic container for self-contained content
- `<figcaption>` provides the caption, which can be placed before or after the content
- The figure can contain multiple images, videos, code blocks, or even tables

Multiple images in one figure:

```html
<figure>
    <img src="before.jpg" alt="Room before renovation">
    <img src="after.jpg" alt="Room after renovation">
    <figcaption>Office renovation project — before and after.</figcaption>
</figure>
```

The `<figure>` element should be self-contained — it can be moved to another part of the document without affecting the main content flow.

### Image Maps

Image maps let different areas of a single image link to different destinations:

```html
<img src="world-map.png" alt="World map" usemap="#world-map">

<map name="world-map">
    <area shape="rect" coords="0,0,100,200" href="/north-america" alt="North America">
    <area shape="circle" coords="200,100,50" href="/europe" alt="Europe">
    <area shape="poly" coords="100,200,150,250,80,300" href="/asia" alt="Asia">
</map>
```

The `<map>` element defines clickable regions. Each `<area>` specifies:

- **`shape`** — `rect` (rectangle), `circle`, or `poly` (polygon)
- **`coords`** — coordinates defining the region
- **`href`** — the link destination
- **`alt`** — accessible text for the region

Coordinates are measured in pixels from the top-left corner of the image.

Image maps are less common today because they do not work well on responsive layouts (coordinates are fixed pixels). Use SVG or CSS-based clickable regions instead for responsive designs.

### Favicons

Favicons are small icons associated with a website. They appear in browser tabs, bookmarks, and search results.

Basic favicon:

```html
<link rel="icon" href="favicon.ico" type="image/x-icon">
```

Modern favicon setup supports multiple devices and resolutions:

```html
<!-- Classic favicon -->
<link rel="icon" href="/favicon.ico" sizes="any">

<!-- PNG favicon for modern browsers -->
<link rel="icon" href="/favicon-16x16.png" sizes="16x16" type="image/png">
<link rel="icon" href="/favicon-32x32.png" sizes="32x32" type="image/png">

<!-- Apple touch icon (iOS home screen) -->
<link rel="apple-touch-icon" href="/apple-touch-icon.png">

<!-- Android / PWA -->
<link rel="manifest" href="/site.webmanifest">
```

Best practices:

- Use an SVG favicon when possible (scales to any size)
- Place favicon files in the root directory so browsers find them automatically
- Use a favicon generator (like realfavicongenerator.net) to create all required sizes
- Keep the favicon simple — it is displayed at very small sizes

### Performance Considerations

Images are the largest contributor to page weight. Optimizing them is one of the most impactful performance improvements available.

**Serve modern formats.** Use WebP or AVIF with `<picture>` fallbacks. These formats typically reduce file size by 25-50% compared to JPEG and PNG.

**Resize images appropriately.** Do not serve a 4000px-wide image for a 400px-wide display. Use `srcset` to let the browser choose the right size.

**Compress images.** Use lossy compression for photographs (quality 80-85 is often indistinguishable from 100) and lossless compression for graphics.

**Use responsive images.** With `srcset` and `sizes`, the browser downloads only what it needs based on the display size and resolution.

**Lazy load offscreen images.** Add `loading="lazy"` to images below the fold. This reduces initial page weight and speeds up the initial render.

**Preload LCP images.** The Largest Contentful Paint (LCP) image should be preloaded to prioritize its download:

```html
<link rel="preload" as="image" href="hero-image.webp" imagesrcset="hero-400.webp 400w, hero-800.webp 800w" imagesizes="100vw">
```

**Use CDN image services.** Image CDNs (Cloudinary, Imgix, Cloudflare Images) automatically resize, compress, and convert images based on the requesting device. They eliminate the need to manually create multiple image variants.

**Add dimensions to prevent layout shift.** Always include `width` and `height` attributes to reserve space and prevent CLS.

### Common Mistakes

**Missing `alt` attribute.** Every `<img>` must have `alt`. For decorative images, use `alt=""` (empty). For informative images, provide a descriptive text alternative.

**Text in images.** Avoid putting text in images. Text in images is not selectable, not searchable, does not scale with browser zoom, and does not translate with browser translation tools. Use CSS-styled text instead.

**Oversized images.** Serving a 3000px-wide image for a thumbnail that displays at 200px wastes bandwidth. Use `srcset` to serve appropriately sized images.

**No lazy loading on long pages.** Pages with many images should use `loading="lazy"` for images below the fold to reduce initial page weight.

**Missing image dimensions.** Without `width` and `height`, images cause layout shifts when they load. Always specify dimensions.

**Outdated format.** Using only JPEG and PNG when WebP and AVIF are supported by most browsers wastes bandwidth. Use `<picture>` with format fallbacks.

**Assuming everyone sees images.** Some users browse with images disabled (screen readers, low-bandwidth mode, text-only browsers). Good `alt` text ensures the content is still understandable.

**Too many HTTP requests.** Each image is a separate HTTP request. Use CSS sprites (combining small images) or SVG icons (inline or sprite sheet) for icons.

## Study Cases

**Case 1: Lazy loading the hero image.**

A developer adds `loading="lazy"` to every image on the page, including the hero image at the top. The hero is the Largest Contentful Paint element. Chrome delays loading it, the hero is blank on initial paint, and the LCP score is terrible.

The fix: use `loading="eager"` (or omit `loading`, which defaults to eager) on the hero image. Only use `loading="lazy"` for images below the fold.

```html
<!-- Hero image — load immediately -->
<img src="hero.jpg" alt="Hero banner" width="1200" height="600" loading="eager">

<!-- Below-fold images — lazy load -->
<img src="gallery-1.jpg" alt="Gallery photo 1" loading="lazy" width="400" height="300">
```

**Case 2: The broken responsive image.**

A developer sets `srcset` but omits the `sizes` attribute:

```html
<img src="photo-800.jpg" srcset="photo-400.jpg 400w, photo-800.jpg 800w, photo-1200.jpg 1200w" alt="Photo">
```

Without `sizes`, the browser assumes `sizes="100vw"` — the image takes the full viewport width. If the image is actually displayed at 400px in a sidebar, the browser may download the 1200px version unnecessarily.

The fix: always include `sizes`:

```html
<img src="photo-800.jpg" srcset="photo-400.jpg 400w, photo-800.jpg 800w, photo-1200.jpg 1200w" sizes="(max-width: 768px) 100vw, 400px" alt="Photo">
```

**Case 3: The decorative image with verbose alt text.**

A developer adds alt text to a decorative border image:

```html
<img src="decorative-border.png" alt="A decorative border with blue and green swirling patterns">
```

Screen reader users hear this description every time they load the page. Since the image is purely decorative, `alt=""` is more appropriate:

```html
<img src="decorative-border.png" alt="">
```

## Examples

**Example 1: Responsive hero image with format fallback.**

```html
<picture>
    <source srcset="hero.avif" type="image/avif">
    <source srcset="hero.webp" type="image/webp">
    <img src="hero.jpg" alt="Mountain landscape at sunset" width="1200" height="600" loading="eager">
</picture>
```

**Example 2: Art direction — different crops per screen size.**

```html
<picture>
    <source
        srcset="hero-desktop.avif"
        media="(min-width: 1024px)"
        type="image/avif"
    >
    <source
        srcset="hero-desktop.webp"
        media="(min-width: 1024px)"
        type="image/webp"
    >
    <source
        srcset="hero-mobile.avif"
        type="image/avif"
    >
    <source
        srcset="hero-mobile.webp"
        type="image/webp"
    >
    <img
        src="hero-mobile.jpg"
        alt="Team collaborating in a modern office space"
        width="800"
        height="600"
        loading="eager"
    >
</picture>
```

**Example 3: Resolution switching with srcset.**

```html
<img
    src="photo-800.jpg"
    srcset="photo-400.jpg 400w, photo-800.jpg 800w, photo-1200.jpg 1200w, photo-1600.jpg 1600w"
    sizes="(max-width: 600px) 100vw, (max-width: 1024px) 50vw, 800px"
    alt="Aerial view of a city skyline"
    width="800"
    height="500"
    loading="lazy"
>
```

**Example 4: Gallery with figure and lazy loading.**

```html
<h2>Photo Gallery</h2>

<figure>
    <img src="gallery/photo1-800.jpg" srcset="gallery/photo1-400.jpg 400w, gallery/photo1-800.jpg 800w" sizes="(max-width: 600px) 100vw, 800px" alt="Sunset over the ocean" width="800" height="600" loading="lazy">
    <figcaption>Sunset over the Pacific Ocean, captured at Cannon Beach.</figcaption>
</figure>

<figure>
    <img src="gallery/photo2-800.jpg" srcset="gallery/photo2-400.jpg 400w, gallery/photo2-800.jpg 800w" sizes="(max-width: 600px) 100vw, 800px" alt="Forest path covered in autumn leaves" width="800" height="600" loading="lazy">
    <figcaption>Forest trail in Olympic National Park during fall.</figcaption>
</figure>
```

**Example 5: Inline SVG for logo.**

```html
<a href="/" aria-label="Home">
    <svg viewBox="0 0 200 50" xmlns="http://www.w3.org/2000/svg" width="200" height="50">
        <rect width="50" height="50" rx="8" fill="#0056b3"/>
        <text x="62" y="35" font-family="Arial, sans-serif" font-size="28" font-weight="bold" fill="#333">DevBook</text>
    </svg>
</a>
```

## Glossary

| Term | Definition |
|------|------------|
| `src` | The image URL attribute |
| `alt` | Alternative text for images, used by screen readers and shown when the image fails to load |
| `srcset` | Attribute listing multiple image sources with their widths or pixel densities |
| `sizes` | Attribute specifying how large the image will display at different viewport widths |
| Responsive image | An image that adapts to different screen sizes and resolutions |
| Art direction | Serving different image crops or compositions for different screen sizes |
| Replaced element | An element whose appearance is determined by an external resource |
| Layout shift | When page content moves after an image loads, causing a poor user experience |
| CLS | Cumulative Layout Shift — a Core Web Vital metric measuring visual stability |
| LCP | Largest Contentful Paint — a Core Web Vital measuring perceived load speed |
| Lazy loading | Deferring image loading until it is near the viewport |
| Eager loading | Loading an image immediately regardless of position |
| Lossy compression | Compression that discards data to reduce file size (JPEG, WebP lossy) |
| Lossless compression | Compression that preserves all original data (PNG, WebP lossless) |
| Vector | Image defined by mathematical paths rather than pixels (SVG) |
| Raster | Image defined by a grid of pixels (JPEG, PNG, WebP) |
| Favicon | A small icon representing a website in tabs and bookmarks |
| Image map | An image with multiple clickable regions defined by `<map>` and `<area>` |
| Figure | A self-contained content unit with optional caption |
| Figcaption | The caption element for a figure |
| CDN | Content Delivery Network — a distributed server network for fast content delivery |
| WebP | Modern image format by Google with superior compression |
| AVIF | Next-gen image format based on AV1 video codec |
| Sprite | A single image file containing multiple smaller images to reduce HTTP requests |

## Quick References

- [MDN: `<img>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img) — complete reference
- [MDN: `<picture>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/picture) — responsive images
- [MDN: `<figure>` Element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/figure) — figure reference
- [MDN: Responsive Images](https://developer.mozilla.org/en-US/docs/Learn/HTML/Multimedia_and_embedding/Responsive_images) — comprehensive guide
- [MDN: Image `loading` attribute](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/img#loading) — lazy loading reference
- [HTML Spec: Images](https://html.spec.whatwg.org/multipage/images.html) — official specification
- [WebP Guide](https://developers.google.com/speed/webp) — Google's WebP documentation
- [AVIF Guide](https://avif.io/) — AVIF format information
- [Real Favicon Generator](https://realfavicongenerator.net/) — generate all favicon sizes
- [Cloudinary Image Optimization](https://cloudinary.com/blog/image-optimization-guide) — practical optimization tips

## Next Steps

- [Tables](tables.md) — tabular data structure and accessibility
- [Meta Tags & Social Sharing](meta-tags.md) — metadata for SEO and social media
- [Video & Audio](video-and-audio.md) — embedding multimedia content
- [Semantic HTML & Document Outline](semantic-html.md) — structural meaning in markup
