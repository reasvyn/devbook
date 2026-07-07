# Cascade, Specificity & Inheritance

## Description

The cascade resolves conflicts when multiple rules target the same element. Understanding specificity, inheritance, and the cascade origin is essential for predictable CSS.

## Prerequisites

- [CSS Selectors](selectors.md) — specificity basics
- [What Is CSS?](intro/what-is-css.md) — how CSS applies to HTML

## Table of Contents

- [What Is the Cascade?](#what-is-the-cascade)
- [Cascade Origins](#cascade-origins)
- [Specificity Recap](#specificity-recap)
- [Order of Appearance](#order-of-appearance)
- [Inline Styles](#inline-styles)
- [The !important Exception](#the-important-exception)
- [Inheritance](#inheritance)
- [The initial, inherit, unset, revert Keywords](#the-initial-inherit-unset-revert-keywords)
- [The all Shorthand](#the-all-shorthand)
- [Cascade Layers](#cascade-layers)
- [Scope](#scope)
- [Source Order Strategies](#source-order-strategies)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is the Cascade?

The cascade is the algorithm that determines which CSS declaration wins for each element and property. When multiple declarations apply to the same element, the cascade picks one using these criteria (in order):

1. **Origin and importance** — where the rule comes from and whether it has `!important`
2. **Context** — inline vs. external, layer membership
3. **Specificity** — the selector's specificity score
4. **Order of appearance** — later rules override earlier ones

```css
/* Both rules target h1 — which wins? */
h1 { color: blue; }
h1 { color: red; }       /* Wins by order of appearance (same specificity, same origin) */
```

The cascade makes CSS predictable. Without it, conflicts would be undefined.

### Cascade Origins

The browser assigns an origin to every declaration. Origins have a built-in priority:

**Lowest priority → Highest priority (normal):**

| Origin | Example | Priority |
|---|---|---|
| User-agent (browser defaults) | `h1 { display: block; font-size: 2em; }` | Lowest |
| User (browser preferences) | User-set font size in browser settings | Middle |
| Author (your CSS) | Your stylesheets, `<style>` blocks | Highest |

**With `!important` the order reverses:**

| Origin (important) | Priority |
|---|---|
| Author `!important` | Lowest |
| User `!important` | Middle |
| User-agent `!important` | Highest |

```css
/* Browser default */
h1 { font-size: 2em; }              /* 1.5em — applying user-agent default */

/* Author styles override */
h1 { font-size: 1.5rem; }           /* Wins over user-agent for normal declarations */
```

User-agent styles are the reason `<h1>` is larger than `<p>` without any CSS.

### Specificity Recap

Specificity is the third criterion in the cascade. It only matters when origins and contexts are equal:

```css
/* Two author-origin declarations — specificity decides */
.special { color: green; }    /* 0,0,1,0 */
#hero { color: blue; }        /* 0,1,0,0 — wins */
```

When specificity is equal, order of appearance breaks the tie.

### Order of Appearance

When all other criteria are equal, the declaration that appears later wins:

```css
/* Same specificity, same origin — later wins */
.button { background: blue; }
.button { background: red; }       /* red wins — appears later */
```

Important rules about order:

- **Stylesheet order matters.** A `<link>` loaded later overrides an earlier one with equal specificity.
- **`<style>` blocks** interact with external stylesheets based on their position in HTML.
- **Linked stylesheets** are treated in document order.

```html
<!-- Later overrides earlier -->
<link rel="stylesheet" href="base.css">
<link rel="stylesheet" href="theme.css">  <!-- Wins in ties -->
<style>
    /* Appears after both links — wins ties against both */
    .button { background: purple; }
</style>
```

### Inline Styles

Inline styles (added via the `style` attribute) beat all author-origin declarations unless `!important` is involved:

```html
<div style="color: red;">       <!-- Wins over #sidebar .text p { color: blue; } -->
```

```css
/* Inline style already has color: red — this does not override it */
#sidebar .text p { color: blue; }   /* Loses to inline */
```

Inline styles have a specificity of `1,0,0,0` in the original algorithm. They are part of the author origin but have higher priority than selector-based declarations.

### The !important Exception

`!important` changes the cascade origins:

```css
/* Normal cascade */
#sidebar a { color: blue; }           /* High specificity, but... */
a { color: red !important; }          /* Wins — !important overrides specificity */
```

When multiple `!important` declarations apply, specificity and order still matter:

```css
/* Both are !important — specificity decides */
.special { color: red !important; }   /* 0,0,1,0 */
#sidebar .special { color: blue !important; }  /* 0,1,1,0 — wins */
```

**When to use `!important` (very rare):**

- Utility classes: `.hidden { display: none !important; }`
- Overriding inline styles you cannot change
- User stylesheets for accessibility

**When NOT to use `!important`:**

- In component CSS that should be overridable
- As a shortcut to fix specificity issues (fix the selector instead)
- In libraries or design systems (let consumers override)

### Inheritance

Inheritance automatically passes property values from parent to child. Not all properties inherit:

**Inherited by default:**

```css
body {
    color: #333;            /* Inherited — all text inside body gets this color */
    font-family: 'Inter', sans-serif;  /* Inherited */
    font-size: 16px;        /* Inherited */
    line-height: 1.6;       /* Inherited */
    text-align: left;       /* Inherited */
}
```

**Not inherited by default:**

```css
.container {
    margin: 2rem;           /* Not inherited */
    padding: 1rem;          /* Not inherited */
    border: 1px solid #ddd; /* Not inherited */
    background: #f8f8f8;    /* Not inherited */
    width: 100%;            /* Not inherited */
    height: auto;           /* Not inherited */
    display: flex;          /* Not inherited */
}
```

Inheritance creates a "cascade" of typographic values from `html` → `body` → `div` → `p` → `span`. This means setting `font-size` on `html` affects everything (except elements with explicit overrides).

The `inherit` keyword forces inheritance for any property:

```css
button {
    /* Normally buttons have user-agent font — force inheritance */
    font-family: inherit;
    font-size: inherit;
    color: inherit;
}
```

This is the standard button reset pattern.

### The initial, inherit, unset, revert Keywords

These four keywords give precise control over property values:

**`initial`** — sets the property to its CSS specification default:

```css
div { display: initial; }   /* Resets to 'inline' — the default for div... wait */
```

`initial` uses the CSS spec default, which may not be what you expect. For `display`, the CSS spec default is `inline`, not `block`.

**`inherit`** — forces the property to inherit from parent, even if it normally does not:

```css
a { color: inherit; }       /* Removes link styling — use parent's color */
p { border: inherit; }      /* p normally does not inherit border — now it does */
```

**`unset`** — acts as `inherit` if the property is inherited, `initial` if not:

```css
a { all: unset; }           /* Resets all properties to natural inheritance behavior */
```

**`revert`** — resets to the value the property would have had without author styles:

```css
h1 { font-size: revert; }   /* Goes back to user-agent style, not to initial */
```

`revert` is useful in component CSS to "undo" a global reset without re-adding browser defaults.

**`revert-layer`** — rolls back to the previous cascade layer value (for Cascade Layers).

### The all Shorthand

The `all` property resets every property at once:

```css
.reset {
    all: initial;   /* All properties to CSS spec defaults */
}

.reset-better {
    all: unset;     /* Inherit for inherited props, initial for non-inherited */
}

.reset-with-browser {
    all: revert;    /* Back to browser defaults */
}
```

Use `all: unset` for complete resets inside a component. Combine with `inherit` for properties you want to control:

```css
.my-component {
    all: unset;
    display: block;     /* Restore block display */
    font-family: inherit;  /* Inherit from parent */
    font-size: inherit;
    color: inherit;
}
```

### Cascade Layers

Cascade Layers (`@layer`) give authors control over origin priority within their own CSS:

```css
/* Define layers in order — base has lowest priority */
@layer base, components, utilities;

/* Layer contents */
@layer base {
    h1 { font-size: 2rem; }
    a { color: blue; }
}

@layer components {
    .card { padding: 1rem; border: 1px solid #ddd; }
}

@layer utilities {
    .hidden { display: none; }
    .text-center { text-align: center; }
}

/* Later layers override earlier layers regardless of specificity */
@layer base {
    h1 { font-size: 3rem; }    /* 0,0,0,1 */
}

@layer utilities {
    h1 { font-size: 2rem; }    /* 0,0,0,1 — wins (later layer) */
}
```

Layer rules:
- Define layer order first, then fill them.
- Unlayered styles beat all layered styles (as if in a final implicit layer).
- `!important` reverses layer priority (earlier layers win).
- Layers normalize specificity between authors and third-party code (put third-party in a lower layer).

### Scope

The `@scope` rule (2024+) limits rules to a subtree:

```css
/* Styles apply only within elements matching .card */
@scope (.card) {
    h2 { font-size: 1.25rem; }
    p { color: #666; }
}

/* With scoping limits */
@scope (.card) to (.card-footer) {
    /* Applies to .card descendants, but stops at .card-footer */
    p { margin: 0; }
}
```

Scope helps with component isolation without Shadow DOM.

### Interaction with Container Queries

Container queries (`@container`) add a new dimension to the cascade. They allow styles to respond to the size of a parent container rather than the viewport:

```css
/* Define a container */
.card-container {
    container-type: inline-size;
    container-name: card;
}

/* Query the container's size */
@container card (min-width: 400px) {
    .card {
        display: grid;
        grid-template-columns: 1fr 2fr;
    }
}

/* Narrower container — stack vertically */
.card {
    display: flex;
    flex-direction: column;
}
```

Container queries interact with the cascade like media queries — they do not change specificity or origin. A container query style has the same specificity as its selector, but only applies when the container condition is met. This allows fine-grained responsive design without increasing specificity battles.

### The :has() Selector and Cascade Impact

The `:has()` relational pseudo-class (2023+) selects elements based on their descendants:

```css
/* Select cards that contain an image */
.card:has(img) {
    grid-template-rows: auto 1fr;
}

/* Style a form differently when it has errors */
form:has(.error) {
    border-color: red;
}

/* Select a parent based on child state — previously impossible */
li:has(> input:checked) {
    background: #e8f5e9;
}
```

`:has()` does not increase specificity beyond the sum of its parts. A selector like `.card:has(img)` has specificity `(0, 1, 1)` — one class plus one pseudo-class. This is higher than `.card` alone but lower than `#card`. Understanding this helps avoid cascade conflicts when using `:has()`.

### Nesting and the Cascade

CSS nesting (2023+) provides syntactic sugar for grouping related rules but does not affect the cascade:

```css
/* Nested syntax — purely syntactic, same cascade behavior */
.card {
    background: white;

    & .title {
        font-size: 1.25rem;    /* Same as .card .title */
    }

    & .body {
        color: #666;            /* Same as .card .body */
    }
}

/* Equivalent without nesting — identical specificity and cascade */
.card { background: white; }
.card .title { font-size: 1.25rem; }
.card .body { color: #666; }
```

The nesting selector `&` does not add specificity. Nested rules have the same specificity as the equivalent un-nested selectors. The cascade treats them identically — only the source order differs.

### Source Order Strategies

Practical strategies for managing cascade order:

**1. CSS file organization:**
```css
/* 1. Reset / normalize — lowest priority */
/* 2. Base / typography */
/* 3. Layout / grid */
/* 4. Components */
/* 5. Utilities / overrides — highest priority */
```

**2. One class per element:**
```css
/* Avoid deep specificity chains */
/* Prefer: */
.card__title { }     /* One class */
.card__body { }      /* One class */

/* Over: */
.card .title { }              /* Descendant selector */
.card > div > p { }          /* Deep chain */
div.card div.body p { }      /* Overqualified */
```

**3. Do not use IDs in CSS selectors:**
```css
/* Avoid */
#sidebar .widget a { }

/* Prefer */
.widget-link { }
```

IDs create high specificity that is hard to override.

## Glossary

| Term | Definition |
|---|---|
| Cascade | Algorithm resolving conflicting CSS declarations |
| Origin | Source of a CSS declaration (user-agent, user, author) |
| Specificity | Selector weight determining priority |
| Inheritance | Automatic property value transfer from parent to child |
| `initial` | CSS specification default value for a property |
| `inherit` | Forces the parent's computed value for any property |
| `unset` | Inherits if inherited property, else initial |
| `revert` | Resets to value without author styles |
| Cascade Layer | Author-controlled priority layer within the cascade |
| `!important` | Declaration flag that reverses origin priority |

## Quick References

- [MDN: Cascade](https://developer.mozilla.org/en-US/docs/Web/CSS/Cascade) — MDN guide to the cascade
- [MDN: Specificity](https://developer.mozilla.org/en-US/docs/Web/CSS/Specificity) — specificity deep dive
- [CSS Cascading and Inheritance Level 5](https://www.w3.org/TR/css-cascade-5/) — official specification
- [Specificity Calculator](https://specificity.keegan.st/) — interactive calculator
- [Cascade Layers Guide](https://developer.chrome.com/docs/css-ui/cascade-layers) — Chrome guide to `@layer`

## Next Steps

- [The Box Model](box-model.md) — how dimensions and spacing work
- [Values & Units](values-and-units.md) — CSS value types and units