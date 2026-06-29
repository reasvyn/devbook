# Events

## Description

Events are the browser's way of saying "something happened" — a click, a keypress, a form submission, a network response. JavaScript listens for these events and runs code in response, making pages interactive.

## Prerequisites

- [DOM Manipulation](dom-manipulation.md) — selecting and creating elements
- [Functions in Depth](functions-in-depth.md) — callbacks, this binding

## Table of Contents

- [The Event Model](#the-event-model)
- [addEventListener](#addeventlistener)
- [Removing Listeners](#removing-listeners)
- [The Event Object](#the-event-object)
- [Event Propagation](#event-propagation)
- [Event Delegation](#event-delegation)
- [Default Behavior](#default-behavior)
- [Custom Events](#custom-events)
- [Mouse Events](#mouse-events)
- [Keyboard Events](#keyboard-events)
- [Form Events](#form-events)
- [Focus Events](#focus-events)
- [Scroll & Resize Events](#scroll--resize-events)
- [Lifecycle Events](#lifecycle-events)
- [Performance Considerations](#performance-considerations)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Event Model

Browsers implement an **event-driven architecture**. JavaScript runs in a single thread, but the event loop processes events one at a time as they occur.

```
User clicks  →  Browser generates event object
             →  Event travels through capture phase
             →  Event reaches target
             →  Event bubbles up through bubble phase
             →  Listener callbacks execute
```

There are three ways to register event listeners:

**HTML attribute (avoid):**

```html
<button onclick="handleClick()">Click</button>
```

This mixes markup and logic, creates global function pollution, and only supports one listener per event.

**DOM property (legacy):**

```javascript
const btn = document.querySelector("button");
btn.onclick = handleClick;                 // only one listener
btn.onclick = null;                        // remove
```

Still limited to one listener.

**addEventListener (modern):**

```javascript
btn.addEventListener("click", handleClick); // unlimited listeners
```

This is the recommended approach. It allows multiple listeners, fine-grained control over propagation, and easy removal.

### addEventListener

The signature:

```javascript
element.addEventListener(type, listener, options);
```

- `type` — event name string (e.g., `"click"`, `"keydown"`)
- `listener` — callback function
- `options` — optional object or boolean for `useCapture`

**Options object:**

```javascript
element.addEventListener("click", handler, {
    capture: false,      // use capture phase instead of bubble
    once: true,          // auto-remove after first invocation
    passive: true,        // hint that preventDefault() won't be called
    signal: abortSignal   // AbortController signal for removal
});
```

**`once`** — listen fires once then removes itself:

```javascript
const welcomeBtn = document.querySelector("#welcome");
welcomeBtn.addEventListener("click", () => {
    alert("Welcome!");                     // fires only once
}, { once: true });
```

**`passive`** — improves scroll performance by telling the browser the listener will not call `preventDefault()`:

```javascript
document.addEventListener("touchstart", handler, { passive: true });
```

**Arrow functions vs. regular functions:**

```javascript
element.addEventListener("click", function() {
    console.log(this);                     // element (the target)
});

element.addEventListener("click", () => {
    console.log(this);                     // outer context (not the element)
});
```

When you need `this` to refer to the element, use a regular function or access `event.currentTarget`.

### Removing Listeners

To remove a listener, you must pass the **same function reference** that was added:

```javascript
function handler() { console.log("clicked"); }

element.addEventListener("click", handler);
element.removeEventListener("click", handler);  // works
```

Anonymous functions cannot be removed:

```javascript
element.addEventListener("click", () => {});    // cannot remove
```

**Using AbortController** — remove multiple listeners at once:

```javascript
const controller = new AbortController();
const { signal } = controller;

element.addEventListener("click", handler1, { signal });
element.addEventListener("mouseenter", handler2, { signal });
element.addEventListener("mouseleave", handler3, { signal });

// Remove all three at once
controller.abort();
```

### The Event Object

Every listener receives an `Event` object with information about the event:

```javascript
element.addEventListener("click", (event) => {
    event.type;                              // "click"
    event.target;                            // element that triggered the event
    event.currentTarget;                     // element the listener is on
    event.eventPhase;                        // 1=capture, 2=at target, 3=bubble
    event.timeStamp;                         // time when event occurred
    event.isTrusted;                         // true if user/browser generated
});
```

**`event.target` vs `event.currentTarget`:**

```html
<div id="parent">
    <button id="child">Click</button>
</div>
```

```javascript
document.querySelector("#parent").addEventListener("click", (e) => {
    e.target;             // <button> — what was clicked
    e.currentTarget;      // <div> — what has the listener
});
```

**Mouse event properties:**

```javascript
element.addEventListener("click", (e) => {
    e.clientX;            // viewport X coordinate
    e.clientY;            // viewport Y coordinate
    e.pageX;              // document X coordinate (includes scroll)
    e.pageY;              // document Y coordinate
    e.screenX;            // screen X coordinate
    e.screenY;            // screen Y coordinate
    e.button;             // 0=left, 1=middle, 2=right
    e.altKey;             // Alt held?
    e.ctrlKey;            // Ctrl held?
    e.shiftKey;           // Shift held?
    e.metaKey;            // Meta (Cmd on Mac, Win on PC)
});
```

**Keyboard event properties:**

```javascript
element.addEventListener("keydown", (e) => {
    e.key;                // "Enter", "a", "ArrowUp", "Escape"
    e.code;               // "KeyA", "ArrowUp", "Enter"
    e.repeat;             // true if held down
});
```

### Event Propagation

When an event fires on a nested element, it travels through three phases:

1. **Capture phase** — event travels from `document` down to the target
2. **Target phase** — event reaches the target element
3. **Bubble phase** — event travels back up from target to `document`

```
document
  └── html
       └── body
            └── div#parent      ← capture going down → ← bubble coming up
                 └── button     ← target
```

**Capture phase listeners** trigger during the descent:

```javascript
document.addEventListener("click", () => {
    console.log("document (capture)");
}, { capture: true });

document.querySelector("#parent").addEventListener("click", () => {
    console.log("parent (capture)");
}, { capture: true });

document.querySelector("#child").addEventListener("click", () => {
    console.log("child (target)");
});                                    // default: bubble phase
```

Clicking `#child` logs:
```
document (capture)
parent (capture)
child (target)
parent (bubble)     ← if parent has bubble listener
document (bubble)   ← if document has bubble listener
```

**Stopping propagation:**

```javascript
child.addEventListener("click", (e) => {
    e.stopPropagation();                // prevents further propagation
});

child.addEventListener("click", (e) => {
    e.stopImmediatePropagation();       // also stops other listeners on same element
});
```

Use `stopPropagation` sparingly. It can break event delegation and make debugging harder.

### Event Delegation

Instead of attaching a listener to every child, attach one to a parent and use `event.target` to determine which child was clicked:

```javascript
// Instead of this:
document.querySelectorAll("li").forEach(li => {
    li.addEventListener("click", () => li.classList.toggle("done"));
});

// Do this:
document.querySelector("ul").addEventListener("click", (e) => {
    if (e.target.tagName === "LI") {
        e.target.classList.toggle("done");
    }
});
```

**Benefits:**
- Single listener, less memory
- Automatically handles dynamically added elements
- Better performance for large lists

**Filtering with `matches`:**

```javascript
document.querySelector("#toolbar").addEventListener("click", (e) => {
    const btn = e.target.closest("button");
    if (!btn) return;                       // not a button or child of button

    if (btn.matches(".save")) save();
    else if (btn.matches(".delete")) deleteItem();
    else if (btn.matches(".edit")) editItem();
});
```

### Default Behavior

Some events have default browser actions. Prevent them with `preventDefault()`:

```javascript
document.querySelector("form").addEventListener("submit", (e) => {
    e.preventDefault();                    // stop form submission
    // handle via AJAX/fetch instead
});

document.querySelector("a").addEventListener("click", (e) => {
    e.preventDefault();                    // prevent navigation
});

document.addEventListener("contextmenu", (e) => {
    e.preventDefault();                    // disable right-click menu
});
```

`preventDefault()` does not stop propagation. The event still bubbles.

Check if `preventDefault` was called:

```javascript
element.addEventListener("click", (e) => {
    e.preventDefault();
    console.log(e.defaultPrevented);       // true
});
```

### Custom Events

Create and dispatch your own events for clean component communication:

```javascript
// Create
const event = new CustomEvent("user-login", {
    detail: { userId: 42, username: "alice" },
    bubbles: true,
    cancelable: true
});

// Dispatch
document.dispatchEvent(event);

// Listen
document.addEventListener("user-login", (e) => {
    console.log(e.detail);                 // { userId: 42, username: "alice" }
});
```

**Component example:**

```javascript
class Dropdown {
    constructor(el) {
        this.el = el;
        this.el.addEventListener("click", () => this.toggle());
    }

    open() {
        this.el.classList.add("open");
        this.el.dispatchEvent(new CustomEvent("dropdown-open", {
            bubbles: true,
            detail: { dropdown: this }
        }));
    }

    close() {
        this.el.classList.remove("open");
        this.el.dispatchEvent(new CustomEvent("dropdown-close", {
            bubbles: true,
            detail: { dropdown: this }
        }));
    }

    toggle() {
        this.el.classList.contains("open") ? this.close() : this.open();
    }
}
```

### Mouse Events

| Event | Description |
|-------|-------------|
| `click` | Full mousedown + mouseup cycle on same element |
| `dblclick` | Two clicks quickly |
| `mousedown` | Button pressed |
| `mouseup` | Button released |
| `mousemove` | Cursor moves over element |
| `mouseenter` | Cursor enters element (does not bubble) |
| `mouseleave` | Cursor leaves element (does not bubble) |
| `mouseover` | Cursor enters element or child (bubbles) |
| `mouseout` | Cursor leaves element or child (bubbles) |
| `contextmenu` | Right-click to open context menu |

**Double-click vs. two clicks:** `dblclick` fires after two `click` events. Use a delay pattern if you need to distinguish:

```javascript
let clickTimer = null;

element.addEventListener("click", (e) => {
    if (clickTimer) {
        clearTimeout(clickTimer);
        clickTimer = null;
        handleDoubleClick(e);              // treat as double-click
    } else {
        clickTimer = setTimeout(() => {
            clickTimer = null;
            handleSingleClick(e);          // treat as single-click
        }, 250);
    }
});
```

### Keyboard Events

| Event | Description |
|-------|-------------|
| `keydown` | Key pressed down (repeats while held) |
| `keyup` | Key released |
| `keypress` | Deprecated — use `keydown` |

**Key properties:**

```javascript
document.addEventListener("keydown", (e) => {
    if (e.key === "Escape") {
        closeModal();
    }

    if (e.key === "Enter" && e.target.matches("input")) {
        submitForm();
    }

    // Ctrl+S to save
    if ((e.ctrlKey || e.metaKey) && e.key === "s") {
        e.preventDefault();
        saveDocument();
    }
});
```

**Modifier check:**

```javascript
if (e.shiftKey && e.key === "ArrowUp") { /* select up */ }
if (e.ctrlKey && e.key === "z") { /* undo */ }
if (e.altKey && e.key === "F4") { /* close */ }
```

### Form Events

| Event | Description |
|-------|-------------|
| `submit` | Form submitted |
| `reset` | Form reset |
| `change` | Value changed and committed (blur for text inputs) |
| `input` | Value changed immediately (every keystroke) |
| `focus` | Element received focus |
| `blur` | Element lost focus |
| `invalid` | Form validation failed |

**Input events in practice:**

```javascript
const input = document.querySelector("#username");
const feedback = document.querySelector("#feedback");

input.addEventListener("input", () => {
    feedback.textContent = input.value.length >= 3
        ? "✓ Valid"
        : "✗ Minimum 3 characters";
});

input.addEventListener("change", () => {
    // fires when value is committed (e.g., on blur)
    validateWithServer(input.value);
});
```

**Form validation:**

```javascript
document.querySelector("form").addEventListener("submit", (e) => {
    const email = document.querySelector("#email");
    if (!email.validity.valid) {
        e.preventDefault();
        email.reportValidity();            // show browser validation UI
    }
});
```

### Focus Events

```javascript
input.addEventListener("focus", () => {
    input.parentElement.classList.add("focused");
});

input.addEventListener("blur", () => {
    input.parentElement.classList.remove("focused");
});

// focusin / focusout — bubble version of focus/blur
document.addEventListener("focusin", (e) => {
    console.log("Focused on:", e.target);
});
```

### Scroll & Resize Events

**Scroll** — fire at high rate, always use throttling or passive listeners:

```javascript
const scrollHandler = () => {
    const scrollY = window.scrollY;
    const nav = document.querySelector("nav");
    nav.classList.toggle("sticky", scrollY > 100);
};

// Passive for performance
window.addEventListener("scroll", scrollHandler, { passive: true });
```

**Resize** — debounce for expensive operations:

```javascript
let resizeTimer;
window.addEventListener("resize", () => {
    clearTimeout(resizeTimer);
    resizeTimer = setTimeout(() => {
        recalculateLayout();               // expensive operation
    }, 200);
});
```

### Lifecycle Events

**DOMContentLoaded** — fires when HTML is parsed, styles and images may still load:

```javascript
document.addEventListener("DOMContentLoaded", () => {
    // Safe to query and manipulate DOM
    initApp();
});
```

**Load** — fires when the full page (including assets) is loaded:

```javascript
window.addEventListener("load", () => {
    // All resources loaded
    console.log("Page fully loaded");
});
```

**BeforeUnload** — confirm before leaving:

```javascript
window.addEventListener("beforeunload", (e) => {
    if (hasUnsavedChanges) {
        e.preventDefault();
        e.returnValue = "";                // modern browsers show default dialog
    }
});
```

**Visibility change** — detect tab switches:

```javascript
document.addEventListener("visibilitychange", () => {
    if (document.hidden) {
        pauseVideo();
    } else {
        resumeVideo();
    }
});
```

### Performance Considerations

**Avoid listener leaks.** Removed elements keep their listeners alive if the listener or its closure references the element:

```javascript
// Memory leak: removed element still referenced in closure
function setup() {
    const el = document.createElement("div");
    el.addEventListener("click", () => {
        console.log(el);                   // closure holds reference to el
    });
    document.body.append(el);
    el.remove();                           // listener still exists
}
```

Use `AbortController` or manually remove listeners when tearing down:

```javascript
function createWidget(container) {
    const controller = new AbortController();
    const { signal } = controller;

    container.addEventListener("click", handler, { signal });

    return {
        destroy() { controller.abort(); }
    };
}
```

**Throttle expensive scroll/resize/mousemove handlers:**

```javascript
function throttle(fn, limit) {
    let inThrottle = false;
    return (...args) => {
        if (!inThrottle) {
            fn(...args);
            inThrottle = true;
            setTimeout(() => { inThrottle = false; }, limit);
        }
    };
}

window.addEventListener("scroll", throttle(() => {
    // expensive calculation
}, 100));
```

## Study Cases

### Case 1: Keyboard Shortcuts for Application

```javascript
class KeyboardShortcuts {
    constructor() {
        this.bindings = new Map();
        document.addEventListener("keydown", (e) => this.handle(e));
    }

    register(combo, handler) {
        this.bindings.set(combo, handler);
    }

    handle(e) {
        const parts = [];
        if (e.ctrlKey || e.metaKey) parts.push("Ctrl");
        if (e.shiftKey) parts.push("Shift");
        if (e.altKey) parts.push("Alt");
        parts.push(e.key === " " ? "Space" : e.key);

        const combo = parts.join("+");
        const handler = this.bindings.get(combo);
        if (handler) {
            e.preventDefault();
            handler(e);
        }
    }
}

const shortcuts = new KeyboardShortcuts();
shortcuts.register("Ctrl+s", () => save());
shortcuts.register("Escape", () => closeModal());
shortcuts.register("Ctrl+Shift+n", () => newNote());
```

### Case 2: Drag-to-Reorder List

```javascript
const list = document.querySelector("#sortable-list");
let dragSrc = null;

list.addEventListener("dragstart", (e) => {
    dragSrc = e.target.closest("li");
    e.dataTransfer.effectAllowed = "move";
    dragSrc.classList.add("dragging");
});

list.addEventListener("dragover", (e) => {
    e.preventDefault();
    const target = e.target.closest("li");
    if (target && target !== dragSrc) {
        const rect = target.getBoundingClientRect();
        const midY = rect.top + rect.height / 2;
        if (e.clientY < midY) {
            list.insertBefore(dragSrc, target);
        } else {
            list.insertBefore(dragSrc, target.nextSibling);
        }
    }
});

list.addEventListener("dragend", () => {
    dragSrc.classList.remove("dragging");
    dragSrc = null;
});
```

### Case 3: Infinite Scroll with IntersectionObserver

```javascript
const sentinel = document.querySelector("#scroll-sentinel");
let page = 1;

const observer = new IntersectionObserver(async (entries) => {
    for (const entry of entries) {
        if (entry.isIntersecting) {
            page++;
            const items = await fetchPage(page);
            renderItems(items);
        }
    }
});

observer.observe(sentinel);

async function fetchPage(pageNum) {
    const res = await fetch(`/api/items?page=${pageNum}`);
    return res.json();
}
```

## Examples

### Example 1: Outside Click Handler

```javascript
function onClickOutside(element, callback) {
    document.addEventListener("click", (e) => {
        if (!element.contains(e.target)) {
            callback(e);
        }
    });
}
```

### Example 2: Event Bus for Component Communication

```javascript
const eventBus = {
    listeners: {},
    on(event, fn) { (this.listeners[event] ||= []).push(fn); },
    off(event, fn) { this.listeners[event] = this.listeners[event]?.filter(f => f !== fn); },
    emit(event, data) { (this.listeners[event] || []).forEach(fn => fn(data)); }
};
```

## Glossary

| Term | Definition |
|------|------------|
| Event | A signal fired by the browser when something happens |
| Listener | A function that runs when an event fires |
| Capture phase | The first phase of propagation — event travels down to target |
| Target phase | The event reaches the element that triggered it |
| Bubble phase | The event travels back up from target to document |
| Event delegation | Attaching a single listener to a parent to handle children |
| Propagation | The flow of an event through the DOM tree |
| preventDefault | Prevents the browser's default action for an event |
| stopPropagation | Prevents the event from traveling further in the tree |
| CustomEvent | Developer-created event with custom data payload |
| Throttle | Limit how often a function can execute |
| Debounce | Delay execution until after a quiet period |
| Passive listener | Hint that preventDefault will not be called (performance) |
| AbortController | API to cancel event listeners or fetch requests |
| isTrusted | Whether an event was generated by the user or by code |

## Quick References

- [MDN: Event Reference](https://developer.mozilla.org/en-US/docs/Web/Events) — complete list of DOM events
- [MDN: addEventListener](https://developer.mozilla.org/en-US/docs/Web/API/EventTarget/addEventListener) — full API reference
- [MDN: Event Delegation](https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Building_blocks/Events#event_delegation)

## Next Steps

- [Fetch API & HTTP](fetch-api.md) — making network requests in response to events
- [Promises & Async/Await](promises-async-await.md) — handling asynchronous event-driven code
- [DOM Manipulation](dom-manipulation.md) — the elements that receive events
