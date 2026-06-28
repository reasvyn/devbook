# Focus Management

## Description

Keyboard focus management determines how users navigate interactive elements using the Tab key, arrow keys, and programmatic focus control. Proper focus management is critical for users who rely on keyboards or assistive technologies.

## Prerequisites

- [ARIA Basics](aria-basics.md) — ARIA roles and states for focusable widgets
- [Scripts, Defer & Async](scripts-and-defer.md) — JavaScript fundamentals for focus manipulation

## Table of Contents

- [What is Focus?](#what-is-focus)
- [The Tab Order](#the-tab-order)
- [Tabindex Deep Dive](#tabindex-deep-dive)
- [Focusable Elements](#focusable-elements)
- [Managing Focus in JavaScript](#managing-focus-in-javascript)
- [Focus Trapping](#focus-trapping)
- [Focus Styles](#focus-styles)
- [Focus Order in Layout](#focus-order-in-layout)
- [Roving Tabindex](#roving-tabindex)
- [Skip Links](#skip-links)
- [Focus in Single-Page Apps](#focus-in-single-page-apps)
- [Accessibility Requirements](#accessibility-requirements)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What is Focus?

Focus is the visual indicator of which element on the page receives keyboard events. It moves in two ways:

- **Sequential navigation** — pressing Tab moves focus forward, Shift+Tab moves backward
- **Direct navigation** — clicking, tapping, or calling `.focus()` in JavaScript

Users who cannot use a mouse rely entirely on sequential navigation. Keyboard-only users include people with motor disabilities, power users, and screen reader users.

The focused element typically shows a **focus ring** — a visual outline or highlight. Removing this ring without providing an alternative (like `outline: 0` without replacement) makes the page unusable for keyboard users.

### The Tab Order

The default tab order follows the DOM order — elements are focused in the order they appear in the source code. This is almost always the correct order.

```html
<!-- Tab order: first → second → third -->
<input type="text" id="first" placeholder="First">
<input type="text" id="second" placeholder="Second">
<button id="third">Submit</button>
```

**Elements that participate in tab order:**

- Form controls: `<input>`, `<select>`, `<textarea>`, `<button>`
- Links: `<a href="...">`
- Elements with `tabindex="0"` or `tabindex="[positive]"`
- Editable elements: `contenteditable`

**Elements that do not participate:**

- Non-interactive elements: `<div>`, `<span>`, `<p>`
- Disabled form controls: `<input disabled>`
- Elements with `tabindex="-1"` (focusable only via JavaScript)
- Hidden elements: `hidden`, `display: none`, `visibility: hidden`

### Tabindex Deep Dive

The `tabindex` attribute controls keyboard focusability and order.

**`tabindex="0"`** — element is focusable in natural tab order:

```html
<div role="button" tabindex="0" onclick="submit()">Submit</div>
```

The element joins the tab order at its position in the DOM. Use this for custom interactive elements that lack native focusability (like a `<div>` acting as a button).

**`tabindex="-1"`** — element is not reachable via Tab but can receive programmatic focus:

```html
<div id="modal" tabindex="-1" role="dialog" aria-modal="true">
    <!-- Focus can be moved here via JavaScript -->
</div>
```

```javascript
modal.focus();  // Works
```

Use cases:
- Modal dialogs that need to receive focus when opened
- Error messages that should be announced immediately
- Off-screen panels that become visible on interaction

**`tabindex="[positive]"`** — element receives focus in custom order (avoid):

```html
<!-- Bad: unnatural tab order -->
<input tabindex="3" type="text">
<input tabindex="1" type="text">
<input tabindex="2" type="text">
```

Positive tabindex values create a separate tab order that runs before the natural DOM order. This is almost always confusing for users. Use DOM reordering or CSS order instead.

Rule: only use `tabindex="0"` and `tabindex="-1"` in production code.

### Focusable Elements

**Natively focusable elements:**

```html
<!-- Automatically in tab order -->
<a href="/page">Link</a>
<button>Button</button>
<input type="text">
<select><option>Option</option></select>
<textarea></textarea>
```

**Making non-focusable elements focusable:**

```html
<!-- Not focusable -->
<div onclick="submit()">Click me</div>

<!-- Now focusable -->
<div tabindex="0" role="button" onclick="submit()">Click me</div>
```

**Disabled elements are not focusable:**

```html
<button disabled>Cannot focus this</button>
```

To disable an element while keeping it focusable, use `aria-disabled`:

```html
<button tabindex="0" aria-disabled="true">Focusable but visually disabled</button>
```

### Managing Focus in JavaScript

**`element.focus()`** — moves focus to an element:

```javascript
document.getElementById('search-input').focus();
```

Options parameter for scroll behavior:

```javascript
element.focus({ preventScroll: true });  // Do not scroll to element
element.focus({ focusVisible: true });   // Always show focus ring
```

**`document.activeElement`** — returns the currently focused element:

```javascript
if (document.activeElement === submitButton) {
    // Submit button is focused
}
```

**Focus when removing elements.** When an element is removed from the DOM while focused, focus jumps to the `<body>`. Always move focus before removing:

```javascript
function removeElement(element) {
    const nextFocus = element.nextElementSibling || element.previousElementSibling;
    if (nextFocus) {
        nextFocus.focus();
    }
    element.remove();
}
```

**Focus on dynamic content.** When content appears dynamically (like a modal), move focus to it:

```javascript
function openModal(modalId) {
    const modal = document.getElementById(modalId);
    const previousFocus = document.activeElement;
    modal.hidden = false;
    modal.focus();
    // Store previous focus for return
    modal.dataset.previousFocus = previousFocus.id;
}
```

### Focus Trapping

Focus trapping keeps keyboard focus within a container — essential for modals, menus, and slide-out panels.

**Basic focus trap implementation:**

```javascript
function trapFocus(element) {
    const focusableElements = element.querySelectorAll(
        'button, [href], input, select, textarea, [tabindex]:not([tabindex="-1"])'
    );
    const firstFocusable = focusableElements[0];
    const lastFocusable = focusableElements[focusableElements.length - 1];

    element.addEventListener('keydown', (e) => {
        if (e.key !== 'Tab') return;
        if (e.shiftKey) {
            if (document.activeElement === firstFocusable) {
                e.preventDefault();
                lastFocusable.focus();
            }
        } else {
            if (document.activeElement === lastFocusable) {
                e.preventDefault();
                firstFocusable.focus();
            }
        }
    });

    firstFocusable.focus();
}
```

**What focus trapping should handle:**

| Event | Behavior |
|---|---|
| Tab (forward) | Move to next focusable child; wrap to first if on last |
| Shift+Tab (backward) | Move to previous focusable child; wrap to last if on first |
| Escape | Close dialog (if applicable), return focus to trigger |
| Tab on single focusable | Do nothing (stay on element or close) |

**When to trap focus:**

- Modal dialogs
- Slide-out navigation panels
- Off-screen menus
- Date picker popovers
- Autocomplete dropdowns
- Inline editors

### Focus Styles

**Never remove focus styles without replacement:**

```css
/* Bad: removes focus indicator entirely */
*:focus {
    outline: none;
}

/* Good: removes default but adds custom style */
*:focus {
    outline: none;
    box-shadow: 0 0 0 3px #4A90D9;
}

/* Good: use :focus-visible to show ring only when keyboard navigating */
:focus-visible {
    outline: 2px solid #4A90D9;
    outline-offset: 2px;
}

/* Reset non-keyboard focus to avoid ring on click */
:focus:not(:focus-visible) {
    outline: none;
}
```

**`:focus-visible`** is a newer pseudo-class that distinguishes keyboard focus from mouse/click focus:

```css
/* Only keyboard users see the focus ring */
button:focus-visible {
    outline: 3px solid #005FCC;
    outline-offset: 2px;
}

/* Clicking a button does not show a ring */
```

Browser support for `:focus-visible` is good in all modern browsers. For older browser support, use a polyfill or always show focus indicators.

**Custom focus styles for interactive elements:**

```css
a:focus-visible {
    outline: 2px solid currentColor;
    text-decoration: underline dotted;
}

input:focus-visible {
    border-color: #005FCC;
    box-shadow: 0 0 0 3px rgba(0, 95, 204, 0.3);
}

[tabindex]:focus-visible {
    outline: 2px solid #005FCC;
    outline-offset: 4px;
}
```

### Focus Order in Layout

**CSS order does not affect tab order.** The `order` property in Flexbox and Grid changes visual order but not DOM/tab order:

```css
.container {
    display: flex;
    flex-direction: row-reverse;
}

/* Visual order is reversed, but tab order follows DOM */
```

If visual order and tab order must differ, restructure the DOM. Do not rely on CSS for focus order.

**`position: absolute`** and **`position: fixed`** also do not affect tab order. An element moved visually to the top of the page may still be tabbed to last if it appears last in the DOM.

### Roving Tabindex

Roving tabindex is a pattern for keyboard navigation within a set of related widgets (tab panels, menus, radio groups). Only one element in the set has `tabindex="0"`, and others have `tabindex="-1"`. Arrow key events swap the tabindex values.

**Tab list with roving tabindex:**

```html
<div role="tablist" aria-label="Tabs">
    <button role="tab" aria-selected="true" tabindex="0" id="tab-1">Tab 1</button>
    <button role="tab" aria-selected="false" tabindex="-1" id="tab-2">Tab 2</button>
    <button role="tab" aria-selected="false" tabindex="-1" id="tab-3">Tab 3</button>
</div>
```

```javascript
const tablist = document.querySelector('[role="tablist"]');

tablist.addEventListener('keydown', (e) => {
    const tabs = [...tablist.querySelectorAll('[role="tab"]')];
    const currentIndex = tabs.indexOf(document.activeElement);

    let newIndex;
    if (e.key === 'ArrowRight') {
        newIndex = (currentIndex + 1) % tabs.length;
    } else if (e.key === 'ArrowLeft') {
        newIndex = (currentIndex - 1 + tabs.length) % tabs.length;
    } else {
        return;
    }

    e.preventDefault();

    // Remove tabindex from current tab
    tabs[currentIndex].tabIndex = -1;

    // Add tabindex to new tab
    tabs[newIndex].tabIndex = 0;

    // Move focus
    tabs[newIndex].focus();
});
```

Benefits:
- Only one Tab stop for the entire group
- Arrow keys handle internal navigation
- Screen readers announce the group as a single widget

### Skip Links

Skip links allow keyboard users to bypass repetitive navigation and jump to main content:

```html
<body>
    <a href="#main-content" class="skip-link">Skip to main content</a>
    <header>
        <nav aria-label="Main">...</nav>
    </header>
    <main id="main-content">
        <h1>Page Title</h1>
    </main>
</body>
```

```css
.skip-link {
    position: absolute;
    top: -40px;
    left: 0;
    background: #000;
    color: #fff;
    padding: 8px;
    z-index: 100;
    transition: top 0.2s;
}

.skip-link:focus {
    top: 0;
}
```

When the user presses Tab on page load, the skip link is the first focusable element. Clicking it moves focus to `main-content`.

**Multiple skip links on complex pages:**

```html
<a href="#nav" class="skip-link">Skip to navigation</a>
<a href="#main-content" class="skip-link">Skip to content</a>
<a href="#search" class="skip-link">Skip to search</a>
```

### Focus in Single-Page Apps

Single-page applications require explicit focus management because page content changes without a full page reload.

**Focus on route change:**

```javascript
function navigateToRoute(route) {
    // Update content
    mainContent.innerHTML = renderRoute(route);

    // Move focus to the new content heading
    const heading = mainContent.querySelector('h1');
    if (heading) {
        heading.tabIndex = -1;
        heading.focus();
        // Screen reader announces the heading as a landmark
    }
}
```

**Announce page changes.** Use a live region to announce route changes:

```html
<div aria-live="polite" id="page-announcer" class="visually-hidden"></div>
```

```javascript
function announceRouteChange(routeName) {
    const announcer = document.getElementById('page-announcer');
    announcer.textContent = `Loaded ${routeName} page`;
}
```

**Return focus for closing actions.** When a modal closes or a menu collapses, return focus to the element that triggered it:

```javascript
let triggerElement;

function openMenu(button) {
    triggerElement = button;
    menu.hidden = false;
    menu.querySelector('a').focus();
}

function closeMenu() {
    menu.hidden = true;
    if (triggerElement) {
        triggerElement.focus();
    }
}
```

### Accessibility Requirements

**WCAG 2.2 success criteria for focus management:**

| Criterion | Level | Requirement |
|---|---|---|
| 2.1.1 Keyboard | A | All functionality operable through keyboard |
| 2.1.2 No Keyboard Trap | A | Focus cannot be trapped in a component; Escape closes or focus moves out |
| 2.4.1 Bypass Blocks | A | Skip link to bypass repeated content |
| 2.4.3 Focus Order | A | Focus order follows a logical sequence |
| 2.4.7 Focus Visible | AA | Keyboard focus indicator is visible |
| 2.4.11 Focus Not Obscured | AA | Focused element is not hidden by other content |
| 2.4.12 Focus Not Obscured (Enhanced) | AAA | Focused element is not partially hidden |
| 2.4.13 Focus Appearance | AAA | Focus indicator thickness: at least 2px perimeter, 3:1 contrast |

## Study Cases

### Accessible Modal Dialog

A modal dialog requires focus trapping, focus return, and keyboard dismissal:

```html
<button id="open-modal" aria-haspopup="dialog">Open Settings</button>

<div id="settings-modal" role="dialog" aria-modal="true"
     aria-labelledby="modal-title" tabindex="-1" hidden>
    <div class="modal-backdrop" onclick="closeModal()"></div>
    <div class="modal-content" role="document">
        <h2 id="modal-title">Settings</h2>
        <label>Theme: <select>...</select></label>
        <button onclick="closeModal()">Close</button>
    </div>
</div>
```

```javascript
const openBtn = document.getElementById('open-modal');
const modal = document.getElementById('settings-modal');
const closeBtn = modal.querySelector('button');
let previousFocus;

function openModal() {
    previousFocus = document.activeElement;
    modal.hidden = false;
    modal.focus();
    trapFocus(modal);
}

function closeModal() {
    modal.hidden = true;
    if (previousFocus) previousFocus.focus();
}

modal.addEventListener('keydown', (e) => {
    if (e.key === 'Escape') closeModal();
});
```

### Accessible Mega Menu

A mega menu with keyboard navigation uses a roving tabindex pattern:

```html
<nav aria-label="Main">
    <ul class="menu-bar">
        <li>
            <button aria-expanded="false" aria-controls="products-menu">Products</button>
            <div id="products-menu" class="mega-menu" hidden>
                <div class="menu-column">
                    <h3>Web</h3>
                    <a href="/html">HTML</a>
                    <a href="/css">CSS</a>
                </div>
                <div class="menu-column">
                    <h3>Mobile</h3>
                    <a href="/ios">iOS</a>
                    <a href="/android">Android</a>
                </div>
            </div>
        </li>
        <li>
            <button aria-expanded="false" aria-controls="docs-menu">Docs</button>
            <div id="docs-menu" class="mega-menu" hidden>...</div>
        </li>
    </ul>
</nav>
```

Keyboard behavior:
- Tab moves between top-level menu items
- Enter/Space opens the mega menu and moves focus to first link
- Arrow keys navigate within the open menu
- Escape closes the menu and returns focus to the top-level button

## Examples

### Example 1: Auto-focus on form error

```html
<form onsubmit="return validate(event)">
    <label for="email">Email</label>
    <input type="email" id="email" aria-describedby="email-error">
    <span id="email-error" role="alert" tabindex="-1"></span>
    <button type="submit">Submit</button>
</form>
```

```javascript
function validate(event) {
    const email = document.getElementById('email');
    const error = document.getElementById('email-error');

    if (!email.validity.valid) {
        event.preventDefault();
        error.textContent = 'Please enter a valid email address.';
        email.focus();
        return false;
    }
    return true;
}
```

### Example 2: Keyboard-navigable data grid

```html
<div role="grid" aria-label="Products table">
    <div role="row">
        <div role="columnheader" tabindex="0">Name</div>
        <div role="columnheader">Price</div>
    </div>
    <div role="rowgroup">
        <div role="row">
            <div role="gridcell" tabindex="-1">Widget</div>
            <div role="gridcell" tabindex="-1">$10</div>
        </div>
    </div>
</div>
```

Arrow keys navigate between cells. Tab enters and exits the grid.

### Example 3: Custom radio group

```html
<div role="radiogroup" aria-labelledby="size-label">
    <span id="size-label" class="label">Size</span>
    <label>
        <input type="radio" name="size" value="s" checked> Small
    </label>
    <label>
        <input type="radio" name="size" value="m"> Medium
    </label>
    <label>
        <input type="radio" name="size" value="l"> Large
    </label>
</div>
```

Native radio groups already support Arrow key navigation. Do not reinvent this with custom elements unless absolutely necessary.

## Glossary

| Term | Definition |
|---|---|
| Focus | The element that currently receives keyboard events |
| Focus ring | Visual indicator (usually an outline) showing the focused element |
| Focus trap | Pattern that prevents focus from leaving a container |
| Tab order | The sequence in which elements receive focus when pressing Tab |
| Tabindex | HTML attribute controlling keyboard focusability and order |
| Roving tabindex | Pattern where only one child in a set has tabindex="0" at a time |
| Skip link | Hidden link to jump to main content, visible on focus |
| `:focus-visible` | CSS pseudo-class matching elements focused via keyboard |
| `document.activeElement` | JavaScript property returning the focused element |
| WCAG | Web Content Accessibility Guidelines |

## Quick References

- [WCAG 2.2 Focus Order](https://www.w3.org/WAI/WCAG22/Understanding/focus-order.html) — understanding focus order requirements
- [MDN: :focus-visible](https://developer.mozilla.org/en-US/docs/Web/CSS/:focus-visible) — browser support and usage
- [WAI-ARIA Authoring Practices: Keyboard Navigation](https://www.w3.org/WAI/ARIA/apg/practices/keyboard-interface/) — keyboard patterns for widgets
- [Focus Management Polyfill](https://github.com/WICG/focus-visible) — `:focus-visible` polyfill

## Next Steps

- [Screen Reader Compatibility](screen-reader-compatibility.md) — writing HTML that works with all screen readers
- [ARIA Basics](aria-basics.md) — ARIA roles and states
- [Semantic HTML & Document Outline](semantic-html.md) — semantic HTML foundations