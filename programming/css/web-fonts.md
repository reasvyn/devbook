# Web Fonts

## Description

Web fonts allow custom typefaces to be downloaded and rendered in the browser. The `@font-face` rule, font format choices, and loading strategies determine both visual quality and page performance.

## Prerequisites

- [Typography](typography.md) — font properties and usage
- [Colors & Backgrounds](colors-and-backgrounds.md) — visual styling foundation

## Table of Contents

- [The @font-face Rule](#the-font-face-rule)
- [Font Formats](#font-formats)
- [Font Display Strategies](#font-display-strategies)
- [Loading Multiple Weights](#loading-multiple-weights)
- [Variable Fonts with @font-face](#variable-fonts-with-font-face)
- [Google Fonts and CDNs](#google-fonts-and-cdns)
- [Self-Hosting](#self-hosting)
- [Font Subsetting](#font-subsetting)
- [Performance Optimization](#performance-optimization)
- [FOUT and FOIT](#fout-and-foit)
- [Font Loading API](#font-loading-api)
- [System Fonts as Fallback](#system-fonts-as-fallback)
- [Unicode Range](#unicode-range)
- [Font Display Descriptors](#font-display-descriptors)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The @font-face Rule

The `@font-face` rule defines a custom font family:

```css
@font-face {
    font-family: 'Inter';
    src: url('Inter-Regular.woff2') format('woff2');
    font-weight: 400;
    font-style: normal;
    font-display: swap;
}
```

Once defined, use it like any font family:

```css
body {
    font-family: 'Inter', 'Segoe UI', sans-serif;
}
```

Required descriptors: `font-family` (name you assign) and `src` (font file URL).

### Font Formats

Different font formats have different browser support and compression:

| Format | Extension | Compression | Support |
|---|---|---|---|
| WOFF2 | .woff2 | Best (30–50% smaller than WOFF) | All modern browsers |
| WOFF | .woff | Good | Legacy browsers (IE9+) |
| TTF/OTF | .ttf, .otf | None | All browsers (slow download) |
| EOT | .eot | Poor | Internet Explorer only |
| SVG | .svg | Poor | Safari (deprecated) |

```css
@font-face {
    font-family: 'MyFont';
    src: url('MyFont.woff2') format('woff2'),
         url('MyFont.woff') format('woff');
    /* No TTF fallback needed — WOFF2 covers 98%+ of users */
}
```

**Recommendation:** WOFF2 only + WOFF fallback for IE11. Skip TTF, EOT, and SVG.

### Font Display Strategies

The `font-display` descriptor controls how the browser handles the font loading period:

```css
/* Default behavior — browser decides */
@font-face {
    font-family: 'Inter';
    src: url('Inter.woff2') format('woff2');
    font-display: auto;
}
```

| Value | Behavior |
|---|---|
| `auto` | Browser default (usually block period then swap) |
| `block` | Invisible text for ~3s, then swap |
| `swap` | Fallback text immediately, swap when loaded |
| `fallback` | Fallback text for ~100ms, then invisible for ~3s, then swap |
| `optional` | Fallback text for ~100ms, font may never load |

**`font-display: swap`** — most common for web fonts. Users see text immediately (in fallback font) and it swaps to custom font when loaded.

**`font-display: optional`** — performance-first. The font is used only if loaded before the page renders; otherwise, fallback text stays.

```css
@font-face {
    font-family: 'Inter';
    src: url('Inter-Regular.woff2') format('woff2');
    font-display: swap;           /* Best UX */
}

@font-face {
    font-family: 'Decorative';
    src: url('Display.woff2') format('woff2');
    font-display: optional;       /* Not critical */
}
```

### Loading Multiple Weights

Each weight needs a separate `@font-face` declaration:

```css
@font-face {
    font-family: 'Inter';
    src: url('Inter-Regular.woff2') format('woff2');
    font-weight: 400;
    font-style: normal;
    font-display: swap;
}

@font-face {
    font-family: 'Inter';
    src: url('Inter-Medium.woff2') format('woff2');
    font-weight: 500;
    font-style: normal;
    font-display: swap;
}

@font-face {
    font-family: 'Inter';
    src: url('Inter-Bold.woff2') format('woff2');
    font-weight: 700;
    font-style: normal;
    font-display: swap;
}

@font-face {
    font-family: 'Inter';
    src: url('Inter-Italic.woff2') format('woff2');
    font-weight: 400;
    font-style: italic;
    font-display: swap;
}
```

Using it:

```css
body {
    font-family: 'Inter', sans-serif;
    font-weight: 400;
}

h1 {
    font-weight: 700;    /* Loads Inter-Bold.woff2 */
}

em {
    font-style: italic;  /* Loads Inter-Italic.woff2 */
}
```

**Avoid `font-weight: bold` in CSS if you only declared `font-weight: 400`** — the browser's faux bold (algorithmic bold) looks worse than the real bold font.

### Variable Fonts with @font-face

Variable fonts encode multiple axes in one file:

```css
@font-face {
    font-family: 'InterVariable';
    src: url('InterVariable.woff2') format('woff2 supports variations');
    font-weight: 100 900;
    font-stretch: 75% 125%;
    font-display: swap;
}
```

`font-weight: 100 900` declares the supported range. Browsers will match any weight within this range without downloading additional files.

For browsers that do not support the `supports variations` format hint:

```css
@font-face {
    font-family: 'InterVariable';
    src: url('InterVariable.woff2') format('woff2 supports variations'),
         url('InterVariable.woff2') format('woff2-variations');
    font-weight: 100 900;
    font-display: swap;
}
```

### Google Fonts and CDNs

Google Fonts provides hosted web fonts:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap"
      rel="stylesheet">
```

The `display=swap` parameter sets `font-display: swap`. Always include it.

**Performance concerns with Google Fonts:**
- Blocking CSS in `<head>` delays render
- Multiple font files = multiple HTTP requests
- Third-party origin = DNS lookup + TLS handshake
- Use only what you need (specific weights, latin subset)

**Self-hosting is often faster** — one fewer connection, no DNS/SSL overhead, and you control caching.

### Self-Hosting

Host font files on your own server:

```
fonts/
├── Inter-Regular.woff2
├── Inter-Medium.woff2
├── Inter-Bold.woff2
└── Inter-Italic.woff2
```

```css
@font-face {
    font-family: 'Inter';
    src: url('/fonts/Inter-Regular.woff2') format('woff2');
    font-weight: 400;
    font-style: normal;
    font-display: swap;
}
/* ... repeat for each weight ... */
```

Advantages:
- No third-party DNS/handshake
- Cache with your own service worker
- Works offline
- No privacy concerns (no visitor data sent to Google)

### Font Subsetting

Reducing the character set to only what you need reduces file size significantly:

```
Before subsetting:  Inter-Regular.woff2 = 96KB
After latin subset: Inter-Regular.woff2 = 28KB
```

Tools: `pyftsubset` (fonttools), glyphhanger, Google's `&text=` parameter.

**Google Fonts text subsetting:**

```html
<!-- Only these letters: load only the characters you need -->
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@700&text=HelloWorld"
      rel="stylesheet">
```

**Manual subsetting with fonttools:**

```bash
pyftsubset Inter-Regular.ttf --unicodes="U+0000-00FF" --output-file=Inter-Regular-subset.woff2 --flavor=woff2
```

Common unicode ranges:
- `U+0000-00FF` — Basic Latin + Latin-1 Supplement (English + Western European)
- `U+0000-024F` — Latin Extended (most European languages)
- `U+0400-04FF` — Cyrillic
- `U+4E00-9FFF` — CJK Unified Ideographs

### Performance Optimization

**1. Preload critical fonts:**

```html
<link rel="preload" href="/fonts/Inter-Regular.woff2" as="font" type="font/woff2" crossorigin>
<link rel="preload" href="/fonts/Inter-Bold.woff2" as="font" type="font/woff2" crossorigin>
```

Preload ensures the font download starts early (before CSS is parsed).

**2. Use font-display: swap:**

Allows text to render immediately with fallback, preventing invisible text.

**3. Subset aggressively:**

Remove characters unused by your content. For English sites, the Latin subset is typically sufficient.

**4. Serve only WOFF2:**

WOFF2 compression is significantly better than WOFF. Skip WOFF unless supporting IE11.

**5. Use a single variable font instead of multiple static fonts:**

One 80KB variable font replaces four 30KB non-variable fonts.

**6. Cache efficiently:**

```apache
# .htaccess — cache font files for a year
<FilesMatch "\.(woff2)$">
    Header set Cache-Control "public, max-age=31536000, immutable"
</FilesMatch>
```

### FOUT and FOIT

**FOUT** — Flash of Unstyled Text (swap behavior):
- Fallback text appears immediately
- Swaps to custom font after load
- Visible flash of text style change

**FOIT** — Flash of Invisible Text (block behavior):
- Text is invisible for up to 3 seconds
- After font loads (or timeout), text appears
- Worse UX — no content for seconds

**FOUT is preferable** — users see content immediately. Mitigate FOUT visual disruption by sizing the fallback font to match:

```css
body {
    font-family: 'Inter', system-ui, sans-serif;
}

/* Inter metrics approximately match system font metrics */
/* This reduces layout shift when the font swaps */
```

The `size-adjust` descriptor in `@font-face` helps align fallback font metrics:

```css
@font-face {
    font-family: 'Fallback';
    src: local('Arial');
    size-adjust: 95%;           /* Scale Arial to match Inter's visual size */
    ascent-override: 90%;
    descent-override: 20%;
    line-gap-override: 10%;
}

body {
    font-family: 'Inter', 'Fallback', sans-serif;
}
```

### Font Loading API

JavaScript control over font loading:

```javascript
// Check if a font is loaded
document.fonts.ready.then(() => {
    console.log('All fonts loaded');
    document.documentElement.classList.add('fonts-loaded');
});

// Load a specific font
const font = new FontFace('Inter', 'url(Inter.woff2)');
font.load().then(() => {
    document.fonts.add(font);
    document.documentElement.classList.add('fonts-loaded');
});

// Font loading events
document.fonts.addEventListener('loading', () => {});
document.fonts.addEventListener('loadingdone', () => {});
document.fonts.addEventListener('loadingerror', () => {});
```

**CSS class pattern for font-aware styling:**

```css
/* Fonts not loaded yet — simplify animations/motion */
.page-heading {
    font-family: 'Inter', sans-serif;
    letter-spacing: -0.03em;
}

.fonts-loaded .page-heading {
    letter-spacing: -0.02em;  /* Adjust for actual font metrics */
}
```

### System Fonts as Fallback

Match system fonts to custom font metrics for minimal FOUT shift:

```css
@font-face {
    font-family: 'InterFallback';
    src: local('Arial');
    size-adjust: 107%;
    ascent-override: 91%;
    descent-override: 23%;
    line-gap-override: 0%;
}

body {
    font-family: 'Inter', 'InterFallback', sans-serif;
}
```

Tools like `@font-face` generator (fonts.google.com) or `fontpie` calculate override values.

### Unicode Range

Limit which characters are loaded from each font declaration:

```css
/* Only load Latin characters from Inter */
@font-face {
    font-family: 'Inter';
    src: url('Inter-Latin.woff2') format('woff2');
    unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA,
                   U+02DC, U+2000-206F, U+2074, U+20AC, U+2122, U+2191, U+2193,
                   U+2212, U+2215, U+FEFF, U+FFFD;
    font-display: swap;
}

/* Separate CJK declaration */
@font-face {
    font-family: 'Noto Sans SC';
    src: url('NotoSansSC.woff2') format('woff2');
    unicode-range: U+4E00-9FFF, U+3400-4DBF;
    font-display: swap;
}
```

### Font Display Descriptors

Full set of `@font-face` descriptors:

```css
@font-face {
    /* Identity */
    font-family: 'MyFont';
    src: url('MyFont.woff2') format('woff2');

    /* Style matching */
    font-weight: 400;
    font-style: normal;
    font-stretch: normal;

    /* Loading behavior */
    font-display: swap;

    /* Metrics overrides for fallback alignment */
    size-adjust: 100%;
    ascent-override: normal;
    descent-override: normal;
    line-gap-override: normal;

    /* Character matching */
    unicode-range: U+0000-00FF;
}
```

The metrics override descriptors (`size-adjust`, `ascent-override`, `descent-override`, `line-gap-override`) are for aligning fallback fonts with custom fonts to minimize layout shift.

## Study Cases

### Complete Self-Hosted Font Setup

```css
/* Inter — Regular */
@font-face {
    font-family: 'Inter';
    src: url('/fonts/inter/Inter-Regular.woff2') format('woff2');
    font-weight: 400;
    font-style: normal;
    font-display: swap;
}

/* Inter — Medium */
@font-face {
    font-family: 'Inter';
    src: url('/fonts/inter/Inter-Medium.woff2') format('woff2');
    font-weight: 500;
    font-style: normal;
    font-display: swap;
}

/* Inter — Semi Bold */
@font-face {
    font-family: 'Inter';
    src: url('/fonts/inter/Inter-SemiBold.woff2') format('woff2');
    font-weight: 600;
    font-style: normal;
    font-display: swap;
}

/* Inter — Bold */
@font-face {
    font-family: 'Inter';
    src: url('/fonts/inter/Inter-Bold.woff2') format('woff2');
    font-weight: 700;
    font-style: normal;
    font-display: swap;
}

/* Fallback with size adjustment for minimal CLS */
@font-face {
    font-family: 'InterFallback';
    src: local('Arial');
    size-adjust: 107%;
    ascent-override: 91%;
    descent-override: 23%;
    line-gap-override: 0%;
}

body {
    font-family: 'Inter', 'InterFallback', system-ui, -apple-system, sans-serif;
}
```

### Optimized Google Fonts Integration

```html
<!-- Preconnect to origins -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>

<!-- Preload the CSS that links fonts -->
<link rel="preload" as="style"
      href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap">

<!-- Load the stylesheet -->
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;700&display=swap"
      rel="stylesheet">
```

## Examples

### Example 1: Variable font setup

```css
@font-face {
    font-family: 'InterVariable';
    src: url('/fonts/InterVariable.woff2') format('woff2 supports variations'),
         url('/fonts/InterVariable.woff2') format('woff2-variations');
    font-weight: 100 900;
    font-stretch: 75% 125%;
    font-display: swap;
}

body {
    font-family: 'InterVariable', system-ui, sans-serif;
}

h1 {
    font-weight: 620;       /* Between 600 and 700 */
    font-stretch: 110%;     /* Slightly wider */
}
```

### Example 2: Font loading with CSS class guard

```css
/* Before fonts load — simpler visuals */
h1 {
    font-size: 2rem;
}

/* After fonts load — apply true typeface */
.fonts-loaded h1 {
    font-family: 'Inter', system-ui, sans-serif;
    font-weight: 700;
    letter-spacing: -0.02em;
}
```

```javascript
// Inline in <head>
(function() {
    if ('fonts' in document) {
        document.fonts.ready.then(function() {
            document.documentElement.classList.add('fonts-loaded');
        });
    } else {
        document.documentElement.classList.add('fonts-loaded');
    }
})();
```

### Example 3: Icon font alternative with inline SVG

```css
/* Instead of icon fonts (which are not fonts), use inline SVGs */
.icon {
    display: inline-block;
    width: 1em;
    height: 1em;
    vertical-align: middle;
    fill: currentColor;     /* Inherit text color */
}
```

## Glossary

| Term | Definition |
|---|---|
| @font-face | CSS rule defining custom font family |
| WOFF2 | Web Open Font Format 2 (best compression) |
| Variable font | Single file with adjustable design axes |
| Font-display | Strategy for rendering text during font load |
| FOUT | Flash of Unstyled Text — fallback then swap |
| FOIT | Flash of Invisible Text — invisible during load |
| Subsetting | Removing unused characters from font file |
| Preload | HTML hint to start font download early |
| Size-adjust | Descriptor scaling fallback font metrics |
| Unicode range | Character range for which @font-face applies |

## Quick References

- [MDN: @font-face](https://developer.mozilla.org/en-US/docs/Web/CSS/@font-face) — complete reference
- [Google Fonts Guide](https://developers.google.com/fonts/docs/css2) — using Google Fonts optimally
- [Variable Fonts](https://variablefonts.io/) — comprehensive variable font resource
- [Font Style Matcher](https://meowni.ca/font-style-matcher/) — align fallback metrics
- [Web Font Optimization](https://web.dev/optimize-webfont-loading/) — performance guide
- [Wakamai Fondue](https://wakamaifondue.com/) — inspect font capabilities

## Next Steps

- [Display Types](display-types.md) — how display properties affect layout