# Custom Elements in Depth

## Description

Custom Elements are the foundation of Web Components, allowing you to define new HTML elements with custom behavior, lifecycle, and APIs using JavaScript classes.

## Prerequisites

- [Web Components Overview](web-components-intro.md) — the three pillars of Web Components
- [Scripts, Defer & Async](scripts-and-defer.md) — JavaScript fundamentals

## Table of Contents

- [Custom Element Types](#custom-element-types)
- [Defining a Custom Element](#defining-a-custom-element)
- [The Constructor](#the-constructor)
- [connectedCallback](#connectedcallback)
- [disconnectedCallback](#disconnectedcallback)
- [attributeChangedCallback](#attributechangedcallback)
- [adoptedCallback](#adoptedcallback)
- [Observed Attributes](#observed-attributes)
- [Properties and Methods](#properties-and-methods)
- [Attribute Reflection](#attribute-reflection)
- [Custom Events](#custom-events)
- [Form-Associated Custom Elements](#form-associated-custom-elements)
- [Extending Built-in Elements](#extending-built-in-elements)
- [Element Upgrade](#element-upgrade)
- [When Elements Are Defined](#when-elements-are-defined)
- [Multiple Definitions and Conflicts](#multiple-definitions-and-conflicts)
- [Lifecycle Pitfalls](#lifecycle-pitfalls)
- [Performance](#performance)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Custom Element Types

There are two types of custom elements:

**Autonomous custom elements** extend `HTMLElement` directly:

```javascript
class MyComponent extends HTMLElement {
    constructor() { super(); }
}
customElements.define('my-component', MyComponent);
```

These are completely new elements with no built-in semantics. Use them for entirely custom widgets.

**Customized built-in elements** extend a specific HTML element:

```javascript
class FancyButton extends HTMLButtonElement {
    constructor() { super(); }
}
customElements.define('fancy-button', FancyButton, { extends: 'button' });
```

These inherit all the semantics, accessibility, and behavior of the built-in element. However, Safari has not implemented customized built-in elements, limiting cross-browser use.

### Defining a Custom Element

**Register with `customElements.define()`:**

```javascript
class Tooltip extends HTMLElement { /* ... */ }
customElements.define('app-tooltip', Tooltip);
```

The name must contain a hyphen, cannot be a single word, and must not be a reserved name.

**Checking if already defined:**

```javascript
if (!customElements.get('app-tooltip')) {
    customElements.define('app-tooltip', Tooltip);
}
```

**Getting a constructor by name:**

```javascript
const Tooltip = customElements.get('app-tooltip');
const tooltip = new Tooltip();
document.body.appendChild(tooltip);
```

### The Constructor

The constructor runs when the element is created (`document.createElement`, `new Element()`, or parser creates the element from HTML).

**Rules for the constructor:**

```javascript
class MyElement extends HTMLElement {
    constructor() {
        super();  // Must call super() first

        // Allowed:
        this.attachShadow({ mode: 'open' });
        this._state = {};
        this.addEventListener('click', this._onClick);

        // NOT allowed:
        // this.appendChild(...)          — use connectedCallback
        // this.innerHTML = '...'         — use connectedCallback
        // this.getAttribute('foo')       — attributes may not be set yet
        // fetch('/api/data')             — use connectedCallback
    }
}
```

**What the constructor should do:**
- Set up initial state
- Create shadow DOM
- Bind event listeners that do not depend on the DOM being connected
- Set up internal data structures

**What the constructor should NOT do:**
- Access or manipulate the DOM (the element is not connected yet)
- Fetch external resources
- Read attributes or children
- Call methods that depend on being in the document

**Constructor and the parser:**

When the HTML parser creates a custom element, the constructor runs. Attributes set in the HTML are not yet available in the constructor. They are set before `connectedCallback` runs.

### connectedCallback

Invoked when the element is added to the document's DOM. Use it to render, set up observers, fetch data, and add event listeners:

```javascript
class MyElement extends HTMLElement {
    connectedCallback() {
        this.render();
        this._observer = new ResizeObserver(() => this.onResize());
        this._observer.observe(this);
    }
}
```

`connectedCallback` may fire multiple times if the element is moved. Cache initialization:

```javascript
connectedCallback() {
    if (this._initialized) return this.updateLayout();
    this._initialized = true;
    this.render();
}
```

Children may not be parsed when `connectedCallback` fires. Use `setTimeout` or `MutationObserver` to access children.

### disconnectedCallback

Invoked when the element is removed from the DOM. Clean up observers, timers, and global listeners:

```javascript
disconnectedCallback() {
    if (this._observer) this._observer.disconnect();
    clearInterval(this._timer);
}
```

Failure to clean up causes memory leaks. `disconnectedCallback` does not fire on page unload — use `beforeunload` or `visibilitychange` for saving state or analytics.

### attributeChangedCallback

Invoked when an observed attribute changes value:

```javascript
class MyElement extends HTMLElement {
    static get observedAttributes() {
        return ['disabled', 'variant', 'size'];
    }

    attributeChangedCallback(name, oldValue, newValue) {
        if (oldValue === newValue) return;

        switch (name) {
            case 'disabled':
                this._updateDisabledState();
                break;
            case 'variant':
                this._updateVariant(newValue);
                break;
            case 'size':
                this._updateSize(newValue);
                break;
        }
    }
}
```

**When it fires:**
1. After the constructor, if the element has attributes in the HTML
2. When `setAttribute()` or `removeAttribute()` is called
3. When `toggleAttribute()` is called

**`oldValue` is `null` on first call** (when the element is first parsed).

**Optimize expensive operations:**

```javascript
attributeChangedCallback(name, oldValue, newValue) {
    if (oldValue === newValue) return;  // Skip no-op changes

    // Debounce render
    if (this._renderPending) return;
    this._renderPending = true;
    requestAnimationFrame(() => {
        this._renderPending = false;
        this.render();
    });
}
```

### adoptedCallback

Invoked when the element is moved to a new document (via `document.adoptNode()` or cross-iframe). Rarely used in practice.

### Observed Attributes

The `observedAttributes` static getter must return an array of attribute names:

```javascript
class MyElement extends HTMLElement {
    static get observedAttributes() {
        return ['checked', 'value', 'label'];
    }
}
```

Only attributes listed in `observedAttributes` trigger `attributeChangedCallback`. Other attribute changes are silent.

**Dynamic observed attributes** — you can observe attrs added after the element is defined, but you must include them in `observedAttributes`:

```javascript
// After this, changes to data-* attrs do NOT trigger callback
myElement.setAttribute('data-custom', 'value');
```

If you need to react to arbitrary attribute changes, use a `MutationObserver` instead.

### Properties and Methods

Custom elements should expose a JavaScript API through properties and methods:

```javascript
class Counter extends HTMLElement {
    constructor() {
        super();
        this._count = 0;
    }

    // Property
    get count() {
        return this._count;
    }

    set count(value) {
        this._count = Number(value);
        this.setAttribute('count', this._count);
        this.render();
    }

    // Method
    increment() {
        this.count++;
    }

    reset() {
        this.count = 0;
    }
}
```

**Property types:**

```javascript
class MyInput extends HTMLElement {
    // Boolean property
    get disabled() {
        return this.hasAttribute('disabled');
    }
    set disabled(value) {
        this.toggleAttribute('disabled', value);
    }

    // String property
    get label() {
        return this.getAttribute('label') || '';
    }
    set label(value) {
        this.setAttribute('label', value);
    }

    // Number property
    get maxLength() {
        return Number(this.getAttribute('max-length')) || Infinity;
    }
    set maxLength(value) {
        this.setAttribute('max-length', value);
    }

    // Array/Object property (does not reflect to attribute)
    get items() {
        return this._items;
    }
    set items(value) {
        this._items = value;
        this.render();
    }
}
```

**Method chaining:**

```javascript
class Counter extends HTMLElement {
    increment() {
        this.count++;
        return this;  // Enable chaining
    }
    reset() {
        this.count = 0;
        return this;
    }
}

// Usage
counter.increment().increment().reset();
```

### Attribute Reflection

**Reflection** keeps the JavaScript property and HTML attribute in sync:

```javascript
class UserCard extends HTMLElement {
    get name() {
        return this.getAttribute('name') || '';
    }

    set name(value) {
        this.setAttribute('name', value);
    }
}
```

Without reflection, setting the property and setting the attribute are disconnected:

```javascript
element.name = 'Alice';       // Only updates property
element.setAttribute('name', 'Bob');  // Only updates attribute
```

**Reflection best practices:**
- Reflect string and number properties to attributes
- Boolean properties reflect as presence-based attributes
- Do not reflect complex types (arrays, objects) to attributes
- Use `JSON.stringify()` for serializable data if needed

**Reflection naming conventions:**

```javascript
// HTML attribute uses kebab-case
// JavaScript property uses camelCase

class MyElement extends HTMLElement {
    // Attribute: max-length → Property: maxLength
    get maxLength() {
        return Number(this.getAttribute('max-length')) || null;
    }
    set maxLength(value) {
        this.setAttribute('max-length', value);
    }
}
```

### Custom Events

Custom elements dispatch events to communicate with the parent page:

```javascript
class Toggle extends HTMLElement {
    connectedCallback() {
        this.addEventListener('click', this._onToggle);
    }

    _onToggle() {
        const isOn = this.hasAttribute('on');
        this.toggleAttribute('on');

        this.dispatchEvent(new CustomEvent('toggle', {
            detail: { on: isOn },
            bubbles: true,
            composed: true,  // Cross shadow boundary
        }));
    }
}
```

**Event patterns:**

```javascript
// Value change event
this.dispatchEvent(new CustomEvent('change', {
    detail: { value: this.value },
    bubbles: true,
    composed: true,
}));

// Action event (user did something)
this.dispatchEvent(new CustomEvent('submit', {
    detail: { data: this.getFormData() },
    bubbles: true,
    composed: true,
}));

// State event (something happened internally)
this.dispatchEvent(new CustomEvent('load', {
    detail: { success: true },
    bubbles: false,  // Internal — do not bubble
}));
```

**Listening from outside:**

```javascript
const toggle = document.querySelector('my-toggle');
toggle.addEventListener('toggle', (e) => {
    console.log('Toggle state:', e.detail.on);
});
```

### Form-Associated Custom Elements

Form-associated custom elements participate in HTML forms, including validation and form submission, via `ElementInternals`:

```javascript
class ColorPicker extends HTMLElement {
    static formAssociated = true;

    constructor() {
        super();
        this.internals = this.attachInternals();
        this.attachShadow({ mode: 'open' });
    }

    get value() { return this.getAttribute('value') || '#000000'; }
    set value(val) {
        this.setAttribute('value', val);
        this.internals.setFormValue(val);
    }

    checkValidity() { return this.internals.checkValidity(); }
    reportValidity() { return this.internals.reportValidity(); }
    get form() { return this.internals.form; }
    get labels() { return this.internals.labels; }
}

customElements.define('color-picker', ColorPicker);
```

Key methods on `ElementInternals`: `setFormValue()` for submitting values, `setValidity(flags, message, anchor)` for validation, and `checkValidity()` / `reportValidity()` for the validity API.

### Extending Built-in Elements

Customized built-in elements inherit all features of a native element. They use the `is` attribute:

```html
<input is="validated-input" type="email" required>
```

**Benefits:** inherits native semantics, form participation, keyboard handling, and accessibility. **Limitation:** Safari has never implemented this, limiting it to Chromium/Firefox-only applications.

### Element Upgrade

Custom elements can be defined before or after they appear in the DOM:

**Definition before usage (synchronous):**

```html
<script>
    class MyElement extends HTMLElement {}
    customElements.define('my-element', MyElement);
</script>
<my-element></my-element>
<!-- Element is immediately upgraded -->
```

**Definition after usage (upgrade):**

```html
<my-element></my-element>
<!-- Element is a generic HTMLElement (not upgraded) -->
<script>
    class MyElement extends HTMLElement {}
    customElements.define('my-element', MyElement);
    // All <my-element> instances are now upgraded
    // constructor() runs retroactively
</script>
```

**`:defined` CSS pseudo-class:**

```css
my-element:not(:defined) {
    display: none;  /* Hide before upgrade */
}

my-element:defined {
    display: block;
}
```

**`customElements.whenDefined()`** returns a promise that resolves when an element is defined:

```javascript
customElements.whenDefined('my-element').then(() => {
    console.log('my-element is now defined');
});

// Or use async/await
await customElements.whenDefined('my-element');
```

This is useful for framework integration where you need to wait for a component to be ready.

### When Elements Are Defined

**Best practice: define at module scope.**

```javascript
// my-element.js
class MyElement extends HTMLElement {}
customElements.define('my-element', MyElement);
```

**Avoid conditional or deferred definition:**

```javascript
// Bad: may cause "already defined" error on re-run
document.addEventListener('DOMContentLoaded', () => {
    customElements.define('my-element', MyElement);
});

// Better: check first
if (!customElements.get('my-element')) {
    customElements.define('my-element', MyElement);
}
```

**Only define once.** Defining the same name twice throws a `DOMException`:

```javascript
try {
    customElements.define('my-element', MyElement);
} catch (e) {
    if (e.name !== 'NotSupportedError') throw e;
}
```

### Multiple Definitions and Conflicts

**Name conflicts:**

```javascript
// Two libraries cannot use the same element name
// Solution: use unique prefixes
// Library A: acme-button
// Library B: corp-button
```

**Framework integration patterns:**

```javascript
// Lazy registration — only define when the component is used
async function defineMyElement() {
    if (customElements.get('my-element')) return;
    const { MyElement } = await import('./my-element.js');
    customElements.define('my-element', MyElement);
}
```

### Lifecycle Pitfalls

1. `connectedCallback` may fire before children are parsed. Access children via `setTimeout(() => ... , 0)`.
2. Moving an element fires `disconnectedCallback` then `connectedCallback` synchronously.
3. Constructor runs on upgrade when elements are defined after DOM appearance.
4. Setting an observed attribute inside `attributeChangedCallback` re-enters the callback. Guard with `oldValue === newValue`.

### Performance

**Use template cloning** instead of `innerHTML` for repeated renders — one parse, many clones. **Batch attribute changes** with `requestAnimationFrame` to avoid redundant renders when multiple attributes change in the same frame.

## Glossary

| Term | Definition |
|---|---|
| Autonomous element | Custom element extending `HTMLElement` directly |
| Customized built-in element | Custom element extending a specific HTML element |
| `customElements.define()` | Method to register a custom element |
| `observedAttributes` | Static getter listing attributes that trigger callbacks |
| `attachInternals()` | Method for form participation via `ElementInternals` |
| Element upgrade | Process of upgrading existing elements when defined later |
| `:defined` | CSS pseudo-class matching defined custom elements |
| `customElements.whenDefined()` | Promise resolving when element is defined |
| Attribute reflection | Syncing JS property and HTML attribute bidirectionally |
| `composed` | Event option for crossing shadow DOM boundaries |

## Quick References

- [MDN: Custom Elements](https://developer.mozilla.org/en-US/docs/Web/API/Web_components/Custom_Elements) — custom elements reference
- [Custom Elements Spec](https://html.spec.whatwg.org/multipage/custom-elements.html) — official specification
- [ElementInternals MDN](https://developer.mozilla.org/en-US/docs/Web/API/ElementInternals) — form participation API
- [Custom Elements Everywhere](https://custom-elements-everywhere.com/) — framework compatibility matrix

## Next Steps

- [Shadow DOM](shadow-dom.md) — encapsulation, slots, and styling
- [Templates and Slots](templates-and-slots.md) — declarative markup patterns
- [Web Components Overview](web-components-intro.md) — intro to all three pillars