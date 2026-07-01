# DOM Manipulation

## Description

The Document Object Model (DOM) is the browser's tree representation of an HTML document. JavaScript can query, traverse, create, modify, and delete elements — this is how every interactive web page works under the hood.

## Prerequisites

- [What Is JavaScript?](intro/what-is-javascript.md) — browser runtime context
- [Values & Types](values-and-types.md) — objects, references, null checks

## Table of Contents

- [The DOM Tree](#the-dom-tree)
- [Selecting Elements](#selecting-elements)
- [Traversing the DOM](#traversing-the-dom)
- [Creating & Inserting Elements](#creating--inserting-elements)
- [Modifying Elements](#modifying-elements)
- [Removing Elements](#removing-elements)
- [Working with Attributes](#working-with-attributes)
- [Working with Classes](#working-with-classes)
- [Working with Styles](#working-with-styles)
- [Text Content & innerHTML](#text-content--innerhtml)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The DOM Tree

When the browser loads an HTML page, it parses the markup into a tree of **nodes**. Each HTML element becomes an `Element` node, text becomes a `Text` node, and attributes become `Attr` nodes.

```
Document
 └── html
      ├── head
      │    ├── title
      │    └── meta
      └── body
           ├── header
           │    └── h1
           ├── main
           │    ├── section
           │    │    ├── h2
           │    │    └── p
           │    └── section
           │         └── ul
           │              ├── li
           │              ├── li
           │              └── li
           └── footer
```

Every node is backed by an object that inherits from `Node`. Element nodes inherit from `Element → HTMLElement → specific interfaces like `HTMLDivElement`.

```javascript
document instanceof Document;          // true
document.body instanceof HTMLElement;   // true
document.body instanceof HTMLBodyElement; // true
```

The `document` object is the entry point — it represents the entire page.

### Selecting Elements

Modern browsers provide five primary selection methods.

**`getElementById`** — fastest, returns a single element or `null`:

```javascript
const header = document.getElementById("header");
```

**`querySelector`** — returns the first match for any CSS selector:

```javascript
const firstButton = document.querySelector(".btn");
const mainHeading = document.querySelector("h1");
const form = document.querySelector("#login-form input[type='email']");
```

**`querySelectorAll`** — returns a static `NodeList` of all matches:

```javascript
const allButtons = document.querySelectorAll("button");
const listItems = document.querySelectorAll("ul li.active");
```

`NodeList` is array-like but not an array. Convert it with `Array.from()` or spread:

```javascript
const items = document.querySelectorAll("li");
items.forEach(item => item.classList.add("highlight"));

const arr = [...items];                // true array
arr.map(item => item.textContent);     // Array methods work
```

**`getElementsByClassName`** — returns a live `HTMLCollection`:

```javascript
const cards = document.getElementsByClassName("card");
```

Live collections update automatically when the DOM changes. This can cause unexpected behavior during iteration.

**`getElementsByTagName`** — live collection by tag name:

```javascript
const paragraphs = document.getElementsByTagName("p");
```

Prefer `querySelector` / `querySelectorAll` in modern code. They are expressive, return static snapshots, and use familiar CSS syntax.

### Traversing the DOM

Once you have a node, navigate the tree with traversal properties.

**Parent traversal:**

```javascript
const child = document.querySelector(".child");
child.parentElement;                    // parent Element node
child.parentNode;                       // parent Node (may be non-element)
child.closest(".container");           // nearest ancestor matching selector
```

`closest` walks up the tree until it finds a match or reaches the root. Useful for event delegation.

**Child traversal:**

```javascript
const parent = document.querySelector(".parent");
parent.children;                        // HTMLCollection of child elements
parent.firstElementChild;               // first child element
parent.lastElementChild;                // last child element
parent.childNodes;                      // NodeList including text nodes
parent.hasChildNodes();                 // boolean
```

**Sibling traversal:**

```javascript
const current = document.querySelector(".current");
current.nextElementSibling;             // next sibling element
current.previousElementSibling;         // previous sibling element
current.nextSibling;                    // next node (may be text)
current.previousSibling;                // previous node
```

**Checking containment:**

```javascript
parent.contains(child);                 // true if child is descendant
child.isConnected;                      // true if element is in the document
```

### Creating & Inserting Elements

**Creating elements:**

```javascript
const div = document.createElement("div");
const p = document.createElement("p");
const text = document.createTextNode("Hello");
```

**Appending children — modern API:**

```javascript
const list = document.querySelector("ul");

const li = document.createElement("li");
li.textContent = "Item 3";
list.append(li);                        // at end (accepts multiple nodes/strings)

const first = document.createElement("li");
first.textContent = "Item 0";
list.prepend(first);                    // at beginning

const second = document.createElement("li");
second.textContent = "Item 1.5";
list.children[1].after(second);        // after a specific child
list.children[1].before(first);        // before a specific child
```

The modern `append`, `prepend`, `after`, `before`, and `replaceWith` methods are more flexible than older API:

```javascript
// Old way
parent.appendChild(child);
parent.insertBefore(child, referenceChild);
parent.replaceChild(newChild, oldChild);

// Modern equivalent
parent.append(child);
referenceChild.before(child);
oldChild.replaceWith(newChild);
```

**Inserting adjacent HTML:**

```javascript
const target = document.querySelector(".target");
target.insertAdjacentHTML("beforebegin", "<p>Before</p>");
target.insertAdjacentHTML("afterbegin", "<p>First child</p>");
target.insertAdjacentHTML("beforeend", "<p>Last child</p>");
target.insertAdjacentHTML("afterend", "<p>After</p>");
```

`insertAdjacentHTML` parses the string and inserts the resulting nodes without re-serializing existing elements. It is more efficient than setting `innerHTML`.

**Clone nodes:**

```javascript
const original = document.querySelector(".card");
const shallow = original.cloneNode(false);   // clone without children
const deep = original.cloneNode(true);       // clone with all descendants
```

### Modifying Elements

**Text content:**

```javascript
const el = document.querySelector("p");
el.textContent = "Updated text";            // safe, doesn't parse HTML
el.innerText = "Shorthand text";            // aware of rendering/styles
```

`textContent` is generally preferred — it's faster and returns the full text regardless of CSS visibility.

**HTML content:**

```javascript
const container = document.querySelector(".container");
container.innerHTML = "<strong>Bold text</strong>";
container.outerHTML = "<div class='replaced'>Full replacement</div>";
```

**Security warning:** `innerHTML` re-parses the string. If the string contains user input, XSS vulnerabilities follow:

```javascript
const userInput = "<img src=x onerror=alert('xss')>";
container.innerHTML = userInput;            // DANGER
```

Always sanitize user input or use `textContent` when you only need text.

### Removing Elements

**Modern removal:**

```javascript
const el = document.querySelector(".to-remove");
el.remove();                                // removes itself from the tree
```

**Older API:**

```javascript
const parent = el.parentElement;
parent.removeChild(el);                     // requires parent reference
```

**Remove all children:**

```javascript
const container = document.querySelector(".container");
container.innerHTML = "";                   // quick but re-parses
while (container.firstChild) {
    container.removeChild(container.firstChild); // safer
}
container.replaceChildren();                // modern no-arg clear
```

### Working with Attributes

**Standard attributes:**

```javascript
const input = document.querySelector("input");
input.getAttribute("type");                 // "text"
input.setAttribute("type", "email");
input.hasAttribute("disabled");             // boolean
input.removeAttribute("disabled");
```

**Boolean attributes** — presence alone means true:

```javascript
input.disabled = true;                      // adds disabled attribute
input.disabled = false;                     // removes it
input.checked = true;
input.readOnly = false;
```

**Data attributes** — accessed via `dataset`:

```javascript
// <div data-user-id="42" data-role="admin"></div>
const div = document.querySelector("[data-user-id]");
div.dataset.userId;                         // "42"
div.dataset.role;                           // "admin"

div.dataset.userId = "99";                  // updates attribute
div.dataset.newField = "value";            // creates data-new-field
```

The `dataset` property converts kebab-case to camelCase: `data-user-name` becomes `dataset.userName`.

**ARIA attributes:**

```javascript
button.setAttribute("aria-expanded", "true");
button.setAttribute("aria-label", "Close dialog");
```

### Working with Classes

The `classList` API is the modern, ergonomic way to manage classes:

```javascript
const el = document.querySelector(".card");
el.classList.add("active");                 // add class
el.classList.remove("hidden");              // remove class
el.classList.toggle("dark-mode");           // add/remove toggle
el.classList.contains("active");            // boolean check
el.classList.replace("old", "new");         // replace one class with another
```

Multiple classes at once:

```javascript
el.classList.add("foo", "bar", "baz");
el.classList.remove("foo", "bar");
```

Using `className` (older, replaces all classes):

```javascript
el.className = "card active highlighted";   // overwrites all classes
```

### Working with Styles

**Inline styles via `style` property:**

```javascript
const el = document.querySelector(".box");
el.style.color = "red";
el.style.backgroundColor = "#f0f0f0";      // camelCase property names
el.style.fontSize = "16px";                // must include units
el.style.cssText = "color: red; background: blue;"; // set multiple at once
```

The `style` property only reflects **inline** styles, not computed styles.

**Computed styles:**

```javascript
const styles = getComputedStyle(el);
console.log(styles.color);                  // "rgb(255, 0, 0)"
console.log(styles.fontSize);               // "16px"
console.log(styles.getPropertyValue("--custom-prop")); // CSS custom properties
```

**Reading dimensions:**

```javascript
const rect = el.getBoundingClientRect();
rect.top;                                   // distance from viewport top
rect.left;
rect.width;                                 // element width including border
rect.height;
rect.bottom;
rect.right;

el.offsetWidth;                             // width including border
el.offsetHeight;
el.clientWidth;                             // width excluding border
el.clientHeight;
el.scrollHeight;                            // total scrollable height
```

**Checking element visibility:**

```javascript
el.checkVisibility();                       // true if visible (CSS + layout)
document.hidden;                            // true if tab is hidden
document.visibilityState;                   // "visible" or "hidden"
```

### Text Content & innerHTML

Understanding the differences between text access methods:

```javascript
// <div>Hello <span>World</span></div>
const div = document.querySelector("div");

div.textContent;                            // "Hello World" — all text, no HTML
div.innerText;                              // "Hello World" — aware of layout, style
div.innerHTML;                              // 'Hello <span>World</span>'
div.outerHTML;                              // '<div>Hello <span>World</span></div>'
```

- `textContent` — raw text of all child nodes, fastest
- `innerText` — respects CSS (no hidden text), triggers reflow
- `innerHTML` — serialized HTML (read) or parsed HTML (write)
- `outerHTML` — includes the element's own tags

## Glossary

| Term | Definition |
|------|------------|
| DOM | Document Object Model — tree representation of an HTML document |
| Node | The base type for all objects in the DOM tree (elements, text, comments) |
| Element | A node that represents an HTML element |
| NodeList | An array-like collection of nodes, usually returned by querySelectorAll |
| HTMLCollection | A live collection of elements that updates with the DOM |
| Live collection | A collection that automatically updates when the DOM changes |
| Static collection | A snapshot of the DOM at the time of query — does not update |
| Traversal | Navigating the DOM tree (parent, child, sibling relationships) |
| XSS | Cross-Site Scripting — injecting malicious scripts via unsanitized input |
| IntersectionObserver | API for detecting when elements enter/exit the viewport |
| Attribute | A property defined in HTML markup (e.g., `class`, `id`, `data-*`) |

## Quick References

- [MDN: Document](https://developer.mozilla.org/en-US/docs/Web/API/Document) — complete document API reference
- [MDN: Element](https://developer.mozilla.org/en-US/docs/Web/API/Element) — element methods and properties
- [MDN: Node](https://developer.mozilla.org/en-US/docs/Web/API/Node) — node-level API
- [MDN: DOM Manipulation Guide](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Client-side_web_APIs/Manipulating_documents)

## Next Steps

- [Events](events.md) — handling user interaction through events
- [Promises & Async/Await](promises-async-await.md) — asynchronous DOM updates
- [HTML & CSS Fundamentals](../../html/index.md) — the markup and styles DOM manipulates
