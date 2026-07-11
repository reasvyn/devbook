# CSS Typography

## Description

Typography is the craft of making text readable, accessible, and visually pleasing. CSS provides fine-grained control over typefaces, sizes, spacing, and layout of text.

## Prerequisites

- [Values & Units](values-and-units.md) — font size units (rem, em, ch)
- [The Box Model](box-model.md) — inline-level element behavior

## Table of Contents

- [Font Family](#font-family)
- [Font Size](#font-size)
- [Font Weight](#font-weight)
- [Font Style](#font-style)
- [Line Height](#line-height)
- [Text Alignment](#text-alignment)
- [Text Decoration](#text-decoration)
- [Text Transform](#text-transform)
- [Letter Spacing](#letter-spacing)
- [Word Spacing](#word-spacing)
- [White Space](#white-space)
- [Font Variant](#font-variant)
- [Font Shorthand](#font-shorthand)
- [Variable Fonts](#variable-fonts)
- [Font Features](#font-features)
- [System Font Stack](#system-font-stack)
- [Readability and Accessibility](#readability-and-accessibility)
- [Web Safe Fonts](#web-safe-fonts)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Font Family

The `font-family` property specifies typeface preferences as a stack:

```css
body {
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto,
                 Oxygen, Ubuntu, Cantarell, sans-serif;
}
```

The browser uses the first available font in the stack. Generic families act as final fallbacks:

```css
body {
    font-family: 'Georgia', 'Times New Roman', serif;
}

code, pre {
    font-family: 'JetBrains Mono', 'Fira Code', 'Consolas', monospace;
}
```

**Generic families:**
- `serif` — small decorative strokes at line ends (Times New Roman, Georgia)
- `sans-serif` — clean without strokes (Arial, Helvetica, system-ui)
- `monospace` — fixed-width characters (Consolas, Courier New)
- `cursive` — handwritten style (Comic Sans, Brush Script)
- `fantasy` — decorative (Impact, Papyrus)
- `system-ui` — the platform's default UI font
- `math` — mathematical notation conventions
- `emoji` — emoji rendering
- `fangsong` — a particular Chinese style between serif and cursive

### Font Size

Set text size with `font-size`:

```css
body {
    font-size: 16px;                    /* Fixed, not recommended */
    font-size: 100%;                     /* Preferred — respects user defaults */
    font-size: 1rem;                     /* Same as 100% */
}

h1 { font-size: 2.5rem; }               /* 40px at 16px root */
h2 { font-size: 2rem; }                 /* 32px */
h3 { font-size: 1.75rem; }             /* 28px */
h4 { font-size: 1.5rem; }              /* 24px */
h5 { font-size: 1.25rem; }             /* 20px */
h6 { font-size: 1rem; }                /* 16px */

p { font-size: 1rem; }
small { font-size: 0.875rem; }
```

**Fluid typography with clamp():**

```css
h1 {
    font-size: clamp(2rem, 1.5rem + 2vw, 4rem);
}

body {
    font-size: clamp(1rem, 0.9rem + 0.4vw, 1.25rem);
}
```

**Relative sizing patterns:**
- `rem` — consistent across the page, independent of nesting
- `em` — scales with parent, risky with nesting
- `%` — behaves like em, relative to parent
- `vw` — scales with viewport, useful for fluid layouts

### Font Weight

Weight controls the thickness of text:

```css
.text {
    font-weight: normal;        /* 400 */
    font-weight: bold;          /* 700 */
    font-weight: lighter;       /* Lighter than parent */
    font-weight: bolder;        /* Heavier than parent */
    font-weight: 100;           /* Thin */
    font-weight: 200;           /* Extra Light */
    font-weight: 300;           /* Light */
    font-weight: 400;           /* Normal / Regular */
    font-weight: 500;           /* Medium */
    font-weight: 600;           /* Semi Bold */
    font-weight: 700;           /* Bold */
    font-weight: 800;           /* Extra Bold */
    font-weight: 900;           /* Black */
}
```

Not every font supports all nine weights. Map available weights to headings:

```css
h1 { font-weight: 800; }
h2 { font-weight: 700; }
h3 { font-weight: 600; }
h4 { font-weight: 600; }
body { font-weight: 400; }
strong { font-weight: 600; }    /* Slightly milder bold */
```

### Font Style

```css
.text {
    font-style: normal;
    font-style: italic;             /* Italic (designed glyphs) */
    font-style: oblique;            /* Slanted (algorithmic slant) */
    font-style: oblique -10deg;     /* Degree of slant (CSS3+) */
}
```

Italic uses specially designed letterforms. Oblique just slants the regular glyphs. Most fonts only provide italic.

### Line Height

Line height controls the vertical space between lines:

```css
body {
    line-height: 1.6;           /* Unitless — relative to font-size */
}

p {
    line-height: 1.75;          /* 1.75 × font-size for comfortable reading */
}

h1, h2, h3 {
    line-height: 1.2;           /* Tighter for headings */
}

.small {
    line-height: 1.2rem;        /* Fixed — not recommended */
    line-height: 120%;          /* Percentage — relative to font-size */
}
```

**Unitless line-height** is strongly preferred. It inherits as a factor, not a computed value:

```css
body { line-height: 1.6; }          /* Factor 1.6 */
.container { font-size: 1.25rem; }  /* line-height stays 1.6 (20px) */

/* With percentage: */
body { line-height: 160%; }         /* Computed: 16px × 1.6 = 25.6px */
.container { font-size: 1.25rem; }  /* line-height: 25.6px (does NOT scale) */
```

With unitless, the line-height scales with the element's font-size. With percentage, it computes to a fixed value based on the parent's font-size.

### Text Alignment

```css
.text {
    text-align: left;           /* Default for LTR languages */
    text-align: right;
    text-align: center;
    text-align: justify;        /* Stretch lines to fill width */
}
```

**Justify caveats:** Justified text can create uneven word spacing (rivers). Pair with `text-justify` for better control:

```css
.text {
    text-align: justify;
    text-justify: inter-word;   /* Better word spacing */
}
```

**Last line alignment:**

```css
.text {
    text-align: justify;
    text-align-last: left;      /* Last line not stretched */
}
```

**Vertical alignment:**

```css
.inline-element {
    vertical-align: baseline;       /* Default */
    vertical-align: top;
    vertical-align: middle;
    vertical-align: bottom;
    vertical-align: super;          /* Superscript */
    vertical-align: sub;            /* Subscript */
    vertical-align: text-top;
    vertical-align: text-bottom;
}
```

`vertical-align` only affects inline and inline-block elements. It does NOT work in Flexbox or Grid (they use `align-items` instead).

### Text Decoration

```css
.text {
    text-decoration: underline;
    text-decoration: overline;
    text-decoration: line-through;
    text-decoration: none;           /* Remove link underlines */
}

/* Thickness, style, and color control (CSS3+) */
.text {
    text-decoration-line: underline;
    text-decoration-style: wavy;     /* solid, double, dotted, dashed, wavy */
    text-decoration-color: red;
    text-decoration-thickness: 2px;  /* Auto, from-font, or explicit */
}

/* Shorthand */
.text {
    text-decoration: underline wavy red 2px;
}

/* Underline offset — position adjustment */
.text {
    text-underline-offset: 0.25em;   /* Move underline down */
    text-underline-offset: auto;
}
```

Skip ink — avoid descenders crossing the underline:

```css
.text {
    text-decoration: underline;
    text-decoration-skip-ink: auto;  /* Default — safe */
    text-decoration-skip-ink: none;  /* Underline cuts through descenders */
}
```

### Text Transform

```css
.text {
    text-transform: uppercase;       /* ALL CAPS */
    text-transform: lowercase;       /* all lowercase */
    text-transform: capitalize;      /* First Letter Of Each Word */
    text-transform: none;            /* As written in HTML */
}
```

Use `text-transform` for visual styling. Keep the original text in HTML for screen readers and copy-paste.

### Letter Spacing

Tracking — uniform space between characters:

```css
h1 {
    letter-spacing: -0.02em;         /* Tighten headings slightly */
}

.uppercase-text {
    letter-spacing: 0.05em;          /* Loosen caps for readability */
}

.small-caps {
    letter-spacing: 0.1em;
}
```

Use `em` for letter spacing so it scales with font size. Avoid `px` values that break on zoom.

### Word Spacing

Space between words:

```css
.text {
    word-spacing: normal;            /* Default (0) */
    word-spacing: 0.25em;            /* Extra space between words */
    word-spacing: -0.1em;            /* Reduce space */
}
```

Rarely changed from normal. Useful for justified text to reduce rivering.

### White Space

Controls how whitespace in HTML is rendered:

```css
.text {
    white-space: normal;             /* Collapse, wrap (default) */
    white-space: nowrap;             /* Collapse, no wrap */
    white-space: pre;                /* Preserve, no wrap (like <pre>) */
    white-space: pre-wrap;           /* Preserve, wrap */
    white-space: pre-line;           /* Preserve lines, collapse spaces, wrap */
    white-space: break-spaces;       /* Pre-wrap + always break at spaces */
}
```

**Word breaking:**

```css
.text {
    word-break: normal;              /* Default breaking rules */
    word-break: break-all;           /* Break at any character */
    word-break: keep-all;            /* No breaking (CJK text) */
}

.text {
    overflow-wrap: normal;           /* Default */
    overflow-wrap: break-word;       /* Break only if word would overflow */
}

/* Hyphenation */
.text {
    hyphens: auto;                   /* Automatic hyphenation */
    hyphens: manual;                 /* Only manual hyphens (soft hyphen) */
    hyphens: none;
}
```

Difference between `word-break: break-all` and `overflow-wrap: break-word`:
- `break-all` breaks at any character boundary immediately when the line is too long
- `break-word` only breaks at natural break points, and only when a word would overflow

### Font Variant

Control over OpenType features:

```css
.text {
    font-variant: normal;
    font-variant: small-caps;        /* Small capitals */
    font-variant: all-small-caps;
    font-variant: common-ligatures;  /* Enable standard ligatures (fi, fl) */
    font-variant: no-common-ligatures;
    font-variant: lining-nums;       /* Numbers on baseline */
    font-variant: oldstyle-nums;     /* Numbers with descenders */
    font-variant: proportional-nums; /* Variable-width numbers */
    font-variant: tabular-nums;      /* Fixed-width numbers (tables) */
}
```

Sub-properties for fine control:

```css
.text {
    font-variant-ligatures: common-ligatures discretionary-ligatures;
    font-variant-numeric: tabular-nums;
    font-variant-caps: small-caps;
    font-variant-alternates: stylistic(swash);
    font-variant-east-asian: proportional-width;
}
```

### Font Shorthand

The `font` shorthand combines multiple font properties:

```css
.element {
    font: italic 700 1.25rem / 1.6 'Inter', sans-serif;
    /* [style] [weight] [size] / [line-height] [family] */
}
```

Mandatory: `font-size` and `font-family`. Order matters: style, variant, weight before size, then line-height after size slash, then family.

```css
/* Equivalent longhand */
.element {
    font-style: italic;
    font-weight: 700;
    font-size: 1.25rem;
    line-height: 1.6;
    font-family: 'Inter', sans-serif;
}
```

Using font shorthand resets all other font sub-properties to their defaults. Be aware of this reset effect.

### Variable Fonts

Variable fonts contain multiple axes of variation in a single file:

```css
@font-face {
    font-family: 'InterVariable';
    src: url('InterVariable.woff2') format('woff2 supports variations');
    font-weight: 100 900;               /* Range */
    font-stretch: 75% 125%;             /* Width range */
    font-style: normal;
}

body {
    font-family: 'InterVariable', sans-serif;
}
```

**Custom axes:**

```css
.text {
    font-weight: 620;                   /* Precise weight in range */
    font-stretch: 110%;                 /* Wider than normal */

    /* Custom axis with font-variation-settings */
    font-variation-settings: 'wght' 620, 'wdth' 110, 'slnt' -5;
}
```

Registered axes (four-character tags):
- `wght` — weight (1–1000)
- `wdth` — width (percentage)
- `slnt` — slant (angle in degrees, positive = right)
- `ital` — italic (binary)
- `opsz` — optical size (point size)

Variable fonts reduce HTTP requests and provide continuous variation between extremes.

### Font Features

OpenType feature control:

```css
.text {
    font-feature-settings: 'kern' 1;        /* Kerning */
    font-feature-settings: 'liga' 1;        /* Standard ligatures */
    font-feature-settings: 'dlig' 1;        /* Discretionary ligatures */
    font-feature-settings: 'tnum' 1;        /* Tabular numbers */
    font-feature-settings: 'ss01' 1;        /* Stylistic set 1 */
    font-feature-settings: 'calt' 1;        /* Contextual alternates */
}

/* Multiple settings */
.text {
    font-feature-settings: 'kern' 1, 'tnum' 1, 'ss01' 1;
}
```

Prefer `font-variant-*` sub-properties over `font-feature-settings`. The sub-properties are higher-level and cascade better.

### System Font Stack

Use the platform's native UI font:

```css
body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto,
                 Oxygen, Ubuntu, Cantarell, 'Fira Sans', 'Droid Sans',
                 'Helvetica Neue', Arial, sans-serif;
}

/* Modern shorter stack */
body {
    font-family: system-ui, -apple-system, sans-serif;
}
```

The system font stack ensures the text matches the OS's native feel, avoids a font download, and performs well.

### Readability and Accessibility

**Line length:** 45–75 characters per line for body text:

```css
article {
    max-width: 65ch;    /* ~65 characters wide */
}
```

**Contrast:** Ensure sufficient color contrast between text and background. WCAG AA requires 4.5:1 for normal text, 3:1 for large text.

**Font size:** Do not set `font-size` below `1rem` for body text. Users should be able to resize text 200% without loss of content.

**Text spacing:** Allow users to override spacing for readability:

```css
/* User stylesheet example for accessibility */
* {
    line-height: 1.5 !important;
    letter-spacing: 0.12em !important;
    word-spacing: 0.16em !important;
}
```

### Web Safe Fonts

Fonts available on most systems without downloading:

| Font | Type | Available on |
|---|---|---|
| Arial | Sans-serif | Windows, macOS |
| Helvetica | Sans-serif | macOS, some Windows |
| Times New Roman | Serif | Windows, macOS |
| Georgia | Serif | Windows, macOS |
| Courier New | Monospace | Windows, macOS |
| Verdana | Sans-serif | Windows, macOS |
| Trebuchet MS | Sans-serif | Windows, macOS |
| Impact | Fantasy | Windows, macOS |
| Comic Sans MS | Cursive | Windows, macOS |
| Palatino Linotype | Serif | Windows, macOS |

Most modern sites use custom web fonts (via `@font-face` or Google Fonts), with fallback to system fonts.

## Glossary

| Term | Definition |
|---|---|
| Typeface | A font family (Inter, Georgia) |
| Font stack | Ordered list of fallback fonts |
| Serif | Small strokes at character edges |
| Sans-serif | Characters without serifs |
| Monospace | Fixed-width characters |
| Variable font | Font with adjustable design axes |
| Leading | Line height, space between lines |
| Tracking | Letter-spacing |
| Kerning | Space adjustment between specific character pairs |
| Ligature | Combined character glyphs (fi, fl) |
| x-height | Height of lowercase 'x' relative to font |

## Quick References

- [MDN: Fonts](https://developer.mozilla.org/en-US/docs/Web/CSS/font) — font properties reference
- [MDN: Text](https://developer.mozilla.org/en-US/docs/Web/CSS/text) — text styling reference
- [Modern Font Stacks](https://modernfontstacks.com/) — curated system font combos
- [Variable Fonts Guide](https://variablefonts.io/) — getting started with variable fonts
- [WCAG Contrast Checker](https://webaim.org/resources/contrastchecker/) — accessibility tool

## Next Steps

- [Colors & Backgrounds](colors-and-backgrounds.md) — color and background styling