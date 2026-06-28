# HTML Templates & Slots

## Description

The `<template>` and `<slot>` elements enable declarative, reusable markup patterns in HTML. Templates hold inert content for later use, while slots project content into Shadow DOM for composable Web Components.

## Prerequisites

- [Shadow DOM](shadow-dom.md) — encapsulation and slot-based composition
- [Custom Elements in Depth](custom-elements.md) — element lifecycle and APIs

## Table of Contents

- [The <template> Element](#the-template-element)
- [Template vs Other Elements](#template-vs-other-elements)
- [Activating Templates](#activating-templates)
- [Templates in Web Components](#templates-in-web-components)
- [The <slot> Element](#the-slot-element)
- [Named Slots](#named-slots)
- [Fallback Content](#fallback-content)
- [The slotchange Event](#the-slotchange-event)
- [The slot Assignment](#the-slot-assignment)
- [Light DOM vs Shadow DOM](#light-dom-vs-shadow-dom)
- [Multiple Elements in One Slot](#multiple-elements-in-one-slot)
- [Slot Styling with ::slotted](#slot-styling-with-slotted)
- [Template Instantiation](#template-instantiation)
- [Templates and Performance](#templates-and-performance)
- [Declarative Shadow DOM with Templates](#declarative-shadow-dom-with-templates)
- [Nested Templates](#nested-templates)
- [Templates in the Browser](#templates-in-the-browser)
- [Template Fragments and Cloning](#template-fragments-and-cloning)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The <template> Element

The `<template>` element holds HTML that is not rendered immediately. It is parsed but not displayed, and its content can be cloned later:

```html
<template id="card-template">
    <style>
        .card { border: 1px solid #ddd; padding: 1rem; border-radius: 4px; }
        .title { font-size: 1.2rem; margin: 0 0 0.5rem; }
    </style>
    <div class="card">
        <h3 class="title"></h3>
        <p class="content"></p>
    </div>
</template>
```

The template's content is invisible in the page. It does not download resources (images, scripts) until activated.

```javascript
const template = document.getElementById('card-template');
const clone = template.content.cloneNode(true);
document.body.appendChild(clone);
```

**Key characteristics:**
- Not rendered by the browser
- Scripts inside do not execute until cloned
- Images and media are not fetched until cloned
- CSS is parsed but does not apply until the template is in the DOM
- The `content` property returns a `DocumentFragment`

### Template vs Other Elements

| Feature | `<template>` | `<div hidden>` | `<script type="text/template">` |
|---|---|---|---|
| Parsed as HTML | Yes | Yes | No (text content) |
| Rendered | No | Yes (hidden) | No |
| Browser parses children | Yes (inert) | Yes (normal) | No (as text) |
| Resources downloaded | No | Yes | No |
| CSS applied | No | Yes | No |
| JavaScript executed | No | No | No |

**Why `<template>`:** It is the only element that leaves HTML fully parsed but completely inert — no resource downloads, no rendering, no script execution.

### Activating Templates

**`template.content`** is a `DocumentFragment`:

```javascript
const template = document.getElementById('my-template');
console.log(template.content);  // DocumentFragment

// Clone the content
const fragment = template.content.cloneNode(true);

// Append to the DOM
document.body.appendChild(fragment);
```

**`cloneNode(true)`** performs a deep clone. Shallow clones (`cloneNode(false)`) copy only the fragment node without children, which is rarely useful.

**Importing templates from another document:**

```javascript
const response = await fetch('/templates.html');
const html = await response.text();
const doc = new DOMParser().parseFromString(html, 'text/html');
const template = doc.querySelector('#my-template');
const clone = template.content.cloneNode(true);
document.body.appendChild(clone);
```

### Templates in Web Components

Templates provide efficient reusable markup for custom elements:

```javascript
// Define template once, outside the class
const template = document.createElement('template');
template.innerHTML = `
    <style>
        :host { display: block; }
        .card { border: 1px solid #ddd; padding: 1rem; }
    </style>
    <div class="card">
        <slot name="title"></slot>
        <slot></slot>
    </div>
`;

class MyCard extends HTMLElement {
    constructor() {
        super();
        this.attachShadow({ mode: 'open' });
        this.shadowRoot.appendChild(template.content.cloneNode(true));
    }
}

customElements.define('my-card', MyCard);
```

This approach parses the template once and clones it for each element instance, which is more performant than setting `innerHTML` in every instance.

### The <slot> Element

`<slot>` is a placeholder inside a shadow tree that projects content from the light DOM:

```javascript
shadow.innerHTML = `
    <div class="card">
        <h2><slot name="heading">Default heading</slot></h2>
        <slot></slot>  <!-- Default slot -->
    </div>
`;
```

```html
<my-card>
    <h2 slot="heading">Custom Heading</h2>
    <p>This goes to the default slot.</p>
    <p>This also goes to the default slot.</p>
</my-card>
```

**How slots work:**
1. The light DOM children of the host element are assigned to slots in the shadow tree
2. Elements with a `slot` attribute matching a named slot's `name` go to that slot
3. Elements without a `slot` attribute go to the unnamed `<slot>`
4. Elements not matching any named slot also fall through to the default slot

### Named Slots

Named slots provide precise placement within the component's structure:

```javascript
shadow.innerHTML = `
    <header>
        <slot name="header"></slot>
    </header>
    <main>
        <slot></slot>
    </main>
    <footer>
        <slot name="footer"></slot>
    </footer>
`;
```

```html
<app-layout>
    <header slot="header">
        <h1>Page Title</h1>
        <nav>...</nav>
    </header>
    <article>
        <p>Main content...</p>
    </article>
    <footer slot="footer">
        <p>&copy; 2026</p>
    </footer>
</app-layout>
```

Named slots can appear in any order in the shadow tree. The `slot` attribute on light DOM elements matches the `name` attribute on `<slot>` elements.

**Multiple elements can share the same slot name:**

```html
<my-list>
    <li slot="item">Item 1</li>
    <li slot="item">Item 2</li>
    <li slot="item">Item 3</li>
</my-list>
```

### Fallback Content

Slots can provide fallback (default) content that displays when no matching light DOM is provided:

```javascript
shadow.innerHTML = `
    <div class="card">
        <slot name="title"><h2>Untitled Card</h2></slot>
        <slot><p>No content provided.</p></slot>
    </div>
`;
```

Fallback content renders inside the shadow tree and is not part of the light DOM. It cannot be styled with `::slotted()`.

**When fallback shows:** Only when there are zero assigned nodes for that slot. If any light DOM element targets the slot (even an empty element like `<span slot="title"></span>`), the fallback is replaced.

**When fallback is hidden:** If a light DOM element assigns to the slot but that element is empty, the fallback content does not appear.

### The slotchange Event

The `slotchange` event fires when the assigned nodes of a slot change:

```javascript
class MyList extends HTMLElement {
    constructor() {
        super();
        this.attachShadow({ mode: 'open' });
        this.shadowRoot.innerHTML = `<slot></slot>`;
    }

    connectedCallback() {
        const slot = this.shadowRoot.querySelector('slot');
        slot.addEventListener('slotchange', (e) => {
            const assigned = e.target.assignedNodes();
            console.log('Assigned nodes:', assigned.length);
            this.updateCount(assigned.length);
        });
    }

    updateCount(count) {
        if (!this.shadowRoot.querySelector('.count')) {
            const countEl = document.createElement('div');
            countEl.className = 'count';
            this.shadowRoot.prepend(countEl);
        }
        this.shadowRoot.querySelector('.count').textContent = `${count} items`;
    }
}
customElements.define('my-list', MyList);
```

**When `slotchange` fires:**
- When the element first connects to the DOM (initial assignment)
- When slotted children are added, removed, or reordered
- When a child's `slot` attribute changes

**When `slotchange` does NOT fire:**
- When attributes of slotted children change (not a slot change)
- When text content of slotted children changes (not a slot change)

### The slot Assignment

Every slot has an **assigned nodes** list — the light DOM elements projected into it:

```javascript
const slot = shadow.querySelector('slot');

// Get assigned nodes (includes text nodes)
slot.assignedNodes();
slot.assignedNodes({ flatten: true });  // Includes fallback content

// Get assigned elements (elements only)
slot.assignedElements();
slot.assignedElements({ flatten: true });  // Includes fallback content
```

**`flatten: true`** returns fallback content when no nodes are assigned.

**Assignment is declarative:**
- The browser, not JavaScript, determines which light DOM goes to which slot
- The matching is based on the `slot` attribute and `<slot name>` attributes
- Re-assigning happens automatically when attributes change

### Light DOM vs Shadow DOM

| Aspect | Light DOM | Shadow DOM |
|---|---|---|
| Owner | The page / parent component | The component |
| Styled by | Page CSS | Component CSS |
| Queried by | `document.querySelector` | `shadowRoot.querySelector` |
| Slotted | Projects into `<slot>` | Contains `<slot>` |
| DOM API | `element.children` | `element.shadowRoot` |

**Event flow between light and shadow:**
- Light DOM events bubble through the slot into the shadow tree
- Shadow DOM events can bubble out if `composed: true`
- Focus can flow across the boundary

### Multiple Elements in One Slot

A single slot can receive multiple light DOM children:

```html
<my-button>
    <img src="icon.svg" class="icon" slot="prefix">
    Click Me
    <span slot="suffix" class="badge">3</span>
</my-button>
```

```javascript
shadow.innerHTML = `
    <style>
        .button { display: inline-flex; align-items: center; gap: 0.5rem; }
    </style>
    <div class="button">
        <slot name="prefix"></slot>
        <slot></slot>
        <slot name="suffix"></slot>
    </div>
`;
```

All light DOM elements with `slot="prefix"` are assigned to the prefix slot. All unnamed content goes to the default slot.

**Ordering:** The browser preserves the order of slotted elements within each slot. Elements from different slots are interleaved based on the shadow DOM structure, not the light DOM order.

### Slot Styling with ::slotted

`::slotted()` styles elements assigned to a slot. It only targets top-level elements:

```javascript
shadow.innerHTML = `
    <style>
        ::slotted(h2) { font-size: 1.5rem; color: var(--title-color, #333); }
        ::slotted(.highlight) { background: yellow; padding: 0.25rem; }
        ::slotted([slot="footer"]) { border-top: 1px solid #eee; margin-top: 1rem; }
    </style>
    <slot name="title"></slot>
    <slot></slot>
    <slot name="footer"></slot>
`;
```

**Limitations:**
- Only direct children of the slot can be styled
- `::slotted(div) span` does not work (combinators not allowed)
- `::slotted(*) p` does not work
- `::slotted()` cannot style fallback content (it is part of shadow DOM)

### Template Instantiation

Templates can be instantiated in multiple ways:

**Clone and append:**

```javascript
const template = document.querySelector('#card');
const clone = template.content.cloneNode(true);
document.querySelector('.container').appendChild(clone);
```

**Clone into Shadow DOM:**

```javascript
class MyElement extends HTMLElement {
    constructor() {
        super();
        const template = document.querySelector('#my-template');
        this.attachShadow({ mode: 'open' })
            .appendChild(template.content.cloneNode(true));
    }
}
```

**Programmatic creation:**

```javascript
const template = document.createElement('template');
template.innerHTML = '<p>Hello, World!</p>';
document.body.appendChild(template.content.cloneNode(true));
```

**Insert adjacent:**

```javascript
const template = document.querySelector('#item');
const clone = template.content.cloneNode(true);
document.querySelector('.list').insertAdjacentElement('beforeend', clone);
```

### Templates and Performance

**One parse, many clones — the performance advantage:**

```javascript
// Slow: parse HTML for every instance
class SlowElement extends HTMLElement {
    connectedCallback() {
        this.shadowRoot.innerHTML = '<div class="card">...</div>';
    }
}

// Fast: parse once, clone many times
const template = document.createElement('template');
template.innerHTML = '<div class="card">...</div>';

class FastElement extends HTMLElement {
    constructor() {
        super();
        this.attachShadow({ mode: 'open' });
        this.shadowRoot.appendChild(template.content.cloneNode(true));
    }
}
```

The template approach avoids HTML parsing cost per instance. For a single element the difference is negligible, but for hundreds or thousands it matters.

**Template caching:**

```javascript
const templateCache = new Map();

function getTemplate(id) {
    if (!templateCache.has(id)) {
        templateCache.set(id,
            document.getElementById(id));
    }
    return templateCache.get(id).content.cloneNode(true);
}
```

### Declarative Shadow DOM with Templates

Declarative Shadow DOM combines `<template>` with `shadowrootmode` to define shadow roots in HTML:

```html
<my-card>
    <template shadowrootmode="open">
        <style>
            .card { border: 1px solid #ddd; padding: 1rem; }
        </style>
        <div class="card">
            <slot name="title">Default</slot>
            <slot></slot>
        </div>
    </template>
    <h2 slot="title">Card Title</h2>
    <p>Content.</p>
</my-card>
```

The browser parses the `<template shadowrootmode="open">` and attaches it as the shadow root. This works without JavaScript, enabling server-side rendering of Web Components.

**Serialization:**

```javascript
// Get HTML with shadow roots included
const html = element.getInnerHTML({ includeShadowRoots: true });

// Can be sent as HTML and parsed back with declarative shadow DOM
```

**Browser support:** Chrome 111+, Safari 16.4+, Firefox 121+.

### Nested Templates

Templates can contain other templates, though nesting is rarely needed:

```html
<template id="outer">
    <div class="outer">
        <template id="inner">
            <p>Inner content</p>
        </template>
    </div>
</template>
```

When cloning the outer template, the inner `<template>` remains inert (a template inside a template does not activate). To activate the inner template, clone it separately.

**Better approach: separate templates.**

```html
<template id="item-template">...</template>
<template id="list-template">
    <div class="list">
        <!-- Items inserted programmatically -->
    </div>
</template>
```

### Templates in the Browser

**How the browser handles `<template>`:**

1. Parsing: the browser parses the template's HTML but does not build render tree nodes
2. Scripts are not executed; the `src` attribute on `<script>` in a template is not fetched
3. Images are not fetched; `<img src="...">` in a template has no network request
4. Styles are parsed but not applied
5. The template's content is a `DocumentFragment` in the `content` property

**`template.content` is read-only and live:**
- `template.content` always returns the same `DocumentFragment`
- Modifying the fragment modifies the template
- Cloning the fragment (`cloneNode(true)`) creates an independent copy

### Template Fragments and Cloning

**DocumentFragment** is a lightweight container:

```javascript
const fragment = template.content.cloneNode(true);
console.log(fragment.nodeType);  // 11 (DocumentFragment)

// Append moves all children into the target
container.appendChild(fragment);
// After append, fragment is empty (children transferred)
```

**Deep clone with `cloneNode(true)`:**

```javascript
const template = document.querySelector('#my-template');
const clone1 = template.content.cloneNode(true);
const clone2 = template.content.cloneNode(true);

// Both are independent
clone1.querySelector('.text').textContent = 'First';
clone2.querySelector('.text').textContent = 'Second';

container.appendChild(clone1);
container.appendChild(clone2);
```

Each clone is a fully independent copy. Changes to one do not affect the template or other clones.

## Study Cases

### Dynamic List Component

A `<dynamic-list>` that renders items from an array using a template:

```javascript
const itemTemplate = document.createElement('template');
itemTemplate.innerHTML = `
    <li class="item">
        <span class="label"></span>
        <button class="remove" aria-label="Remove">×</button>
    </li>
`;

class DynamicList extends HTMLElement {
    constructor() {
        super();
        this.attachShadow({ mode: 'open' });
        this._items = [];
    }

    connectedCallback() {
        this.render();
    }

    set items(value) {
        this._items = value;
        this.render();
    }

    get items() {
        return this._items;
    }

    render() {
        const template = document.createElement('template');
        template.innerHTML = `
            <style>
                .list { list-style: none; padding: 0; margin: 0; }
                .item {
                    display: flex; justify-content: space-between;
                    padding: 0.5rem; border-bottom: 1px solid #eee;
                }
                .remove {
                    background: none; border: none;
                    color: #c00; cursor: pointer; font-size: 1.2rem;
                }
                .empty { color: #999; font-style: italic; }
            </style>
            ${this._items.length === 0
                ? '<p class="empty">No items</p>'
                : `<ul class="list">
                    ${this._items.map((item, i) => `
                        <li class="item">
                            <span>${item}</span>
                            <button class="remove" data-index="${i}"
                                    aria-label="Remove ${item}">×</button>
                        </li>
                    `).join('')}
                   </ul>`
            }
            <slot name="footer"></slot>
        `;
        this.shadowRoot.innerHTML = '';
        this.shadowRoot.appendChild(template.content.cloneNode(true));

        this.shadowRoot.querySelectorAll('.remove').forEach(btn => {
            btn.addEventListener('click', () => {
                const index = Number(btn.dataset.index);
                this._items.splice(index, 1);
                this.render();
                this.dispatchEvent(new CustomEvent('item-removed', {
                    detail: { index }, bubbles: true, composed: true,
                }));
            });
        });
    }
}
customElements.define('dynamic-list', DynamicList);
```

```html
<dynamic-list id="todo-list">
    <p slot="footer">Drag to reorder</p>
</dynamic-list>
<script>
    const list = document.getElementById('todo-list');
    list.items = ['Buy groceries', 'Write docs', 'Review PR'];
</script>
```

### Card Layout with Named Slots

A product card using multiple named slots:

```javascript
const cardTemplate = document.createElement('template');
cardTemplate.innerHTML = `
    <style>
        :host { display: inline-block; }
        .card {
            border: 1px solid #ddd; border-radius: 8px; overflow: hidden;
            width: 300px; font-family: sans-serif;
        }
        .image { width: 100%; height: 200px; object-fit: cover; }
        .body { padding: 1rem; }
        .title { font-size: 1.25rem; margin: 0 0 0.5rem; }
        .price { font-weight: bold; color: #007bff; }
        .actions {
            display: flex; gap: 0.5rem; padding: 1rem; border-top: 1px solid #eee;
        }
        ::slotted([slot="image"]) { width: 100%; display: block; }
    </style>
    <div class="card">
        <slot name="image">
            <img class="image" src="https://via.placeholder.com/300" alt="Placeholder">
        </slot>
        <div class="body">
            <slot name="title"><h2 class="title">Product</h2></slot>
            <slot name="price"><span class="price">$0.00</span></slot>
            <slot name="description"><p>No description</p></slot>
        </div>
        <div class="actions">
            <slot name="actions"></slot>
        </div>
    </div>
`;

class ProductCard extends HTMLElement {
    constructor() {
        super();
        this.attachShadow({ mode: 'open' });
        this.shadowRoot.appendChild(cardTemplate.content.cloneNode(true));
    }
}
customElements.define('product-card', ProductCard);
```

```html
<product-card>
    <img slot="image" src="/product.jpg" alt="Product photo">
    <h2 slot="title">Widget Pro</h2>
    <span slot="price">$29.99</span>
    <p slot="description">High-quality widget with advanced features.</p>
    <button slot="actions">Add to Cart</button>
    <button slot="actions">Wishlist</button>
</product-card>
```

## Examples

### Example 1: Template for repetitive elements

```html
<template id="user-row">
    <tr>
        <td class="name"></td>
        <td class="email"></td>
        <td><button class="edit">Edit</button></td>
    </tr>
</template>

<table id="user-table">
    <thead>
        <tr><th>Name</th><th>Email</th><th>Actions</th></tr>
    </thead>
    <tbody></tbody>
</table>
```

```javascript
const users = [
    { name: 'Alice', email: 'alice@example.com' },
    { name: 'Bob', email: 'bob@example.com' },
];

const template = document.getElementById('user-row');
const tbody = document.querySelector('#user-table tbody');

users.forEach(user => {
    const clone = template.content.cloneNode(true);
    clone.querySelector('.name').textContent = user.name;
    clone.querySelector('.email').textContent = user.email;
    tbody.appendChild(clone);
});
```

### Example 2: Slot fallback for empty states

```javascript
shadow.innerHTML = `
    <style>
        .empty { color: #999; text-align: center; padding: 2rem; }
    </style>
    <slot>
        <div class="empty">
            <p>No content provided.</p>
            <p>Add children to this element to see them here.</p>
        </div>
    </slot>
`;
```

When no light DOM children exist, the fallback content displays. When children are added (even dynamically), the fallback is replaced.

## Glossary

| Term | Definition |
|---|---|
| `<template>` | Element for declarative, inert markup fragments |
| `DocumentFragment` | Lightweight container node, not part of the main DOM tree |
| `content` | Property on `<template>` returning its `DocumentFragment` |
| `<slot>` | Element projecting light DOM content into shadow tree |
| Named slot | `<slot>` with a `name` attribute for targeted projection |
| Fallback content | Default content inside a `<slot>` when no nodes are assigned |
| `slotchange` | Event fired when a slot's assigned nodes change |
| `assignedNodes()` | Method returning nodes projected into a slot |
| `assignedElements()` | Method returning elements projected into a slot |
| `flatten` | Option to include fallback content in slot queries |
| Light DOM | Children of a custom element (outside shadow tree) |

## Quick References

- [MDN: <template>](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/template) — template element reference
- [MDN: <slot>](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/slot) — slot element reference
- [HTML Spec: Templates](https://html.spec.whatwg.org/multipage/scripting.html#the-template-element) — official specification
- [HTML Spec: Slots](https://html.spec.whatwg.org/multipage/dom.html#shadow-tree-slots) — slot specification
- [Declarative Shadow DOM](https://web.dev/articles/declarative-shadow-dom) — HTML-only shadow DOM

## Next Steps

- [Web Components Overview](web-components-intro.md) — overview of all three pillars
- [Shadow DOM](shadow-dom.md) — deeper dive into encapsulation and scoping
- [Custom Elements in Depth](custom-elements.md) — lifecycle and APIs