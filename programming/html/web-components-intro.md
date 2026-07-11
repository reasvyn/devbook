# Web Components Overview

## Description

Web Components are a set of browser-native APIs for creating reusable, encapsulated custom HTML elements. They consist of Custom Elements, Shadow DOM, and HTML Templates — no frameworks or build tools required.

## Prerequisites

- [Scripts, Defer & Async](scripts-and-defer.md) — JavaScript fundamentals for custom element lifecycle
- [Elements, Tags & Attributes](elements-and-attributes.md) — HTML element structure
- [Semantic HTML & Document Outline](semantic-html.md) — semantic structure

## Table of Contents

- [What Are Web Components?](#what-are-web-components)
- [The Three Pillars](#the-three-pillars)
- [Browser Support](#browser-support)
- [Web Components vs Frameworks](#web-components-vs-frameworks)
- [Design Principles](#design-principles)
- [Naming Conventions](#naming-conventions)
- [Lifecycle Overview](#lifecycle-overview)
- [Encapsulation](#encapsulation)
- [Styling Strategies](#styling-strategies)
- [Event Handling](#event-handling)
- [Attributes and Properties](#attributes-and-properties)
- [Slots and Composition](#slots-and-composition)
- [Form Participation](#form-participation)
- [Accessibility](#accessibility)
- [Testing Web Components](#testing-web-components)
- [Performance Considerations](#performance-considerations)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Are Web Components?

Web Components are three browser APIs that together let you create custom HTML elements with their own behavior, styles, and structure:

```html
<!-- Custom element looks like any other HTML tag -->
<product-card
    name="Widget Pro"
    price="29.99"
    image="widget.jpg">
</product-card>
```

The browser treats `<product-card>` as a first-class HTML element. It can have attributes, children, lifecycle callbacks, and participate in the DOM just like `<div>`, `<button>`, or `<video>`.

**Core benefits:**
- **Encapsulation** — styles and DOM are isolated from the page
- **Reusability** — write once, use anywhere (any framework, any page)
- **Standard-based** — no dependencies, no lock-in
- **Interoperable** — works in any framework (React, Vue, Angular, or vanilla JS)

### The Three Pillars

| API | Purpose | Specification |
|---|---|---|
| **Custom Elements** | Define new HTML elements with JavaScript classes | HTML Living Standard |
| **Shadow DOM** | Encapsulate DOM and styles | DOM Living Standard |
| **HTML Templates** | Declare reusable markup fragments | HTML Living Standard |

**Custom Elements** let you define a class that extends `HTMLElement` and register it with the browser:

```javascript
class ProductCard extends HTMLElement {
    constructor() {
        super();
        // Initialize component
    }
}

customElements.define('product-card', ProductCard);
```

**Shadow DOM** provides style and DOM encapsulation:

```javascript
class ProductCard extends HTMLElement {
    constructor() {
        super();
        const shadow = this.attachShadow({ mode: 'open' });
        shadow.innerHTML = `
            <style>
                .card { border: 1px solid #ddd; padding: 1rem; }
            </style>
            <div class="card">
                <slot></slot>
            </div>
        `;
    }
}
```

**HTML Templates** define reusable fragments that are not rendered until cloned:

```html
<template id="card-template">
    <style>
        .card { border: 1px solid #ddd; }
    </style>
    <div class="card">
        <img class="card-image" src="" alt="">
        <h3 class="card-title"></h3>
        <p class="card-price"></p>
    </div>
</template>
```

### Browser Support

All modern browsers support Web Components natively since 2020. Safari was the last to catch up (full support in Safari 13.1). Polyfills exist but are rarely needed now.

### Web Components vs Frameworks

**Use Web Components for:** design systems/component libraries that work across frameworks, widgets embedded in third-party pages, micro-frontends with different frameworks, progressive enhancement of vanilla HTML pages.

**Use a framework for:** complex application state management, server-side rendering (SSR with WC requires extra tooling), large teams with existing framework investment.

**Framework interoperability:**

```html
<!-- Same Web Component works in all frameworks -->

<!-- In React -->
<my-counter count="0"></my-counter>

<!-- In Vue -->
<my-counter :count="count"></my-counter>

<!-- In Angular -->
<my-counter [count]="count"></my-counter>

<!-- In vanilla HTML -->
<my-counter count="0"></my-counter>
```

### Design Principles

**Single responsibility** — each component does one thing well.

**Encapsulation first** — use Shadow DOM for style isolation instead of global CSS classes.

**Attribute-driven configuration** — use attributes for declarative configuration:

```html
<my-button variant="primary" size="large" disabled>Click me</my-button>
```

**Progressive enhancement** — components should work without JavaScript when possible.

### Naming Conventions

Custom element names **must** contain a hyphen (`my-component`, `x-foo`). This prevents collisions with built-in elements. Use a project-specific prefix (`app-`, `ds-`) and avoid reserved names like `annotation-xml`.

### Lifecycle Overview

Custom elements have four lifecycle callbacks:

| Callback | When it fires | Purpose |
|---|---|---|
| `constructor()` | Element is created | Initialize state, create shadow DOM |
| `connectedCallback()` | Element is added to DOM | Set up event listeners, fetch data, start timers |
| `disconnectedCallback()` | Element is removed from DOM | Clean up: remove listeners, clear timers |
| `attributeChangedCallback()` | Observed attribute changes | React to attribute changes |
| `adoptedCallback()` | Element is moved to a new document | Rarely used |

**Lifecycle order:**

```
constructor() → attributeChangedCallback() → connectedCallback()
```

`attributeChangedCallback` may fire before `connectedCallback`, depending on when attributes are set in the DOM.

### Encapsulation

**Shadow DOM** provides three types of encapsulation:

1. **DOM encapsulation** — shadow root is separate from the light DOM
2. **Style encapsulation** — styles inside the shadow root do not leak out, and page styles do not leak in
3. **Scoped events** — event retargeting makes it look like events come from the host element

**Shadow DOM modes:**

```javascript
// Open: accessible from JavaScript via element.shadowRoot
this.attachShadow({ mode: 'open' });

// Closed: not accessible from outside (prevents element.shadowRoot)
this.attachShadow({ mode: 'closed' });
```

Use `mode: 'open'` for almost all cases. Closed mode breaks tooling, testing, and accessibility inspection.

**What crosses the Shadow DOM boundary:**
- **Slotted content** — children of the host element are projected into `<slot>` elements
- **Events** that bubble: `click`, `keydown`, `focus`, `blur` (but retargeted)
- **Focus** — tab order flows through shadow boundaries
- **Inherited CSS properties** — `color`, `font-family`, `line-height` inherit into shadow DOM

**What does NOT cross:**
- Global CSS selectors (`.class`, `#id`)
- CSS from the parent page
- IDs inside shadow DOM (scoped to the shadow root)

### Styling Strategies

**Inside Shadow DOM — styles are scoped:**

```javascript
const shadow = this.attachShadow({ mode: 'open' });
shadow.innerHTML = `
    <style>
        /* Only applies within this component */
        .container { display: flex; gap: 1rem; }
        .highlight { background: yellow; }
    </style>
    <div class="container">
        <div class="highlight"><slot></slot></div>
    </div>
`;
```

**CSS custom properties (variables) cross the shadow boundary:**

```css
/* Page-level CSS sets a custom property */
my-component {
    --primary-color: blue;
    --border-radius: 8px;
}
```

```javascript
// Component uses the custom property
shadow.innerHTML = `
    <style>
        .card {
            border-radius: var(--border-radius, 4px);
            background: var(--primary-color, gray);
        }
    </style>
`;
```

CSS custom properties are the recommended way to theme Web Components from outside.

**`:host` pseudo-class** styles the component's host element:

```javascript
shadow.innerHTML = `
    <style>
        :host {
            display: inline-block;
            border: 1px solid #ddd;
        }
        :host([disabled]) {
            opacity: 0.5;
            pointer-events: none;
        }
        :host-context(.dark-theme) {
            background: #333;
            color: white;
        }
    </style>
`;
```

**`:host-context()`** styles the component based on the parent context (e.g., dark theme).

**`::part()`** allows external styling of specific parts:

```javascript
shadow.innerHTML = `
    <style>
        .button { /* ... */ }
    </style>
    <button part="button">Submit</button>
`;
```

```css
/* Page-level CSS can style the part */
my-button::part(button) {
    font-size: 1.2rem;
}
```

**`::slotted()`** styles projected content:

```javascript
shadow.innerHTML = `
    <style>
        ::slotted(h2) {
            font-size: 1.5rem;
            margin: 0;
        }
        ::slotted(.highlight) {
            color: var(--highlight-color);
        }
    </style>
    <slot></slot>
`;
```

### Event Handling

**Dispatching custom events:**

```javascript
class MyCounter extends HTMLElement {
    connectedCallback() {
        this.shadowRoot.querySelector('button')
            .addEventListener('click', () => {
                this.dispatchEvent(new CustomEvent('count-change', {
                    detail: { count: this._count },
                    bubbles: true,
                    composed: true,  // Cross shadow boundary
                }));
            });
    }
}
```

**`composed: true`** is required for events to bubble out of the Shadow DOM into the main document.

**Event retargeting:**

When an event crosses a Shadow DOM boundary, the `target` is retargeted to the host element. This prevents internal implementation details from leaking.

**Internal event listeners:**

```javascript
class MyTabs extends HTMLElement {
    connectedCallback() {
        // Listen inside the component
        this.shadowRoot.addEventListener('click', (e) => {
            const tab = e.target.closest('[data-tab]');
            if (tab) this.selectTab(tab.dataset.tab);
        });
    }

    disconnectedCallback() {
        // Clean up event listeners
        // If using addEventListener, save references for removal
    }
}
```

### Attributes and Properties

**Observed attributes:**

```javascript
class UserCard extends HTMLElement {
    static get observedAttributes() {
        return ['name', 'avatar', 'role'];
    }

    attributeChangedCallback(name, oldValue, newValue) {
        if (oldValue === newValue) return;
        this.render();
    }

    render() {
        this.shadowRoot.innerHTML = `
            <div class="card">
                <img src="${this.getAttribute('avatar')}" alt="${this.getAttribute('name')}">
                <h3>${this.getAttribute('name')}</h3>
                <p>${this.getAttribute('role')}</p>
            </div>
        `;
    }
}
```

**Reflecting properties to attributes:**

For a clean API, keep JavaScript properties and HTML attributes in sync:

```javascript
class MyCounter extends HTMLElement {
    get count() {
        return Number(this.getAttribute('count')) || 0;
    }

    set count(value) {
        this.setAttribute('count', value);
    }

    attributeChangedCallback(name, oldValue, newValue) {
        if (name === 'count') {
            this.updateDisplay(newValue);
        }
    }
}
```

**Boolean attributes (presence-based):**

```javascript
class MyButton extends HTMLElement {
    get disabled() {
        return this.hasAttribute('disabled');
    }

    set disabled(value) {
        if (value) {
            this.setAttribute('disabled', '');
        } else {
            this.removeAttribute('disabled');
        }
    }
}
```

### Slots and Composition

Slots allow children of the host element to be projected into the Shadow DOM. Named slots match by `slot` attribute, and the unnamed `<slot>` catches all other children:

```javascript
shadow.innerHTML = `
    <div class="card">
        <header><slot name="title">Default Title</slot></header>
        <div class="content"><slot></slot></div>
        <footer><slot name="actions"></slot></footer>
    </div>
`;
```

```html
<custom-card>
    <h2 slot="title">Card Title</h2>
    <p>Main content (goes to default slot).</p>
</custom-card>
```

Named slots with no matching content display their fallback text. Listen for slot changes with the `slotchange` event on the slot element.

**Projected content belongs to the light DOM** — it is not part of the shadow tree. Styles from the page apply to projected content; component styles do not (unless using `::slotted()`).

### Form Participation

**`ElementInternals`** allows custom elements to participate in forms:

```javascript
class MyInput extends HTMLElement {
    static formAssociated = true;

    constructor() {
        super();
        this.internals = this.attachInternals();
    }

    get value() {
        return this.getAttribute('value') || '';
    }

    set value(val) {
        this.setAttribute('value', val);
        this.internals.setFormValue(val);
    }

    // Validity
    get validity() {
        return this.internals.validity;
    }

    checkValidity() {
        return this.internals.checkValidity();
    }
}

customElements.define('my-input', MyInput);
```

Key form methods:
- `attachInternals()` — returns an `ElementInternals` object
- `internals.setFormValue(value)` — sets the element's form value
- `internals.setValidity(flags, message)` — sets validation state
- `internals.form` — reference to the parent `<form>`
- `internals.labels` — associated `<label>` elements

Built-in form features available:
- Form submission (value included in `FormData`)
- Form validation (`checkValidity`, `reportValidity`)
- Labels and accessibility
- Disabled state (inherits from `<fieldset>`)

### Accessibility

**Use semantic elements inside Shadow DOM:**

```javascript
shadow.innerHTML = `
    <button part="button" aria-label="${this.getAttribute('aria-label')}">
        <slot></slot>
    </button>
`;
```

**Delegating focus:**

```javascript
class MyInput extends HTMLElement {
    constructor() {
        super();
        this.attachShadow({ mode: 'open', delegatesFocus: true });
    }
}
```

`delegatesFocus: true` makes the component focusable as a whole, with focus moving to the first focusable element inside the shadow root.

**ARIA in Shadow DOM:**

ARIA attributes inside Shadow DOM are scoped but participate in the global accessibility tree. Use `aria-label` on the host or inside the shadow root:

```javascript
// Set aria-label on the host element
connectedCallback() {
    if (!this.hasAttribute('aria-label')) {
        this.setAttribute('aria-label', this.getAttribute('name'));
    }
}
```

### Testing Web Components

**Unit testing:**

```javascript
import { html, fixture, expect } from '@open-wc/testing';

describe('my-counter', () => {
    it('renders with default count', async () => {
        const el = await fixture(html`<my-counter></my-counter>`);
        expect(el.shadowRoot.textContent).to.include('0');
    });

    it('increments on click', async () => {
        const el = await fixture(html`<my-counter></my-counter>`);
        el.shadowRoot.querySelector('button').click();
        expect(el.count).to.equal(1);
    });
});
```

**Using `@open-wc/testing`** provides helpers for Web Component testing with any test runner.

**Testing tools:**
- `@open-wc/testing` — test helpers and fixtures
- `web-test-runner` — modern test runner for browsers
- `playwright` or `puppeteer` — integration testing

### Performance Considerations

**Avoid heavy work in `constructor()`:**

```javascript
// Bad: slow initialization
constructor() {
    super();
    this._data = fetchExpensiveData();  // Blocking
}

// Good: defer to connectedCallback
constructor() {
    super();
    this.attachShadow({ mode: 'open' });
}

connectedCallback() {
    // Do async work here
    fetch('/api/data').then(data => this.render(data));
}
```

**Use template cloning instead of innerHTML:**

```javascript
// Fast: clone from template
const template = document.getElementById('my-template');
connectedCallback() {
    this.shadowRoot.appendChild(template.content.cloneNode(true));
}

// Slower: parse innerHTML each time
connectedCallback() {
    this.shadowRoot.innerHTML = `<div>Content</div>`;
}
```

**Batch attribute updates:**

```javascript
// Bad: triggers attributeChangedCallback for each set
element.setAttribute('a', '1');
element.setAttribute('b', '2');
element.setAttribute('c', '3');

// Good: batch updates (use a render queue)
connectedCallback() {
    requestAnimationFrame(() => this.render());
}
```

## Glossary

| Term | Definition |
|---|---|
| Custom Element | User-defined HTML element registered with `customElements.define()` |
| Shadow DOM | Encapsulated DOM subtree with scoped styles |
| Shadow root | Root node of a Shadow DOM tree |
| Light DOM | Children of the host element (projected into slots) |
| Slot | Placeholder in Shadow DOM that projects light DOM content |
| Host element | The custom element that contains the shadow root |
| `attachShadow()` | Method that creates a shadow root on an element |
| `observedAttributes` | Static getter listing attributes that trigger `attributeChangedCallback` |
| `ElementInternals` | Object allowing custom elements to participate in forms |
| `delegatesFocus` | Shadow DOM option that forwards focus to first focusable element |
| `composed` | Event option allowing events to cross shadow boundaries |
| `::part()` | Pseudo-element for styling specific parts of a shadow component |
| `::slotted()` | Pseudo-element for styling projected light DOM content |
| `:host` | Pseudo-class matching the custom element host |

## Quick References

- [MDN: Web Components](https://developer.mozilla.org/en-US/docs/Web/API/Web_components) — comprehensive reference
- [Custom Elements Spec](https://html.spec.whatwg.org/multipage/custom-elements.html) — official specification
- [open-wc](https://open-wc.org/) — Web Component development tooling and recommendations
- [Lit](https://lit.dev/) — lightweight library for building Web Components
- [Web Components Gallery](https://www.webcomponents.org/) — community component repository

## Next Steps

- [Custom Elements in Depth](custom-elements.md) — detailed lifecycle and API
- [Shadow DOM](shadow-dom.md) — encapsulation, slots, and styling
- [Templates and Slots](templates-and-slots.md) — declarative markup patterns