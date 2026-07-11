# BEM & CSS Architecture

## Description

CSS architecture is how you organize styles at scale. Naming conventions (BEM), cascade layers, CSS modules, utility-first frameworks, and preprocessors each solve different scaling problems.

## Prerequisites

- [Cascade, Specificity & Inheritance](cascade-and-specificity.md) — the cascade problem
- [Custom Properties](custom-properties.md) — design tokens and theming

## Table of Contents

- [The Scaling Problem](#the-scaling-problem)
- [BEM Naming Convention](#bem-naming-convention)
- [BEM Variations](#bem-variations)
- [Cascade Layers](#cascade-layers)
- [CSS Scope](#css-scope)
- [CSS Modules](#css-modules)
- [Utility-First CSS](#utility-first-css)
- [CSS Preprocessors](#css-preprocessors)
- [Design Systems and Tokens](#design-systems-and-tokens)
- [Component CSS](#component-css)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Scaling Problem

As projects grow, CSS without architecture leads to:
- Specificity wars (adding selectors to override other selectors)
- Dead code (styles that no longer apply)
- Non-obvious side effects (changing one class breaks another)
- Large bundles (unused CSS shipped to users)

Solutions vary by scale:
- **Small projects (1–5 pages):** Semantic classes, descriptive names
- **Medium projects (5–50 pages):** BEM naming or CSS Modules
- **Large projects (50+ pages, multiple teams):** Design system + utility framework + CSS Modules

### BEM Naming Convention

BEM stands for Block, Element, Modifier:

```
.block {}
.block__element {}
.block--modifier {}
.block__element--modifier {}
```

**Block** — standalone component:

```css
.card {}
.card__title {}
.card__body {}
.card__image {}
```

**Element** — part of a block (double underscore):

```html
<div class="card">
    <img class="card__image" src="...">
    <h2 class="card__title">Title</h2>
    <p class="card__body">Content</p>
</div>
```

**Modifier** — variation of a block or element (double hyphen):

```html
<div class="card card--featured">
    <h2 class="card__title card__title--large">Title</h2>
</div>
```

```css
.card { }
.card--featured { border-color: gold; }
.card__title { }
.card__title--large { font-size: 1.5rem; }
```

BEM rules:
- Blocks are independent — they can be moved anywhere
- Elements have meaning only inside their block
- Modifiers change appearance, not structure
- No deep nesting — `.card__wrapper__title` is wrong (flatten to `.card__title`)

### BEM Variations

**No deep nesting:** BEM intentionally has only two levels (block + element). Use a new block for nested components:

```html
<div class="card">
    <div class="card__header">
        <div class="user-info">   <!-- New block -->
            <img class="user-info__avatar" src="">
            <span class="user-info__name">Alice</span>
        </div>
    </div>
</div>
```

**SMACSS-like** — use namespace prefixes for categorization:

```css
.l-grid {}              /* Layout */
.c-card {}              /* Component */
.u-hidden {}            /* Utility */
.is-active {}           /* State */
.js-modal-toggle {}     /* JavaScript hook (no styles) */
```

**Atomic CSS** — single-purpose classes:

```css
.mt-2 { margin-top: 0.5rem; }
.p-4 { padding: 1rem; }
.text-center { text-align: center; }
.flex { display: flex; }
```

### Cascade Layers

`@layer` gives authors explicit control over priority within their own stylesheet:

```css
/* Define layer order (lowest to highest priority) */
@layer reset, base, components, utilities;

/* Layer contents */
@layer reset {
    *, *::before, *::after { box-sizing: border-box; margin: 0; }
}

@layer base {
    body { font-family: 'Inter', sans-serif; color: #1a1a1a; }
    h1 { font-size: 2rem; }
}

@layer components {
    .card { padding: 1.5rem; border: 1px solid #ddd; border-radius: 8px; }
    .button { padding: 0.5rem 1rem; background: #0066cc; color: white; }
}

@layer utilities {
    .hidden { display: none; }
    .sr-only { /* ... */ }
}
```

Layers solve the specificity arms race — styles in `utilities` always beat `components`, even if component selectors have higher specificity:

```css
@layer components {
    #sidebar .card a { color: blue; }    /* Very specific */
}

@layer utilities {
    .text-red { color: red !important; }  /* Without layer, need !important */
}

/* With layers, .text-red wins even without !important */
```

Layer features:
- Define layer order once at the top
- Unlayered styles beat all layered styles (like a final implicit layer)
- `!important` reverses layer order (earlier layers win)
- Third-party CSS can be imported into a low-priority layer

### CSS Scope

The `@scope` rule (2024+) limits styles to a DOM subtree:

```css
@scope (.card) {
    /* Only within .card */
    h2 { font-size: 1.25rem; }
    p { color: #666; }
}

@scope (.card) to (.card-footer) {
    /* Apply within .card, but stop at .card-footer */
    p { margin-bottom: 0.5rem; }
}
```

Scope advantages:
- Encapsulation without Shadow DOM
- No specificity increase from scoping
- Lighter-weight than Shadow DOM for component isolation

### CSS Modules

CSS Modules are build-time scoped class names:

```css
/* Component.module.css */
.card { padding: 1rem; }
.title { font-size: 1.25rem; }
```

```javascript
// React component
import styles from './Component.module.css';

function Card() {
    return (
        <div className={styles.card}>
            <h2 className={styles.title}>Title</h2>
        </div>
    );
}
```

The build tool generates unique class names:

```html
<div class="Card_card_1a2b3">
    <h2 class="Card_title_4d5e6">Title</h2>
</div>
```

Benefits:
- True scoping — class names cannot conflict
- Dead code elimination (CSS that is not imported is not bundled)
- Co-located styles with components

### Utility-First CSS

Utility-first CSS (Tailwind CSS) uses small, single-purpose classes composed in HTML:

```html
<div class="p-6 max-w-sm mx-auto bg-white rounded-xl shadow-md">
    <h2 class="text-xl font-bold text-gray-900">Card Title</h2>
    <p class="text-gray-500">Card content goes here.</p>
</div>
```

**Pros:**
- No naming decisions
- No unused CSS (purging)
- Consistent design via design tokens
- Rapid prototyping

**Cons:**
- Verbose HTML
- Learning curve for the utility vocabulary
- Design changes require HTML changes
- Can be inaccessible without semantic structure

**Hybrid approach — utilities + components:**

```html
<div class="card">
    <h2 class="card__title text-lg font-bold">Title</h2>
    <p class="text-gray-500">Content</p>
</div>
```

### CSS Preprocessors

Sass (SCSS) is the most common CSS preprocessor:

**Variables (replaced by custom properties):**

```scss
// Sass variables — compile-time
$primary: #0066cc;
$spacing-unit: 0.25rem;

// Modern alternative — use CSS custom properties
```

**Nesting:**

```scss
.card {
    padding: 1rem;

    .title {
        font-size: 1.25rem;       // → .card .title
    }

    &--featured {
        border-color: gold;       // → .card--featured
    }

    &:hover {
        box-shadow: 0 2px 8px;   // → .card:hover
    }
}
```

Native CSS nesting (2024+) is replacing the need for preprocessor nesting.

**Mixins:**

```scss
@mixin respond-above($bp) {
    @media (min-width: $bp) {
        @content;
    }
}

.card {
    padding: 1rem;

    @include respond-above(768px) {
        padding: 2rem;
    }
}
```

Modern CSS with `clamp()` and container queries reduce the need for mixins.

**Extend / Inheritance:**

```scss
%button-base {
    padding: 0.5rem 1rem;
    border-radius: 6px;
    font: inherit;
}

.btn-primary {
    @extend %button-base;
    background: #0066cc;
    color: white;
}
```

**When to use preprocessors in 2024+:**
- Legacy projects that already use them
- Teams that prefer SCSS syntax
- Specific features not yet in CSS (color functions, math)

Otherwise, modern CSS covers most preprocessor features.

### Design Systems and Tokens

A design system with CSS custom properties:

```css
:root {
    /* Raw tokens */
    --color-blue-500: #0066cc;
    --color-gray-100: #f3f4f6;
    --color-gray-900: #111827;
    --space-4: 1rem;
    --space-6: 1.5rem;

    /* Semantic tokens */
    --color-primary: var(--color-blue-500);
    --color-surface: white;
    --color-text: var(--color-gray-900);
    --color-text-secondary: #6b7280;
    --card-padding: var(--space-6);
    --card-radius: 8px;
}
```

Then component styles reference semantic tokens:

```css
.card {
    padding: var(--card-padding);
    border-radius: var(--card-radius);
    background: var(--color-surface);
    color: var(--color-text);
    border: 1px solid var(--color-border);
}
```

### Component CSS

Pattern for self-contained component styles:

```css
/* Component: Card */

/* Container block */
.card {
    --card-padding: 1.5rem;
    --card-radius: 8px;
    --card-border-color: #e0e0e0;

    padding: var(--card-padding);
    border-radius: var(--card-radius);
    border: 1px solid var(--card-border-color);
    background: var(--card-bg, white);
}

/* Elements */
.card__title {
    font-size: 1.25rem;
    font-weight: 600;
    margin-bottom: 0.5rem;
}

.card__body {
    color: #666;
    line-height: 1.6;
}

.card__image {
    width: 100%;
    border-radius: calc(var(--card-radius) - 2px);
}

/* Modifier */
.card--featured {
    --card-border-color: gold;
}
```

## Glossary

| Term | Definition |
|---|---|
| BEM | Block-Element-Modifier naming convention |
| Cascade layer | Author-defined priority layer within the cascade |
| CSS Module | Build-time scoped CSS class names |
| Utility-first | CSS approach using single-purpose classes |
| Design token | Named value representing a design decision |
| CSS Scope | Zone-limited style application |
| Mixin | Reusable CSS block (Sass) |
| Dead code elimination | Removing unused CSS via build tools |

## Quick References

- [BEM Documentation](https://en.bem.info/methodology/css/) — naming convention guide
- [MDN: @layer](https://developer.mozilla.org/en-US/docs/Web/CSS/@layer) — cascade layer reference
- [MDN: @scope](https://developer.mozilla.org/en-US/docs/Web/CSS/@scope) — scope reference
- [Tailwind CSS](https://tailwindcss.com/docs) — utility-first framework
- [CSS Modules Docs](https://github.com/css-modules/css-modules) — specification
- [Sass Documentation](https://sass-lang.com/documentation/) — preprocessor guide

## Next Steps

- [CSS Tooling & Performance](tooling-and-performance.md) — build tools, linting, debugging