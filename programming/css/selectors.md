# CSS Selectors

## Description

Selectors are patterns that match elements in the DOM, determining which styles apply to which elements. Mastering selectors is the foundation of writing efficient, maintainable CSS.

## Prerequisites

- [What Is CSS?](intro/what-is-css.md) — CSS fundamentals and syntax
- [Elements, Tags & Attributes](../html/elements-and-attributes.md) — HTML element anatomy

## Table of Contents

- [How Selectors Work](#how-selectors-work)
- [Simple Selectors](#simple-selectors)
- [Combinators](#combinators)
- [Pseudo-Classes](#pseudo-classes)
- [Pseudo-Elements](#pseudo-elements)
- [Attribute Selectors](#attribute-selectors)
- [Compound Selectors](#compound-selectors)
- [Selector Lists](#selector-lists)
- [Grouping Selectors](#grouping-selectors)
- [Selector Specificity](#selector-specificity)
- [Performance Considerations](#performance-considerations)
- [The :is() and :where() Selectors](#the-is-and-where-selectors)
- [The :has() Selector](#the-has-selector)
- [The :not() Selector](#the-not-selector)
- [Nesting Selectors](#nesting-selectors)
- [Matching Behavior](#matching-behavior)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### How Selectors Work

A selector is a pattern that the browser matches against DOM nodes. When a match is found, the associated declarations apply to that element.

```css
/* Selector: matches all <p> elements */
p {
    color: #333;
    line-height: 1.6;
}
```

The browser evaluates selectors from right to left (rightmost part is the key selector):

```css
/* Browser reads: "find all <li> inside <ul> with class nav" */
ul.nav > li {
    display: inline-block;
}
```

The browser first finds all `<li>` elements, then checks if their parent matches `ul.nav`. This right-to-left evaluation is why the rightmost selector has the biggest performance impact.

### Simple Selectors

**Type selector** — matches elements by their tag name:

```css
h1 { }      /* All <h1> elements */
div { }     /* All <div> elements */
a { }       /* All <a> elements */
```

**Class selector** — matches elements by class attribute:

```css
.card { }           /* All elements with class="card" */
.highlight { }      /* All elements with class="highlight" */

/* Combined with type */
div.card { }        /* <div> elements with class="card" */
```

**ID selector** — matches a single element by its ID:

```css
#header { }         /* The element with id="header" */
#main-content { }   /* The element with id="main-content" */
```

IDs must be unique per page. An ID selector has the highest specificity among simple selectors.

**Universal selector** — matches all elements:

```css
* { }               /* Every element */
* { box-sizing: border-box; }  /* Apply to everything */
```

Use the universal selector sparingly. Combined with other selectors it can create performance issues on large pages.

### Combinators

Combinators define relationships between selectors:

**Descendant combinator (space)** — matches descendants at any depth:

```css
article p { }       /* All <p> inside <article>, any depth */
div span { }        /* All <span> inside <div>, any depth */
```

**Child combinator (>)** — matches direct children only:

```css
ul > li { }         /* <li> directly inside <ul> */
nav > a { }         /* <a> directly inside <nav> */
```

**Next sibling combinator (+)** — matches the immediately following sibling:

```css
h2 + p { }          /* <p> that directly follows an <h2> */
label + input { }   /* <input> directly after a <label> */
```

**Subsequent sibling combinator (~)** — matches any following sibling:

```css
h2 ~ p { }          /* Any <p> that follows an <h2> at the same level */
.toggle ~ .panel { } /* .panel that follows .toggle */
```

**Column combinator (||)** — matches elements belonging to a table column (rarely used):

```css
col.selected || td { }  /* <td> in the selected column */
```

### Pseudo-Classes

Pseudo-classes select elements based on state, position, or condition. They start with a single colon:

**Dynamic pseudo-classes** — user interaction states:

```css
a:link { }          /* Unvisited link */
a:visited { }       /* Visited link */
a:hover { }         /* Mouse over */
a:active { }        /* Being activated (clicked) */
a:focus { }         /* Has keyboard focus */
a:focus-visible { } /* Focus via keyboard (not mouse click) */
```

Form state pseudo-classes:

```css
input:disabled { }      /* Disabled input */
input:enabled { }       /* Enabled input */
input:checked { }       /* Checked checkbox/radio */
input:required { }      /* Required field */
input:optional { }      /* Optional field */
input:valid { }         /* Passes validation */
input:invalid { }       /* Fails validation */
input:in-range { }      /* Within min/max range */
input:out-of-range { }  /* Outside min/max range */
input:placeholder-shown { }  /* Placeholder is visible */
input:user-valid { }    /* Valid after user interaction */
input:user-invalid { }  /* Invalid after user interaction */
```

**Structural pseudo-classes** — position in the DOM:

```css
li:first-child { }          /* First child of parent */
li:last-child { }           /* Last child of parent */
li:nth-child(2n) { }        /* Even children */
li:nth-child(odd) { }       /* Odd children */
li:nth-child(3) { }         /* Third child */
li:nth-last-child(2) { }    /* Second from last */
li:nth-child(3n+1) { }      /* Every third, starting from 1 */

p:first-of-type { }         /* First <p> in parent */
p:last-of-type { }          /* Last <p> in parent */
p:nth-of-type(2) { }        /* Second <p> in parent */

:root { }                   /* Document root (<html>) */
:empty { }                  /* Elements with no children */
:only-child { }             /* Only child of parent */
```

**Other pseudo-classes:**

```css
:target { }                 /* Element whose ID matches URL hash */
:lang(fr) { }               /* Elements with lang="fr" */
:defined { }                /* Custom elements that have been defined */
:host { }                   /* Shadow DOM host element */
```

### Pseudo-Elements

Pseudo-elements style specific parts of an element. They use double colon syntax (CSS3+):

```css
/* Generated content */
p::before { content: "→ "; }    /* Insert before element content */
p::after { content: ""; }       /* Insert after element content */

/* Text fragments */
p::first-letter { font-size: 2em; }    /* First letter */
p::first-line { font-weight: bold; }   /* First line */

/* Selection */
::selection { background: yellow; }          /* User selection */
::highlight(search) { background: pink; }    /* Custom highlight (modern) */

/* Input elements */
input::placeholder { color: #999; }         /* Placeholder text */

/* Lists */
li::marker { color: red; }                  /* List marker (bullet/number) */

/* Part of a shadow component */
my-button::part(button) { }                 /* Shadow DOM part */
```

Pseudo-elements with `::before` and `::after` require `content` property to render. They are inserted inside the element, as child elements.

### Attribute Selectors

Attribute selectors match elements based on attribute presence or value:

**Presence:**

```css
[disabled] { }              /* Any element with disabled attribute */
[hidden] { }                /* Any element with hidden attribute */
```

**Exact value:**

```css
[type="text"] { }           /* type="text" */
[lang="fr"] { }             /* lang="fr" */
```

**Partial value matches:**

```css
[class~="highlight"] { }    /* Contains word "highlight" in space-separated list */
[href^="https"] { }         /* Starts with "https" */
[href$=".pdf"] { }          /* Ends with ".pdf" */
[src*="thumbnail"] { }      /* Contains "thumbnail" anywhere */
[lang|="en"] { }            /* Starts with "en" followed by "-" (e.g., en-US) */
```

**Case insensitivity:**

```css
[type="text" i] { }         /* i flag makes value case-insensitive */
[class~="highlight" i] { }  /* Matches "highlight", "Highlight", "HIGHLIGHT" */
```

Attribute selectors are useful for styling by data attributes:

```css
[data-state="active"] { }
[data-theme="dark"] { }
[data-size="large"] { }
```

### Compound Selectors

Compound selectors combine multiple selectors on the same element (no spaces):

```css
/* All three conditions must match the same element */
div.highlight#hero { }       /* <div class="highlight" id="hero"> */
a.external[href^="https"] { } /* <a class="external" href="https://..."> */
input[type="text"].error { }  /* <input type="text" class="error"> */
```

Each additional selector increases specificity. A compound selector `div.highlight` is more specific than `.highlight` alone.

### Selector Lists

Separate selectors with commas to apply the same styles to multiple elements:

```css
h1, h2, h3 {
    font-family: 'Inter', sans-serif;
    line-height: 1.3;
}
```

Commas create a selector list. If any selector in the list is invalid, the entire list is invalid in older browsers. Modern CSS handles invalid selectors in lists better:

```css
/* Old behavior: one invalid selector invalidates the whole list */
h1, h2, h3:unknown-pseudo, h4 { }  /* Entire list invalid */

/* Modern: :is() isolates invalid selectors */
:is(h1, h2, h3:unknown-pseudo, h4) { }  /* h1 and h2 still match */
```

### Grouping Selectors

Grouping applies the same block to different selectors:

```css
/* These two are equivalent */
h1, h2, h3 { color: #333; }

/* Same as */
h1 { color: #333; }
h2 { color: #333; }
h3 { color: #333; }
```

Grouping improves maintainability — change one block instead of three.

### Selector Specificity

Specificity determines which selector wins in the cascade. Calculate it as a three-part score:

| Selector type | Points |
|---|---|
| Inline style | 1,0,0,0 |
| ID | 0,1,0,0 |
| Class, pseudo-class, attribute | 0,0,1,0 |
| Element, pseudo-element | 0,0,0,1 |

```css
/* Specificity values */
* { }                         /* 0,0,0,0 */
h1 { }                        /* 0,0,0,1 */
p.intro { }                   /* 0,0,1,1 */
.intro { }                    /* 0,0,1,0 */
#sidebar { }                  /* 0,1,0,0 */
#sidebar a:hover { }          /* 0,1,1,1 */
style="color: red" { }        /* 1,0,0,0 */
```

Compare left to right. `0,1,0,0` beats `0,0,9,9` — a single ID wins against any number of classes.

**`!important`** overrides specificity:

```css
/* !important overrides inline styles and all specificity */
.override { color: red !important; }
```

Use `!important` only for utility classes (e.g., `.hidden { display: none !important; }`) and user stylesheets. It breaks the cascade.

### Performance Considerations

Selectors affect rendering performance, especially on large pages:

```css
/* Fast selectors — prefer these */
.nav { }                    /* Class selector */
#header { }                 /* ID selector */
a { }                       /* Type selector */

/* Slower selectors — use sparingly */
ul li a { }                 /* Descendant combinator (three levels) */
ul > li > a { }             /* Child combinator (three levels) */
* .highlight { }            /* Universal selector as key */
```

Performance rules:
- Avoid universal selector as key selector (the rightmost part): `*`
- Keep descendant chains short (3+ levels start to hurt)
- Prefer class selectors for repeated elements
- Browsers are fast — do not micro-optimize selectors. Write for readability first, then profile.

### The :is() and :where() Selectors

`:is()` and `:where()` group selectors but differ in specificity:

```css
/* :is() takes the specificity of the most specific argument */
:is(h1, h2, .special) { }       /* Specificity: 0,0,1,0 (class wins) */
:is(#header, .nav) a { }        /* Specificity: 0,1,0,1 (ID wins) */

/* :where() always has 0 specificity */
:where(h1, h2, .special) { }    /* Specificity: 0,0,0,0 */
:where(#header, .nav) a { }     /* Specificity: 0,0,0,1 (only a counts) */
```

Use `:is()` to group selectors without repeating the parent:

```css
/* Without :is() */
section h1, article h1, aside h1 { }
section p, article p, aside p { }

/* With :is() — shorter, more maintainable */
:is(section, article, aside) :is(h1, p) { }
```

Use `:where()` for resets and baseline styles where you want low specificity:

```css
/* Easily overridden — specificity 0 for the prefix */
:where(nav, aside) a {
    color: inherit;
    text-decoration: none;
}
```

### The :has() Selector

`:has()` is the parent selector — it styles an element based on its descendants:

```css
/* Style <figure> that contains an <img> */
figure:has(img) {
    border: 1px solid #ddd;
    padding: 1rem;
}

/* Style <li> that contains a checked checkbox */
li:has(input:checked) {
    background: #e8f5e9;
}

/* Style <section> that does NOT contain a heading */
section:not(:has(h1, h2, h3, h4)) {
    background: #f5f5f5;
}

/* Style <form> with invalid fields */
form:has(:invalid) {
    border-color: red;
}
```

`:has()` works with combinators for complex queries:

```css
/* <a> with an <img> child — has child combinator */
a:has(> img) { border: none; }

/* <li> that is followed by another <li> containing .active */
li:has(+ li .active) { border-bottom: 2px solid blue; }
```

`:has()` is widely supported in all modern browsers (2023+). It opens up styling patterns that previously required JavaScript.

### The :not() Selector

`:not()` selects elements that do not match the argument:

```css
/* All <p> except those with class "intro" */
p:not(.intro) { color: #666; }

/* All inputs except checkboxes and radios */
input:not([type="checkbox"]):not([type="radio"]) {
    width: 100%;
}

/* All children except the last */
li:not(:last-child) { border-bottom: 1px solid #eee; }
```

`:not()` accepts a selector list (modern browsers):

```css
/* Exclude multiple classes */
p:not(.intro, .warning, .error) { }
```

### Nesting Selectors

Native CSS nesting (2024+) allows selector nesting without a preprocessor:

```css
.card {
    border: 1px solid #ddd;
    padding: 1rem;

    /* Nest with & reference to parent */
    & .title { font-size: 1.25rem; }
    & .body { color: #666; }

    /* Pseudo-classes with & */
    &:hover { border-color: #333; }

    /* Nesting without & (implicit descendant) */
    .highlight { background: yellow; }

    /* Nested media queries */
    @media (min-width: 768px) {
        display: flex;
        gap: 1rem;
    }
}
```

Nesting reduces repetition and keeps related styles grouped. The `&` token references the parent selector.

### Matching Behavior

The browser matches selectors differently depending on context:

```css
/* Multiple classes on one element */
<div class="card featured large">
```

```css
/* All three match the element */
.card { }           /* Matches */
.featured { }       /* Matches */
.card.featured { }  /* Matches (both classes required) */
.large { }          /* Matches */
```

**Class order in HTML does not matter.** `class="card featured"` and `class="featured card"` are identical.

**Case sensitivity:** HTML element names and attributes are case-insensitive, but class and ID values are case-sensitive:

```css
/* HTML: <div class="Card"> */
.card { }   /* Does NOT match — case matters in class values */

/* HTML: <DIV class="card"> */
div { }     /* Matches — element names are case-insensitive */
```

**Shadow DOM selectors** work differently — styles inside a shadow root do not leak out, and page selectors do not reach in.

## Glossary

| Term | Definition |
|---|---|
| Selector | Pattern matching DOM elements for style application |
| Type selector | Matches by element tag name (`div`, `p`) |
| Class selector | Matches by class attribute (`.card`) |
| ID selector | Matches by unique ID attribute (`#header`) |
| Combinator | Symbol defining relationship between selectors |
| Pseudo-class | Selector based on state or position (`:hover`, `:nth-child`) |
| Pseudo-element | Selector for a part of an element (`::before`) |
| Compound selector | Multiple selectors on the same element (no space) |
| Specificity | Score determining which selector wins in the cascade |
| Key selector | Rightmost selector in a chain — determines matching start point |

## Quick References

- [MDN: CSS Selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_selectors) — comprehensive selector reference
- [CSS Selectors Spec (Level 4)](https://www.w3.org/TR/selectors-4/) — official specification
- [Specificity Calculator](https://specificity.keegan.st/) — visualize specificity scores
- [Can I Use: :has()](https://caniuse.com/css-has) — browser support for parent selector
- [Selectors Explained](https://kittygiraudel.github.io/selectors-explained/) — visualize selector meaning

## Next Steps

- [Cascade, Specificity & Inheritance](cascade-and-specificity.md) — how the cascade resolves conflicts
- [The Box Model](box-model.md) — sizing and spacing fundamentals