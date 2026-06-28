# CSS Custom Properties (Variables)

## Description

Custom properties bring variables to CSS. They hold values that can be reused, computed, and dynamically changed — enabling theming, responsive components, and reduced repetition.

## Prerequisites

- [Values & Units](values-and-units.md) — CSS value types
- [Cascade, Specificity & Inheritance](cascade-and-specificity.md) — how property values resolve

## Table of Contents

- [Defining Custom Properties](#defining-custom-properties)
- [Using var()](#using-var)
- [Fallback Values](#fallback-values)
- [Scope and Inheritance](#scope-and-inheritance)
- [Dynamic Changes](#dynamic-changes)
- [Custom Properties in Media Queries](#custom-properties-in-media-queries)
- [Custom Properties in calc()](#custom-properties-in-calc)
- [Type Checking with @property](#type-checking-with-property)
- [Performance Considerations](#performance-considerations)
- [Theming Patterns](#theming-patterns)
- [Design Tokens](#design-tokens)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Defining Custom Properties

Custom properties are defined with a double-hyphen prefix (`--`):

```css
:root {
    --primary-color: #0066cc;
    --spacing-unit: 0.25rem;
    --font-sans: 'Inter', system-ui, sans-serif;
    --border-radius: 8px;
    --transition-fast: 150ms ease-in-out;
}
```

Property names are case-sensitive:
```css
:root {
    --color: red;      /* Different from */
    --Color: blue;     /* This one */
}
```

Custom properties can hold any CSS value, including multi-value shorthand:

```css
:root {
    --shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    --gradient: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    --font-stack: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
    --size: 1rem 1.5rem;
}
```

### Using var()

Custom properties are consumed via the `var()` function:

```css
.button {
    background: var(--primary-color);
    padding: var(--spacing-unit);
    font-family: var(--font-sans);
    border-radius: var(--border-radius, 4px);  /* With fallback */
}
```

You can nest `var()` calls:

```css
:root {
    --base-color: #333;
    --text-color: var(--base-color);
}

p {
    color: var(--text-color);  /* Resolves to #333 */
}
```

### Fallback Values

The `var()` function accepts a fallback as second argument:

```css
.element {
    color: var(--undefined-variable, red);            /* Falls back to red */
    background: var(--bg, var(--default-bg, white));  /* Nested fallback */
}
```

The fallback is a literal CSS value, not a second custom property reference (but you can nest `var()`):

```css
/* Fallback is treated as-is */
margin: var(--margin, 1rem);        /* Fallback: 1rem */

/* To use a different custom property as fallback, nest var() */
margin: var(--margin, var(--default-margin, 1rem));
```

Fallbacks are only used when the custom property is not defined (not when it is invalid). If the custom property is defined but has an invalid value for the property, the property gets its initial value instead — the fallback is ignored.

```css
:root {
    --bad-value: 1px solid;     /* Invalid for color */
}

.element {
    color: var(--bad-value, red);   /* Ignored — var() resolves to 1px solid */
    /* color: 1px solid is invalid — browser uses initial value (depends on element) */
}
```

### Scope and Inheritance

Custom properties inherit by default. You define them at different scope levels:

```css
/* Root level — available everywhere */
:root {
    --text-color: #333;
}

/* Container level — available inside .card only */
.card {
    --card-padding: 1.5rem;
    --card-bg: white;
}

.card__title {
    color: var(--text-color);       /* Inherited from root */
    padding: var(--card-padding);   /* Inherited from .card parent */
}

/* Outside .card, --card-padding is undefined */
body {
    padding: var(--card-padding, 0);  /* Uses fallback 0 */
}
```

Scoping allows component-local variables without affecting the global scope:

```css
.alert {
    --alert-bg: #fff3cd;
    --alert-border: #ffc107;
    --alert-color: #856404;
    background: var(--alert-bg);
    border: 1px solid var(--alert-border);
    color: var(--alert-color);
}

.alert--error {
    --alert-bg: #f8d7da;
    --alert-border: #f5c6cb;
    --alert-color: #721c24;
}
/* The --error variant overrides only the custom properties, properties are inherited */
```

### Dynamic Changes

The power of custom properties is runtime reactivity. Change a custom property, and everything using it updates:

```css
.button {
    --bg: #0066cc;
    background: var(--bg);
    transition: background 0.2s;
}

.button:hover {
    --bg: #0052a3;          /* Changes background automatically */
}

/* Using JavaScript */
/*
 * document.querySelector('.theme-toggle').addEventListener('click', () => {
 *   document.documentElement.style.setProperty('--primary', '#7c3aed');
 * });
 */
```

State-based theming without redefining all properties:

```css
.card {
    --border-color: #e0e0e0;
    border: 1px solid var(--border-color);
}

.card[data-state="active"] {
    --border-color: #0066cc;    /* Just update the variable */
}

.card[data-state="error"] {
    --border-color: #dc3545;
}
```

### Custom Properties in Media Queries

Custom properties can be set inside media queries:

```css
:root {
    --columns: 4;
    --gap: 1.5rem;
    --font-size: 1rem;
}

@media (max-width: 768px) {
    :root {
        --columns: 2;
        --gap: 1rem;
        --font-size: 0.875rem;
    }
}

@media (max-width: 480px) {
    :root {
        --columns: 1;
        --gap: 0.75rem;
    }
}

.grid {
    display: grid;
    grid-template-columns: repeat(var(--columns), 1fr);
    gap: var(--gap);
}
```

This pattern centralizes breakpoint logic into variable values instead of scattering it.

**Important limitation:** Custom properties cannot be used inside `@media` query conditions themselves:

```css
/* THIS DOES NOT WORK */
:root {
    --breakpoint: 768px;
}

@media (min-width: var(--breakpoint)) { }  /* Invalid */
```

Media query conditions must be literal values — custom properties are not resolved there.

### Custom Properties in calc()

Custom properties work seamlessly with `calc()`:

```css
:root {
    --spacing: 1rem;
    --columns: 3;
}

.element {
    padding: calc(var(--spacing) * 2);
    width: calc((100% - var(--spacing) * 2) / var(--columns));
}
```

**Important type gotcha:** Custom properties are text-substituted. If the variable contains a unitless number, arithmetic works:

```css
:root {
    --multiplier: 2;
    --font-base: 1rem;
}

.element {
    font-size: calc(var(--font-base) * var(--multiplier));  /* 2rem — works */
}
```

But concatenation does not work:

```css
:root {
    --value: 2;
}

.element {
    /* THIS DOES NOT WORK */
    font-size: calc(1rem * var(--value)px);         /* Invalid */
    font-size: calc(1rem * (var(--value) * 1px));   /* Also invalid */

    /* Correct way — the unit must be inside var or explicitly multiplied */
    font-size: calc(1rem * var(--multiplier));
}
```

To add units, include them in the variable or use `@property` for type checking.

### Type Checking with @property

The `@property` rule (2024+) defines custom property types, initial values, and inheritance behavior:

```css
@property --spacing {
    syntax: "<length>";
    initial-value: 0px;
    inherits: true;
}

@property --primary-color {
    syntax: "<color>";
    initial-value: #0066cc;
    inherits: true;
}

@property --columns {
    syntax: "<number>";
    initial-value: 4;
    inherits: false;
}

@property --shadow {
    syntax: "<shadow>";
    initial-value: 0 2px 4px rgba(0, 0, 0, 0.1);
    inherits: true;
}
```

Benefits of `@property`:
- Type validation — invalid values are rejected, not silently treated as unset.
- Animatable — typed custom properties can transition and animate.
- Default values — no need to check `var(--x, fallback)` everywhere.
- Inherits control — prevent inheritance explicitly.

```css
/* Before @property — custom properties cannot transition */
:root {
    --bg-color: white;
}

.element {
    background: var(--bg-color);
    transition: --bg-color 0.3s;    /* Does NOT work without @property */
}

/* After @property */
@property --bg-color {
    syntax: "<color>";
    initial-value: white;
    inherits: true;
}

.element {
    background: var(--bg-color);
    transition: --bg-color 0.3s;    /* Now animates! */
}

.element:hover {
    --bg-color: #0066cc;
}
```

Supported syntax types:
- `<length>`, `<number>`, `<percentage>`, `<length-percentage>`
- `<color>`, `<angle>`, `<time>`, `<resolution>`
- `<string>`, `<url>`, `<integer>`
- `<custom-ident>`, `<transform-function>`, `<shadow>`, `<image>`
- `*` (any value, default for untyped vars)
- `+` (space-separated list), `#` (comma-separated list), `|` (union, e.g., `<length> | <percentage>`)

### Performance Considerations

Custom properties have some performance impact — they are resolved by the browser at computed-value time:

```css
/* Slightly slower — each use of var() requires resolution */
* {
    color: var(--text-color, black);
}

/* Faster — direct value */
* {
    color: black;
}
```

Impact is negligible in most cases. The benefits of maintainability far outweigh the cost. Avoid using custom properties on hundreds of thousands of elements (e.g., inside a loop of a list with 50k items).

**Painting:** Changing a custom property triggers style recalc for all elements referencing it. Limit the scope of variable changes to the smallest subtree possible.

### Theming Patterns

**Light/Dark theme with custom properties:**

```css
:root {
    /* Light theme (default) */
    --bg: #ffffff;
    --text: #1a1a1a;
    --surface: #f5f5f5;
    --border: #e0e0e0;
    --primary: #0066cc;
    --primary-text: #ffffff;
}

[data-theme="dark"] {
    --bg: #1a1a1a;
    --text: #e0e0e0;
    --surface: #2d2d2d;
    --border: #404040;
    --primary: #66b3ff;
    --primary-text: #1a1a1a;
}

body {
    background: var(--bg);
    color: var(--text);
}

.card {
    background: var(--surface);
    border: 1px solid var(--border);
}
```

To persist the theme:

```javascript
// Apply theme based on system preference
const prefersDark = window.matchMedia('(prefers-color-scheme: dark)');
document.documentElement.dataset.theme = prefersDark.matches ? 'dark' : 'light';

prefersDark.addEventListener('change', (e) => {
    document.documentElement.dataset.theme = e.matches ? 'dark' : 'light';
});
```

**Color palette as custom properties:**

```css
:root {
    --gray-50: #f9fafb;
    --gray-100: #f3f4f6;
    --gray-200: #e5e7eb;
    --gray-300: #d1d5db;
    --gray-400: #9ca3af;
    --gray-500: #6b7280;
    --gray-600: #4b5563;
    --gray-700: #374151;
    --gray-800: #1f2937;
    --gray-900: #111827;
}
```

### Design Tokens

Design tokens are named values representing design decisions. Custom properties are the CSS implementation of tokens:

```css
:root {
    /* Spacing scale */
    --space-1: 0.25rem;
    --space-2: 0.5rem;
    --space-3: 0.75rem;
    --space-4: 1rem;
    --space-6: 1.5rem;
    --space-8: 2rem;
    --space-12: 3rem;
    --space-16: 4rem;

    /* Typography scale */
    --text-xs: 0.75rem;
    --text-sm: 0.875rem;
    --text-base: 1rem;
    --text-lg: 1.125rem;
    --text-xl: 1.25rem;
    --text-2xl: 1.5rem;
    --text-3xl: 1.875rem;

    /* Semantic tokens */
    --color-text-primary: var(--gray-900);
    --color-text-secondary: var(--gray-500);
    --color-text-disabled: var(--gray-300);
    --color-bg-primary: white;
    --color-bg-secondary: var(--gray-50);
    --color-border: var(--gray-200);

    /* Component tokens */
    --button-padding-x: var(--space-4);
    --button-padding-y: var(--space-2);
    --button-font-size: var(--text-sm);
    --button-border-radius: 6px;
}
```

This three-layer system (raw values → semantic → component) makes systematic design scalable.

## Study Cases

### Component Variants Without Sprawl

Before custom properties — three button variants require redefining everything:

```css
.btn { padding: 0.5rem 1rem; border-radius: 6px; font-size: 0.875rem; }
.btn-primary { background: #0066cc; color: white; border: none; }
.btn-secondary { background: #6c757d; color: white; border: none; }
.btn-outline { background: transparent; color: #0066cc; border: 1px solid #0066cc; }
```

After custom properties — one unified button:

```css
.btn {
    /* Defaults */
    --btn-bg: #0066cc;
    --btn-color: white;
    --btn-border-color: transparent;
    --btn-border-width: 0;

    padding: 0.5rem 1rem;
    border-radius: 6px;
    font-size: 0.875rem;
    background: var(--btn-bg);
    color: var(--btn-color);
    border: var(--btn-border-width) solid var(--btn-border-color);
}

.btn--secondary {
    --btn-bg: #6c757d;
}

.btn--outline {
    --btn-bg: transparent;
    --btn-color: #0066cc;
    --btn-border-color: #0066cc;
    --btn-border-width: 1px;
}
```

### Color with Opacity Fallback

Before OKLCH and relative color syntax, custom properties made opacity variants manageable:

```css
:root {
    --primary: 0, 102, 204;    /* Store as raw RGB components */
}

.element {
    /* Use with opacity via rgb() */
    background: rgb(var(--primary) / 0.1);
    border: 1px solid rgb(var(--primary) / 0.3);
    color: rgb(var(--primary));
}
```

Now with OKLCH and relative colors, the pattern is cleaner:

```css
:root {
    --primary: oklch(0.5 0.2 250deg);
    --primary-100: oklch(from var(--primary) 0.9 0.05 h);
    --primary-200: oklch(from var(--primary) 0.8 0.1 h);
    --primary-300: oklch(from var(--primary) 0.7 0.15 h);
}
```

## Examples

### Example 1: Fluid spacing scale

```css
:root {
    --fluid-ratio: clamp(0.75rem, 2vw, 1.5rem);
    --space-sm: calc(var(--fluid-ratio) * 0.5);
    --space-md: var(--fluid-ratio);
    --space-lg: calc(var(--fluid-ratio) * 2);
    --space-xl: calc(var(--fluid-ratio) * 4);
}
```

### Example 2: Theme with accent colors

```css
:root {
    --accent-hue: 210;                     /* Blue default */
    --accent: oklch(0.5 0.2 var(--accent-hue));
    --accent-light: oklch(0.9 0.05 var(--accent-hue));
}

[data-accent="green"] { --accent-hue: 150; }
[data-accent="purple"] { --accent-hue: 270; }
[data-accent="orange"] { --accent-hue: 30; }
```

### Example 3: Transition on variable change

```css
@property --surface-color {
    syntax: "<color>";
    initial-value: white;
    inherits: true;
}

.card {
    background: var(--surface-color);
    transition: --surface-color 0.3s;
}

.card:hover {
    --surface-color: #f0f4ff;
}
```

## Glossary

| Term | Definition |
|---|---|
| Custom property | User-defined CSS property with `--` prefix |
| var() | Function to retrieve a custom property value |
| Fallback | Default value used when custom property is undefined |
| Design token | Named design decision value |
| Semantic token | Token describing purpose rather than value |
| @property | Rule defining custom property type and behavior |
| Component token | Token scoped to a component |

## Quick References

- [MDN: Custom Properties](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties) — comprehensive guide
- [MDN: @property](https://developer.mozilla.org/en-US/docs/Web/CSS/@property) — typed custom properties
- [CSS Custom Properties Spec](https://www.w3.org/TR/css-variables-1/) — W3C specification
- [Caniuse: @property](https://caniuse.com/mdn-css_at-rules_property) — browser support
- [Design Tokens Format](https://design-tokens.github.io/community-group/format/) — DTCG standard for tokens

## Next Steps

- [CSS Functions](css-functions.md) — the complete CSS function landscape