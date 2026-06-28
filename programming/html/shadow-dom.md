# Shadow DOM

## Description

Shadow DOM provides DOM and style encapsulation for Web Components, isolating component internals from the rest of the page. It is the foundation for building reusable, conflict-free custom elements.

## Prerequisites

- [Custom Elements in Depth](custom-elements.md) — custom element lifecycle and APIs
- [Web Components Overview](web-components-intro.md) — the three pillars

## Table of Contents

- [What is Shadow DOM?](#what-is-shadow-dom)
- [Shadow Root](#shadow-root)
- [Shadow DOM Modes](#shadow-dom-modes)
- [DOM Encapsulation](#dom-encapsulation)
- [Style Encapsulation](#style-encapsulation)
- [CSS That Crosses the Boundary](#css-that-crosses-the-boundary)
- [The :host Pseudo-Class](#the-host-pseudo-class)
- [The :host-context Pseudo-Class](#the-host-context-pseudo-class)
- [The ::slotted Pseudo-Element](#the-slotted-pseudo-element)
- [CSS Custom Properties (Variables)](#css-custom-properties-variables)
- [The ::part Pseudo-Element](#the-part-pseudo-element)
- [The theme Attribute Pattern](#the-theme-attribute-pattern)
- [Event Retargeting](#event-retargeting)
- [Event Composition](#event-composition)
- [Focus Delegation](#focus-delegation)
- [Slot-Based Composition](#slot-based-composition)
- [Nested Shadow DOM](#nested-shadow-dom)
- [Declarative Shadow DOM](#declarative-shadow-dom)
- [Accessibility in Shadow DOM](#accessibility-in-shadow-dom)
- [Testing Shadow DOM](#testing-shadow-dom)
- [Performance](#performance)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What is Shadow DOM?

Shadow DOM is a browser API that attaches a hidden DOM tree to an element. This shadow tree is:

- **Isolated from the page DOM** — queries like `document.querySelector` do not reach into shadow trees
- **Style-scoped** — CSS from the page does not apply inside, and styles inside do not leak out
- **Event-scoped** — events crossing the boundary are retargeted to the host element

**Without Shadow DOM:**

```html
<style>
    .card { border: 1px solid #ddd; }
</style>
<div class="card">My card</div>
```

The style is global. Another component using `.card` could conflict.

**With Shadow DOM:**

```javascript
this.attachShadow({ mode: 'open' });
this.shadowRoot.innerHTML = `
    <style>
        .card { border: 1px solid #ddd; }
    </style>
    <div class="card"><slot></slot></div>
`;
```

Now `.card` is scoped to this component. Page CSS cannot accidentally affect it.

### Shadow Root

The shadow root is the root node of the shadow tree. It is attached to a **shadow host** (the custom element):

```javascript
const host = document.querySelector('my-component');
const root = host.attachShadow({ mode: 'open' });

// root is a DocumentFragment-like node
console.log(root.nodeType);  // 11 (DocumentFragment)
```

**Only certain elements can host a shadow root:**
- Custom elements (autonomous and customized built-in)
- `<article>`, `<aside>`, `<blockquote>`, `<body>`, `<div>`, `<footer>`, `<h1>`-`<h6>`, `<header>`, `<main>`, `<nav>`, `<p>`, `<section>`, `<span>`

Most practical use is with custom elements.

### Shadow DOM Modes

| Mode | `element.shadowRoot` | External access |
|---|---|---|
| `open` | Returns the shadow root | Yes, via `element.shadowRoot` |
| `closed` | Returns `null` | No direct access (but can be worked around) |

```javascript
this.attachShadow({ mode: 'open' });    // Accessible
this.attachShadow({ mode: 'closed' });  // Not accessible
```

**Always use `mode: 'open'`.** Closed mode provides no real security (the shadow root can still be accessed via other means) and breaks tooling, testing, and accessibility inspection.

### DOM Encapsulation

The shadow tree is separate from the light DOM (the element's children):

```javascript
// Light DOM — element's children
<my-component>
    <span>I am light DOM</span>
</my-component>
```

```javascript
// Inside the component constructor
const shadow = this.attachShadow({ mode: 'open' });
shadow.innerHTML = `<p>I am shadow DOM</p>`;

// Both exist independently
console.log(this.children);          // [<span>] (light DOM)
console.log(this.shadowRoot.children);  // [<p>] (shadow DOM)
```

**Queries do not cross the boundary:**

```javascript
// Cannot find internal elements from outside
document.querySelector('my-component p');  // null (p is in shadow)

// Must query through shadowRoot
document.querySelector('my-component')
    .shadowRoot.querySelector('p');  // works
```

### Style Encapsulation

Styles declared inside a shadow root do not affect the page, and page styles do not affect elements inside the shadow root:

```javascript
shadow.innerHTML = `
    <style>
        /* These styles only apply inside this component */
        .container { display: flex; gap: 1rem; }
        h2 { font-size: 1.2rem; }
    </style>
    <div class="container">
        <h2><slot></slot></h2>
    </div>
`;
```

**Page CSS does not penetrate:**

```css
/* Page-level CSS — does NOT affect shadow DOM */
h2 { color: red; }
.container { background: blue; }
/* These do NOT apply inside <my-component>'s shadow root */
```

**Component CSS does not leak:**

```css
/* Inside shadow DOM — does NOT affect the page */
h2 { color: green; }
/* This only affects h2 inside this component */
```

### CSS That Crosses the Boundary

Some CSS properties are inherited from the page into the shadow DOM:

```css
/* Inherited properties cross the boundary */
color, font-family, font-size, line-height, text-align
/* These are inherited by the shadow root from the host */
```

Non-inherited properties do not cross:

```css
/* NOT inherited into shadow DOM */
background, border, margin, padding, width, height, display
```

**`all: initial`** in shadow styles resets everything:

```javascript
shadow.innerHTML = `
    <style>
        :host { all: initial; }  /* Reset inherited properties */
        display: block;
    </style>
`;
```

This is useful when you want full isolation from page styles.

### The :host Pseudo-Class

`:host` styles the shadow host element from inside the shadow root:

```javascript
shadow.innerHTML = `
    <style>
        :host {
            display: inline-block;
            border: 1px solid var(--border-color, #ddd);
            padding: 1rem;
        }
        :host([disabled]) {
            opacity: 0.5;
            pointer-events: none;
        }
        :host(.active) {
            border-color: blue;
        }
    </style>
`;
```

`:host` can take a selector argument to apply conditional styles:

```javascript
:host([hidden]) { display: none; }
:host([variant="primary"]) { background: blue; color: white; }
:host([variant="secondary"]) { background: gray; color: white; }
```

### The :host-context Pseudo-Class

`:host-context()` styles the host based on a parent selector in the light DOM. Useful for theme-based styling:

```javascript
shadow.innerHTML = `
    <style>
        :host-context(.dark-theme) { background: #333; color: #fff; }
        :host-context(.dark-theme) button { background: #555; }
    </style>
`;
```

It matches if any ancestor (not just the parent) matches the selector.

### The ::slotted Pseudo-Element

`::slotted()` styles content projected into `<slot>` elements. It only targets top-level slotted nodes — descendant combinators like `::slotted(div) p` do not work.

```javascript
shadow.innerHTML = `
    <style>
        ::slotted(h2) { font-size: 1.5rem; margin: 0; }
        ::slotted([slot="footer"]) { border-top: 1px solid #eee; }
    </style>
    <slot name="title"></slot>
    <slot></slot>
    <slot name="footer"></slot>
`;
```

### CSS Custom Properties (Variables)

CSS custom properties are the recommended way to pass styles into Shadow DOM:

```javascript
shadow.innerHTML = `
    <style>
        .card {
            background: var(--card-bg, #fff);
            border-color: var(--card-border, #ddd);
            border-radius: var(--card-radius, 4px);
        }
        button {
            background: var(--primary, #007bff);
            color: var(--primary-text, #fff);
        }
    </style>
`;
```

```css
/* Page sets these values */
my-component {
    --card-bg: #f8f9fa;
    --primary: #28a745;
}
```

**Why CSS variables work:** Custom properties inherit through the shadow boundary. Non-inherited CSS properties do not, but custom properties always inherit.

**Default values** in `var()` ensure the component has fallbacks:

```css
color: var(--text-color, #333);  /* Falls back to #333 if --text-color not set */
```

### The ::part Pseudo-Element

`::part()` allows external page CSS to style specific elements inside the shadow tree:

```javascript
shadow.innerHTML = `
    <style>
        .button { /* base styles */ }
        .icon { /* base styles */ }
    </style>
    <button part="button">
        <img part="icon" src="icon.png">
        <slot></slot>
    </button>
`;
```

```css
/* Page-level CSS styles the parts */
my-button::part(button) {
    font-size: 1.2rem;
    padding: 0.75rem 1.5rem;
}
my-button::part(icon) {
    width: 24px;
    height: 24px;
}
my-button::part(button):hover {
    background: #e0e0e0;
}
```

**Export parts** to allow styling of nested components:

```javascript
// Child component
shadow.innerHTML = `<div part="container">...</div>`;

// Parent component uses forwardpart
shadow.innerHTML = `
    <my-child exportparts="container: child-container"></my-child>
`;
```

```css
/* Page styles the child's part through the forward */
my-parent::part(child-container) {
    border: 2px solid red;
}
```

### The theme Attribute Pattern

Combine `::part()` with attribute-based theming:

```javascript
shadow.innerHTML = `
    <style>
        :host {
            --btn-bg: var(--primary, #007bff);
        }
        :host([variant="danger"]) { --btn-bg: #dc3545; }
        :host([variant="success"]) { --btn-bg: #28a745; }
    </style>
    <button part="button" style="background: var(--btn-bg)">
        <slot></slot>
    </button>
`;
```

```html
<my-button variant="danger" theme="dark">Delete</my-button>
```

### Event Retargeting

When an event crosses the shadow boundary, `target` is retargeted to the shadow host. Use `composedPath()` to get the full path including shadow-internal elements:

```javascript
// Outside
document.querySelector('my-component').addEventListener('click', (e) => {
    console.log(e.target);         // <my-component> (retargeted)
    console.log(e.composedPath());  // [button, shadow-root, my-component, body, html]
});
```

Retargeting hides internal implementation details from external code.

### Event Composition

The `composed` option controls whether events bubble through shadow boundaries:

```javascript
// This event does NOT cross the shadow boundary (default)
this.dispatchEvent(new CustomEvent('internal', {
    bubbles: true,
    composed: false,  // Default
}));

// This event bubbles out of shadow DOM
this.dispatchEvent(new CustomEvent('external', {
    bubbles: true,
    composed: true,
}));
```

**Events that are always composed:**
- `click`, `mousedown`, `mouseup`, `keydown`, `keyup`
- `focus`, `blur`, `focusin`, `focusout`
- `input`, `change`
- `touchstart`, `touchend`

**Events that are NOT composed by default:**
- Custom events (`CustomEvent`)
- `mouseenter`, `mouseleave`
- `load`, `error` (on some elements)

### Focus Delegation

The `delegatesFocus` option controls focus behavior:

```javascript
this.attachShadow({ mode: 'open', delegatesFocus: true });
```

With `delegatesFocus: true`:
- Focusing the host moves focus to the first focusable element inside the shadow root
- Tab into the component automatically focuses the first interactive element
- Clicking non-interactive areas also focuses the first interactive element

```javascript
this.attachShadow({ mode: 'open', delegatesFocus: true });
shadow.innerHTML = `
    <input type="text" placeholder="First name">
    <button>Submit</button>
`;
```

Without `delegatesFocus`, clicking the component focuses the host. With it, the `<input>` receives focus.

### Slot-Based Composition

Slots project light DOM content into the shadow tree:

```javascript
shadow.innerHTML = `
    <div class="card">
        <header><slot name="title"></slot></header>
        <main><slot></slot></main>
        <footer><slot name="actions"></slot></footer>
    </div>
`;
```

**Fallback content** displays when no slot content is provided:

```javascript
<slot name="title">Default Title</slot>
```

**Multiple elements can target the same slot:**

```html
<my-card>
    <h2 slot="title">Card Title</h2>
    <p>Main content.</p>
    <p>More content.</p>
    <button slot="actions">Save</button>
    <button slot="actions">Cancel</button>
</my-card>
```

Both `<p>` elements go to the unnamed `<slot>`. Both buttons go to the `actions` slot.

**The `slotchange` event** fires when slotted content changes:

```javascript
shadow.querySelector('slot').addEventListener('slotchange', (e) => {
    console.log('Slotted content changed:', e.target.assignedNodes());
});
```

### Nested Shadow DOM

Shadow DOM can be nested — a component inside another component has its own shadow root:

```html
<outer-component>
    <!-- outer-component's shadow root -->
    <inner-component>
        <!-- inner-component's shadow root -->
    </inner-component>
</outer-component>
```

Each shadow root provides independent encapsulation. Styles from the outer component do not affect the inner component, and vice versa.

**Cross-component styling:**

```css
/* Can style parts of nested components via exportparts */
outer-component::part(inner-button) {
    /* Only works if inner exports its part */
}
```

### Declarative Shadow DOM

Declarative Shadow DOM allows defining shadow roots directly in HTML without JavaScript:

```html
<my-component>
    <template shadowrootmode="open">
        <style>
            p { color: blue; }
        </style>
        <p>Shadow content</p>
        <slot></slot>
    </template>
    <p>Light DOM content</p>
</my-component>
```

**Benefits:**
- Works without JavaScript (server-side rendering)
- Content is visible during loading (no FOUC)
- Progressive enhancement

**Browser support:** Declarative Shadow DOM is supported in Chrome 111+, Safari 16.4+, Firefox 121+.

**Serialization and cloning:**

```javascript
// Serialize to string
const html = element.getInnerHTML({ includeShadowRoots: true });

// Clone with shadow roots
const clone = element.cloneNode(true);  // Deep clone preserves shadow roots
```

### Accessibility in Shadow DOM

**ARIA works across shadow boundaries.** ARIA attributes inside shadow DOM participate in the global accessibility tree:

```javascript
shadow.innerHTML = `
    <button aria-label="Close dialog">×</button>
`;
```

**`aria-labelledby` and `aria-describedby` work across the boundary** — a light DOM label can reference a shadow DOM element and vice versa.

**Focus management with `delegatesFocus`:**

```javascript
// Makes the component focusable and delegates to first focusable child
this.attachShadow({ mode: 'open', delegatesFocus: true });
```

**Screen reader behavior:**
- Screen readers enter the shadow root seamlessly
- The accessibility tree merges shadow and light DOM
- `role` and `aria-*` attributes inside shadow DOM are recognized

**Best practices:**
- Always set accessible names on interactive elements inside shadow DOM
- Use semantic HTML elements inside shadow DOM (they get correct implicit roles)
- Do not rely on `aria-describedby` across shadow boundaries without testing

### Testing Shadow DOM

```javascript
// Access shadow root
const element = document.querySelector('my-component');
const shadow = element.shadowRoot;

// Query within shadow DOM
shadow.querySelector('.internal-class');
shadow.querySelectorAll('[role="button"]');

// Test slotted content
const slot = shadow.querySelector('slot');
const assigned = slot.assignedNodes();
const assignedElements = slot.assignedElements();
```

**Testing with `@open-wc/testing`:**

```javascript
import { fixture, expect } from '@open-wc/testing';

describe('my-component', () => {
    it('renders shadow content', async () => {
        const el = await fixture(`<my-component></my-component>`);
        expect(el.shadowRoot.querySelector('.card')).to.exist;
    });
});
```

### Performance

Shadow DOM has minimal performance overhead. The browser does the encapsulation natively.

**Best practices:**
- Attach shadow root in `constructor()`, not `connectedCallback()`
- Use `<template>` for cloning instead of `innerHTML` for repeated renders
- Avoid deep nesting of shadow roots (more than ~5 levels may affect performance)
- Use `mode: 'open'` — closed mode has the same performance

## Study Cases

### Themed Button Component

A reusable button with variants, sizes, and dark mode support using Shadow DOM styling features:

```javascript
class ThemedButton extends HTMLElement {
    constructor() {
        super();
        this.attachShadow({ mode: 'open' });
    }

    connectedCallback() {
        this.shadowRoot.innerHTML = `
            <style>
                :host {
                    display: inline-block;
                }
                :host([variant="primary"]) { --btn-bg: var(--primary, #007bff); }
                :host([variant="danger"]) { --btn-bg: var(--danger, #dc3545); }
                :host([size="small"]) { --btn-padding: 0.25rem 0.5rem; --btn-font: 0.875rem; }
                :host([size="large"]) { --btn-padding: 0.75rem 1.5rem; --btn-font: 1.125rem; }
                :host-context(.dark-mode) button {
                    --btn-color: #e0e0e0;
                }
                button {
                    padding: var(--btn-padding, 0.5rem 1rem);
                    font-size: var(--btn-font, 1rem);
                    background: var(--btn-bg, #007bff);
                    color: var(--btn-color, #fff);
                    border: none; border-radius: 4px;
                    cursor: pointer;
                }
                :host([disabled]) button {
                    opacity: 0.5; pointer-events: none;
                }
            </style>
            <button part="button"><slot></slot></button>
        `;
    }
}
customElements.define('themed-button', ThemedButton);
```

```html
<div class="dark-mode">
    <themed-button variant="primary" size="large">Save</themed-button>
    <themed-button variant="danger">Delete</themed-button>
</div>
```

The button responds to `:host-context(.dark-mode)` for dark mode, `::part(button)` for external styling, and CSS variables for theming.

### Tab Panel with Shadow DOM

```javascript
class TabPanel extends HTMLElement {
    constructor() {
        super();
        this.attachShadow({ mode: 'open' });
    }
    connectedCallback() {
        this.render();
        this.shadowRoot.querySelector('[role="tablist"]')
            .addEventListener('click', (e) => {
                const tab = e.target.closest('[role="tab"]');
                if (tab) this.selectTab(tab.dataset.tab);
            });
    }
    selectTab(name) {
        this.shadowRoot.querySelectorAll('[role="tab"]')
            .forEach(t => t.setAttribute('aria-selected', t.dataset.tab === name));
        this.shadowRoot.querySelectorAll('[role="tabpanel"]')
            .forEach(p => p.hidden = p.dataset.panel !== name);
    }
    render() {
        const tabs = Array.from(this.querySelectorAll('[slot="tab"]'))
            .map((t, i) => `<button role="tab" aria-selected="${i === 0}"
                data-tab="${t.dataset.tab}">${t.textContent}</button>`).join('');
        const panels = this.innerHTML.replace(/<[^>]+slot="tab"[^>]*>.*?<\/[^>]+>/g, '');
        this.shadowRoot.innerHTML = `
            <style>
                [role="tablist"] { display: flex; border-bottom: 2px solid #ddd; }
                [role="tab"] { padding: 0.5rem 1rem; cursor: pointer; background: none; border: none; }
                [role="tab"][aria-selected="true"] { border-bottom: 2px solid blue; }
            </style>
            <div role="tablist">${tabs}</div>
            <slot></slot>
        `;
    }
}
customElements.define('tab-panel', TabPanel);
```

```html
<tab-panel>
    <span slot="tab" data-tab="html">HTML</span>
    <div data-tab="html">HTML content</div>
    <span slot="tab" data-tab="css">CSS</span>
    <div data-tab="css">CSS content</div>
</tab-panel>
```

## Examples

### Example 1: Isolated counter with external override

```javascript
class IsolatedCounter extends HTMLElement {
    constructor() {
        super();
        this.attachShadow({ mode: 'open' });
        this._count = 0;
    }

    connectedCallback() {
        this.shadowRoot.innerHTML = `
            <style>
                :host { display: inline-block; text-align: center; }
                .count { font-size: 2em; font-weight: bold; }
                button { padding: 0.5rem 1rem; cursor: pointer; }
            </style>
            <div class="count">0</div>
            <button part="increment">+1</button>
        `;

        this.shadowRoot.querySelector('button')
            .addEventListener('click', () => {
                this._count++;
                this.shadowRoot.querySelector('.count').textContent = this._count;
            });
    }
}
customElements.define('isolated-counter', IsolatedCounter);
```

```css
/* Page can style the button via part */
isolated-counter::part(increment) {
    background: #28a745;
    color: white;
}
```

The shadow DOM encapsulates the counter's DOM and styles. The page can only style the exposed `part`.

## Glossary

| Term | Definition |
|---|---|
| Shadow root | Root node of the shadow tree, attached to a host element |
| Shadow host | Element that has a shadow root attached |
| Shadow tree | DOM tree rooted at the shadow root |
| Light DOM | Children of the host element (not in shadow tree) |
| Slot | Element that projects light DOM content into shadow tree |
| `:host` | Pseudo-class selecting the shadow host |
| `::slotted()` | Pseudo-element styling projected content |
| `::part()` | Pseudo-element for external styling of shadow internals |
| Retargeting | Adjusting event target to the shadow host |
| `composed` | Event option controlling shadow boundary crossing |
| `delegatesFocus` | Option forwarding focus to the first focusable child |
| Declarative Shadow DOM | HTML-only shadow root definition without JavaScript |

## Quick References

- [MDN: Shadow DOM](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Shadow_DOM) — shadow DOM reference
- [CSS Scoping Module](https://www.w3.org/TR/css-scoping-1/) — `:host`, `::slotted` specification
- [CSS Shadow Parts](https://www.w3.org/TR/css-shadow-parts-1/) — `::part()` and `exportparts`
- [Declarative Shadow DOM](https://web.dev/articles/declarative-shadow-dom) — HTML-only shadow root

## Next Steps

- [Templates and Slots](templates-and-slots.md) — HTML template elements and slot-based composition
- [Custom Elements in Depth](custom-elements.md) — detailed lifecycle and APIs
- [Web Components Overview](web-components-intro.md) — the three pillars