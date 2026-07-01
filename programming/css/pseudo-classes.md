# Pseudo-Classes & Pseudo-Elements

## Description

Pseudo-classes select elements based on state or position. Pseudo-elements select specific parts of elements. Together they enable fine-grained styling without extra markup.

## Prerequisites

- [CSS Selectors](selectors.md) — selector syntax and combinator basics
- [Values & Units](values-and-units.md) — content property values

## Table of Contents

- [Pseudo-Classes vs Pseudo-Elements](#pseudo-classes-vs-pseudo-elements)
- [Dynamic Pseudo-Classes](#dynamic-pseudo-classes)
- [Form State Pseudo-Classes](#form-state-pseudo-classes)
- [Structural Pseudo-Classes](#structural-pseudo-classes)
- [Positional Pseudo-Classes](#positional-pseudo-classes)
- [Input Value Pseudo-Classes](#input-value-pseudo-classes)
- [Language Pseudo-Classes](#language-pseudo-classes)
- [Negation Pseudo-Class](#negation-pseudo-class)
- [Relational Pseudo-Class](#relational-pseudo-class)
- [Focus Pseudo-Classes](#focus-pseudo-classes)
- [Target Pseudo-Class](#target-pseudo-class)
- [Time-Dimensional Pseudo-Classes](#time-dimensional-pseudo-classes)
- [Pseudo-Elements](#pseudo-elements)
- [Generated Content with ::before and ::after](#generated-content-with-before-and-after)
- [The ::marker Pseudo-Element](#the-marker-pseudo-element)
- [The ::selection Pseudo-Element](#the-selection-pseudo-element)
- [The ::placeholder Pseudo-Element](#the-placeholder-pseudo-element)
- [The ::part and ::slotted Pseudo-Elements](#the-part-and-slotted-pseudo-elements)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Pseudo-Classes vs Pseudo-Elements

| Type | Syntax | Selects |
|---|---|---|
| Pseudo-class | `:` (single colon) | State or position of an element |
| Pseudo-element | `::` (double colon) | Part of an element |

```css
a:hover { }          /* Pseudo-class — link in hover state */
p::first-line { }    /* Pseudo-element — first line of paragraph */
```

CSS3 standardized `::` for pseudo-elements, but browsers also accept `:` for older pseudo-elements (`:before`, `:after`, `:first-line`, `:first-letter`).

### Dynamic Pseudo-Classes

User interaction states:

```css
a:link { }              /* Unvisited link */
a:visited { }           /* Visited link */
a:hover { }             /* Mouse over */
a:active { }            /* Being clicked */
a:focus { }             /* Has focus (any method) */
a:focus-visible { }     /* Focus via keyboard */
a:focus-within { }      /* Element or any descendant has focus */
```

Order matters for links: `:link` → `:visited` → `:hover` → `:active` (LoVe HAte).

**`:focus-visible`** — shows focus ring only when it helps the user (keyboard navigation), not on mouse click:

```css
/* Always show focus for keyboard users */
:focus-visible {
    outline: 2px solid #0066cc;
    outline-offset: 2px;
}

/* Remove focus outline on mouse click only */
:focus:not(:focus-visible) {
    outline: none;
}
```

**`:focus-within`** — parent element receives focus when any child has focus:

```css
.form-group:focus-within {
    border-color: #0066cc;
    box-shadow: 0 0 0 3px rgba(0, 102, 204, 0.2);
}

.form-group:focus-within label {
    color: #0066cc;
}
```

### Form State Pseudo-Classes

```css
input:enabled { }           /* Enabled */
input:disabled { }          /* Disabled */
input:read-only { }         /* readonly attribute */
input:read-write { }        /* Not readonly */
input:required { }          /* required attribute */
input:optional { }          /* Not required */
input:placeholder-shown { } /* Placeholder text visible */
input:default { }           /* Default button/submit in a form */
```

### Structural Pseudo-Classes

Based on DOM position:

```css
:root { }                       /* Document root (html) */
:empty { }                      /* No children at all (including text) */

li:first-child { }              /* First child of parent */
li:last-child { }               /* Last child of parent */
li:only-child { }               /* Only child of parent */

li:first-of-type { }            /* First <li> in parent */
li:last-of-type { }             /* Last <li> in parent */
li:only-of-type { }             /* Only <li> in parent */
```

`:empty` only matches elements with absolutely no content (not even whitespace or text nodes):

```html
<div></div>          <!-- :empty matches -->
<div> </div>         <!-- :empty does NOT match (whitespace) -->
<div><!-- comment --></div>   <!-- :empty matches (comments ignored) -->
```

### Positional Pseudo-Classes

An+B notation for selecting specific children:

```css
/* Numerical */
li:nth-child(3) { }             /* Third child */
li:nth-last-child(3) { }        /* Third from last */

/* Even/odd */
li:nth-child(even) { }          /* 2nd, 4th, 6th... */
li:nth-child(odd) { }           /* 1st, 3rd, 5th... */

/* Every N */
li:nth-child(3n) { }            /* Every 3rd: 3, 6, 9... */
li:nth-child(3n+1) { }          /* Every 3rd starting from 1: 1, 4, 7... */
li:nth-child(3n+2) { }          /* Every 3rd starting from 2: 2, 5, 8... */

/* First/last N */
li:nth-child(-n+3) { }          /* First 3 children */
li:nth-last-child(-n+3) { }     /* Last 3 children */

/* Of-type variants — same syntax, but only count matching elements */
li:nth-of-type(2n) { }          /* Even-indexed <li> among siblings */
li:nth-last-of-type(2) { }      /* Second-to-last <li> */
```

An+B formula breakdown:
- `a` = step size (how many to skip)
- `n` = counter starting from 0
- `b` = offset

```
3n+1: n=0→1, n=1→4, n=2→7, n=3→10...
-n+3: n=0→3, n=1→2, n=2→1, n=3→0 (stop)
```

### Input Value Pseudo-Classes

```css
input:checked { }               /* Checked checkbox/radio/option */
input:indeterminate { }         /* Neither checked nor unchecked (e.g., checkbox group) */

/* Range */
input:in-range { }              /* Within min/max */
input:out-of-range { }          /* Outside min/max */

/* Validity */
input:valid { }                 /* Passes constraint validation */
input:invalid { }               /* Fails constraint validation */

/* User interaction */
input:user-valid { }            /* Valid after user interaction */
input:user-invalid { }          /* Invalid after user interaction */
```

`user-valid` and `user-invalid` only trigger after the user has interacted with the field, preventing the immediate "red flash" on page load.

### Language Pseudo-Classes

```css
:lang(en) { }                   /* Element with lang="en" */
:lang(fr) { }                   /* Element with lang="fr" */
:lang(de) { }                   /* Element with lang="de" */
```

`:lang()` matches based on language inheritance (not just the attribute). If a parent has `lang="fr"`, all children match `:lang(fr)`.

### Negation Pseudo-Class

`:not()` excludes elements matching the argument:

```css
/* All inputs except checkboxes */
input:not([type="checkbox"]) { width: 100%; }

/* All paragraphs except .intro and .warning */
p:not(.intro, .warning) { color: #666; }

/* All items except the last */
li:not(:last-child) { border-bottom: 1px solid #eee; }

/* All children except those that are empty or disabled */
.field:not(:empty, :disabled) { background: white; }
```

`:not()` accepts a selector list (comma-separated) in modern browsers. Invalid selectors inside `:not()` do not invalidate the entire rule.

### Relational Pseudo-Class

`:has()` selects elements based on their descendants:

```css
/* Style figure only if it contains an img */
figure:has(img) { border: 1px solid #ddd; }

/* Style form sections that contain an error */
.form-section:has(.error-message) { border-color: red; }

/* Style the active tab -- list item containing a link with class active */
li:has(a.active) { background: #f0f4ff; }

/* Style card differently when it has an image */
.card:has(> img) { grid-template-columns: 200px 1fr; }
```

`:has()` can traverse forward and up the DOM:

```css
/* Select h2 that is directly followed by a paragraph */
h2:has(+ p) { margin-bottom: 0; }

/* Style a row when any of its cells has a value */
tr:has(td.value) { font-weight: 600; }
```

`:has()` is widely supported in modern browsers (2023+). It is commonly called the "parent selector."

### Focus Pseudo-Classes

```css
:focus { }                      /* Any focus — mouse or keyboard */
:focus-visible { }              /* Keyboard-only focus */
:focus-within { }               /* Element contains focused child */
```

`:focus-visible` is the recommended focus indicator for keyboard accessibility:

```css
/* Custom focus ring for keyboard users */
:focus-visible {
    outline: 3px solid #0066cc;
    outline-offset: 2px;
    border-radius: 2px;
}

/* Remove default browser focus */
:focus:not(:focus-visible) {
    outline: none;
}
```

### Target Pseudo-Class

`:target` matches the element whose ID matches the URL hash:

```css
<section id="features">...</section>
```

```css
/* URL: /page.html#features */
#features:target {
    animation: highlight 2s;
    scroll-margin-top: 60px;    /* Offset for fixed header */
}

@keyframes highlight {
    0% { background: #fff3cd; }
    100% { background: transparent; }
}
```

### Time-Dimensional Pseudo-Classes

For media with timing (audio/video/text tracks):

```css
video::cue { }                  /* Text track cues */
:paused { }                     /* Video/audio in paused state */
:playing { }                    /* Video/audio playing */
:muted { }                      /* Audio muted */
:volume-locked { }              /* Volume cannot be changed */
```

### Pseudo-Elements

Pseudo-elements style specific parts of elements:

```css
::before { }                    /* First child of element (generated content) */
::after { }                     /* Last child of element (generated content) */
::first-letter { }              /* First letter of text content */
::first-line { }                /* First line of text content */
::selection { }                 /* User-selected text */
::placeholder { }               /* Placeholder text */
::marker { }                    /* List marker (bullet/number) */
::backdrop { }                  /* Fullscreen element's background */
::file-selector-button { }      /* Upload button in <input type="file"> */
::cue { }                       /* WebVTT text cue */
::highlight(name) { }           /* Custom highlighted text range */
::target-text { }               /* Scroll-to-text target highlight */
::part(name) { }                /* Shadow DOM exposed part */
::slotted(selector) { }         /* Content slotted into Shadow DOM */
```

### Generated Content with ::before and ::after

These pseudo-elements insert content before or after an element's content:

```css
.element::before {
    content: "→ ";
    color: #0066cc;
}

.element::after {
    content: attr(data-label);
    font-size: 0.875rem;
}
```

They require `content` to render. Without `content`, they do not appear.

```css
/* Decorative elements */
.link::after {
    content: "";
    display: inline-block;
    width: 1em;
    height: 1em;
    background: url('arrow.svg') center no-repeat;
}

.icon-link::before {
    content: "";
    display: inline-block;
    width: 1em;
    height: 1em;
    mask: url('icon.svg') center no-repeat;
    background: currentColor;
}
```

`::before` and `::after` are inline by default. Set `display: block` or `display: inline-block` for sizing.

### The ::marker Pseudo-Element

Style list markers:

```css
li::marker {
    color: #0066cc;
    font-weight: 600;
    content: "▶ ";              /* Custom marker */
}

li::marker {
    content: counter(list-item) ". ";  /* Keep default numbering but restyle */
}
```

`::marker` supports a limited set of properties: `color`, `font-*`, `content`, `white-space`, `direction`, `unicode-bidi`.

### The ::selection Pseudo-Element

Style user-selected text:

```css
::selection {
    background: #0066cc;
    color: white;
}

/* Per-element selection style */
p::selection {
    background: #fff3cd;
}

h1::selection {
    background: #0066cc;
    color: white;
}
```

### The ::placeholder Pseudo-Element

Style input placeholder text:

```css
input::placeholder {
    color: #999;
    opacity: 1;                 /* Firefox default is < 1 */
    font-style: italic;
}

input::placeholder {
    color: #dc3545;              /* Error color for invalid state */
}
```

### The ::part and ::slotted Pseudo-Elements

For Shadow DOM:

```css
/* From outside the shadow tree — style exposed parts */
my-button::part(button) {
    background: #0066cc;
    color: white;
    border: none;
}

my-button::part(icon) {
    width: 1em;
    height: 1em;
}
```

```css
/* Inside shadow DOM — style slotted content */
::slotted(p) {
    color: #666;
}

::slotted([slot="header"]) {
    font-weight: 600;
}
```

## Glossary

| Term | Definition |
|---|---|
| Pseudo-class | Selects element by state or position |
| Pseudo-element | Selects a part of an element |
| :has() | Parent selector — selects based on children |
| :not() | Negation selector |
| :focus-visible | Keyboard-only focus indicator |
| :focus-within | Element containing focused child |
| ::before | Generated content before element |
| ::after | Generated content after element |
| ::marker | List marker style |
| ::selection | User-selected text style |
| An+B | Formula for nth-child patterns |

## Quick References

- [MDN: Pseudo-classes](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes) — complete list
- [MDN: Pseudo-elements](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements) — complete list
- [CSS Selectors Level 4](https://www.w3.org/TR/selectors-4/) — specification
- [CSS Pseudo-Elements Level 4](https://www.w3.org/TR/css-pseudo-4/) — specification
- [Caniuse: :has()](https://caniuse.com/css-has) — browser support

## Next Steps

- [CSS Transitions & Animations](transitions-and-animations.md) — movement and interactivity