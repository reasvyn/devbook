# HTML & ARIA

## Description

Accessible Rich Internet Applications (ARIA) extends HTML to make dynamic content and custom widgets accessible to screen readers and other assistive technologies. ARIA attributes bridge the gap when semantic HTML alone is not enough.

## Prerequisites

- [Semantic HTML & Document Outline](semantic-html.md) — semantic HTML foundations
- [Elements, Tags & Attributes](elements-and-attributes.md) — HTML basics

## Table of Contents

- [What is ARIA?](#what-is-aria)
- [The First Rule of ARIA](#the-first-rule-of-aria)
- [ARIA Roles](#aria-roles)
- [ARIA States and Properties](#aria-states-and-properties)
- [Live Regions](#live-regions)
- [Keyboard Focus and ARIA](#keyboard-focus-and-aria)
- [ARIA in Forms](#aria-in-forms)
- [ARIA in Navigation](#aria-in-navigation)
- [ARIA in Dynamic Content](#aria-in-dynamic-content)
- [Testing ARIA](#testing-aria)
- [Common Mistakes](#common-mistakes)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What is ARIA?

ARIA is a set of attributes that modify how an element is exposed to the accessibility tree. It does not change the element's appearance or behavior — only how assistive technologies perceive it.

**ARIA roles** define what an element is:

```html
<button role="tab" aria-selected="true">Tab 1</button>
```

**ARIA states and properties** define the element's current state:

```html
<button aria-pressed="false" aria-expanded="false">Toggle</button>
```

ARIA is interpreted by the browser's accessibility tree and exposed to assistive technologies through platform accessibility APIs (MSAA, IAccessible2, UIA, AX API).

### The First Rule of ARIA

**Do not use ARIA if you can use a native HTML element that provides the semantics and behavior you need.**

The first rule of ARIA:

```html
<!-- Bad: ARIA over native -->
<div role="button" tabindex="0" onclick="submit()">Submit</div>

<!-- Good: native HTML -->
<button type="submit">Submit</button>
```

Native HTML elements have built-in:
- Keyboard handling (Enter/Space to activate buttons)
- Focus management (tabindex, focus order)
- Accessibility semantics (role, state)
- Browser consistency across platforms

**When to use ARIA:**
- Custom widgets that do not have native HTML equivalents (tree views, sliders, tab panels)
- When native semantics are removed (e.g., `display: flex` on a `<ul>` does not change its semantics, but `role="list"` may be needed in some browsers)
- To supplement semantic HTML with additional states and descriptions
- Live regions for dynamic content updates

**When not to use ARIA:**
- When a native HTML element exists (use `<nav>` instead of `role="navigation"`)
- To fix invalid HTML (fix the HTML first)
- As a substitute for focus management
- To change how visual elements look

### ARIA Roles

ARIA roles define the type of widget or structure an element represents.

**Landmark roles** identify regions of a page:

```html
<nav aria-label="Main navigation" role="navigation">...</nav>
```

Semantic HTML elements provide landmark roles implicitly. Only add explicit roles if the element is not semantic:

```html
<div role="navigation" aria-label="Footer navigation">...</div>
```

| Landmark role | Semantic HTML equivalent |
|---|---|
| `banner` | `<header>` (page-level) |
| `navigation` | `<nav>` |
| `main` | `<main>` |
| `complementary` | `<aside>` |
| `contentinfo` | `<footer>` (page-level) |
| `form` | `<form>` |
| `search` | No native element (use `role="search"` on a search form) |

**Widget roles** define interactive controls:

```html
<button role="tab">Tab</button>
<div role="tabpanel">Panel content</div>
```

| Role | Purpose | Keyboard required |
|---|---|---|
| `button` | Clickable control | Yes (Enter/Space) |
| `link` | Navigational link | Yes (Enter) |
| `tab` | Tab in a tab list | Yes (Arrow keys) |
| `tabpanel` | Content panel for a tab | No |
| `checkbox` | Checkable input | Yes (Space) |
| `radio` | Radio button | Yes (Arrow keys) |
| `switch` | Toggle switch | Yes (Space) |
| `slider` | Range slider | Yes (Arrow keys) |
| `progressbar` | Progress indicator | No |
| `menuitem` | Item in a menu | Yes (Arrow keys) |

**Document structure roles** describe the structure of content:

```html
<article role="article">...</article>
```

| Role | Purpose |
|---|---|
| `article` | Self-contained content |
| `heading` | Heading (use `<h1>`-`<h6>` instead) |
| `img` | Image (use `<img>` instead) |
| `list` / `listitem` | Lists (use `<ul>`/`<ol>`/`<li>` instead) |
| `table` / `row` / `cell` | Tables (use `<table>`/`<tr>`/`<td>` instead) |
| `toolbar` | Toolbar container |
| `tooltip` | Tooltip popup |

**Abstract roles** are used to define role hierarchies in the ARIA spec. Never use abstract roles in HTML.

### ARIA States and Properties

**`aria-label`** — provides an accessible name when no visible label exists:

```html
<button aria-label="Close dialog">×</button>
<nav aria-label="Main navigation">...</nav>
<input type="search" aria-label="Search products">
```

**`aria-labelledby`** — references another element as the accessible name:

```html
<h2 id="section-title">Settings</h2>
<div role="region" aria-labelledby="section-title">
    <!-- Settings content -->
</div>
```

If both `aria-label` and `aria-labelledby` are present, `aria-labelledby` takes precedence.

**`aria-describedby`** — associates a description with an element:

```html
<label for="password">Password</label>
<input type="password" id="password" aria-describedby="password-hint">
<span id="password-hint">Must be at least 8 characters.</span>
```

Screen readers announce the description after the label and role.

**`aria-required`** — indicates a field is required:

```html
<input type="email" required aria-required="true">
```

The `required` attribute already conveys this to screen readers. Use `aria-required="true"` as a supplement for custom form controls.

**`aria-invalid`** — indicates a field has a validation error:

```html
<input type="email" aria-invalid="true" aria-describedby="email-error">
<span id="email-error" role="alert">Invalid email format.</span>
```

**`aria-expanded`** — indicates whether a collapsible element is open or closed:

```html
<button aria-expanded="false" aria-controls="menu">Menu</button>
<ul id="menu" hidden>
    <li><a href="/">Home</a></li>
</ul>
```

**`aria-controls`** — references the element this widget controls:

```html
<button aria-controls="panel" aria-expanded="false">Toggle</button>
<div id="panel">Content</div>
```

**`aria-hidden`** — hides an element from the accessibility tree:

```html
<span aria-hidden="true">*</span>  <!-- Decorative asterisk -->
<svg aria-hidden="true" focusable="false">...</svg>  <!-- Decorative icon -->
```

Do not use `aria-hidden="true"` on focusable elements. Its effect can be overridden by CSS `display: none` or `visibility: hidden`.

**`aria-current`** — indicates the current item in a set:

```html
<nav aria-label="Breadcrumb">
    <a href="/">Home</a> /
    <a href="/docs">Docs</a> /
    <a aria-current="page" href="/docs/html">HTML</a>
</nav>
```

| `aria-current` value | Meaning |
|---|---|
| `page` | Current page in a set of pages |
| `step` | Current step in a process |
| `location` | Current item in a flow |
| `date` | Current date in a calendar |
| `time` | Current time |
| `true` | Current item (generic) |

**`aria-selected`** — indicates the selected item in a list or tab:

```html
<div role="tablist">
    <button role="tab" aria-selected="true">Tab 1</button>
    <button role="tab" aria-selected="false">Tab 2</button>
</div>
```

**`aria-disabled`** — indicates an element is disabled but still focusable:

```html
<button aria-disabled="true">Save</button>
```

Unlike the HTML `disabled` attribute, `aria-disabled` does not prevent focus or keyboard events. It only communicates the disabled state to screen readers.

### Live Regions

Live regions announce content changes to screen readers without requiring focus:

**`aria-live`** — the politeness level of announcements:

| Value | Behavior |
|---|---|
| `off` | Do not announce changes (default) |
| `polite` | Announce when idle (for non-urgent updates) |
| `assertive` | Announce immediately (for urgent updates) |

```html
<div aria-live="polite" id="status-message">
    <!-- Dynamic content updates announced here -->
</div>
```

**`role="alert"`** — an assertive live region for error messages:

```html
<span id="error" role="alert">Email is required.</span>
```

Automatically behaves like `aria-live="assertive"` and announces the content immediately.

**`role="status"`** — a polite live region for status messages:

```html
<div role="status">3 items in cart.</div>
```

Automatically behaves like `aria-live="polite"`.

**`aria-atomic`** — whether to announce the entire live region or just the changed part:

```html
<div aria-live="polite" aria-atomic="true">
    <!-- Full content is announced on any change -->
</div>
```

**`aria-relevant`** — which types of changes trigger announcement:

```html
<div aria-live="polite" aria-relevant="additions removals">
    <!-- Announce both added and removed content -->
</div>
```

Default is `additions text`.

### Keyboard Focus and ARIA

**`tabindex`** — controls keyboard focusability:

- `tabindex="0"` — element is focusable in natural tab order
- `tabindex="-1"` — element is focusable only via JavaScript (not tab navigation)
- `tabindex="1"` or higher — element is focusable with custom tab order (avoid)

```html
<div role="button" tabindex="0" onclick="submit()">Submit</div>
```

**`aria-activedescendant`** — identifies the currently active child in a composite widget:

```html
<div role="combobox" aria-expanded="true" aria-activedescendant="option-2">
    <input type="text" aria-controls="listbox">
</div>
<ul role="listbox" id="listbox">
    <li role="option" id="option-1">Option 1</li>
    <li role="option" id="option-2" aria-selected="true">Option 2</li>
</ul>
```

Focus remains on the combobox, but screen readers announce option-2 as active.

**`aria-modal`** — indicates a modal dialog:

```html
<div role="dialog" aria-modal="true" aria-labelledby="dialog-title">
    <h2 id="dialog-title">Confirm Delete</h2>
    <p>Are you sure?</p>
    <button>Delete</button>
    <button>Cancel</button>
</div>
```

### ARIA in Forms

**`aria-required`** — supplement for custom controls:

```html
<div role="checkbox" tabindex="0" aria-checked="true" aria-required="true">
    I agree to the terms
</div>
```

**`aria-invalid`** — field error state:

```html
<input type="email" aria-invalid="true" aria-describedby="email-error">
```

**`aria-describedby`** — associate hints and errors:

```html
<label for="password">Password</label>
<input type="password" id="password"
       aria-describedby="password-hint password-error">
<span id="password-hint">8+ characters with uppercase and number.</span>
<span id="password-error" role="alert"></span>
```

**`aria-errormessage`** — a stronger association than `aria-describedby`:

```html
<input type="email" aria-invalid="true" aria-errormessage="email-error">
<span id="email-error" role="alert">Please enter a valid email address.</span>
```

`aria-errormessage` is preferred when the message is specifically an error (not just a hint). It replaces `aria-describedby` for error messages when both are present.

### ARIA in Navigation

**Breadcrumb navigation:**

```html
<nav aria-label="Breadcrumb">
    <ol>
        <li><a href="/">Home</a></li>
        <li><a href="/docs">Documentation</a></li>
        <li aria-current="page">HTML Guide</li>
    </ol>
</nav>
```

**Pagination:**

```html
<nav aria-label="Pagination">
    <ul>
        <li><a href="/page/1" aria-label="Page 1" aria-current="page">1</a></li>
        <li><a href="/page/2" aria-label="Page 2">2</a></li>
        <li><a href="/page/3" aria-label="Page 3">3</a></li>
    </ul>
</nav>
```

**Skip link:**

```html
<a href="#main-content" class="skip-link">Skip to main content</a>
<main id="main-content">
    <!-- Primary content -->
</main>
```

### ARIA in Dynamic Content

**Tab panel pattern:**

```html
<div role="tablist" aria-label="Documentation sections">
    <button role="tab" aria-selected="true" aria-controls="panel-html"
            id="tab-html">HTML</button>
    <button role="tab" aria-selected="false" aria-controls="panel-css"
            id="tab-css">CSS</button>
</div>

<div role="tabpanel" id="panel-html" aria-labelledby="tab-html">
    <h2>HTML Content</h2>
</div>
<div role="tabpanel" id="panel-css" aria-labelledby="tab-css" hidden>
    <h2>CSS Content</h2>
</div>
```

Keyboard handling required: Arrow keys to switch tabs, Home/End for first/last.

**Accordion pattern:**

```html
<button aria-expanded="false" aria-controls="section-1">
    Section 1
</button>
<div id="section-1" hidden>
    <p>Hidden content revealed on toggle.</p>
</div>
```

**Modal dialog pattern:**

```html
<button aria-haspopup="dialog" onclick="openDialog()">Open Dialog</button>

<div role="dialog" aria-modal="true" aria-labelledby="dialog-title"
     id="dialog" hidden>
    <h2 id="dialog-title">Confirm</h2>
    <p>Are you sure you want to proceed?</p>
    <button onclick="confirm()">Yes</button>
    <button onclick="closeDialog()" autofocus>No</button>
</div>
```

Requirements: Focus must be trapped inside the modal, the close button should return focus to the trigger, pressing Escape closes the dialog.

### Testing ARIA

**Manual testing with screen readers:**

- **VoiceOver** (macOS/iOS): built-in, activate with Cmd+F5
- **NVDA** (Windows): free, open-source
- **JAWS** (Windows): commercial, most common

Check:
1. Are all elements announced with correct roles?
2. Are dynamic updates announced via live regions?
3. Can all interactive elements be reached and operated with keyboard?
4. Are states (expanded, selected, disabled) announced?

**Automated testing tools:**

- **axe DevTools** — browser extension for accessibility audit
- **Lighthouse** — built into Chrome DevTools
- **WAVE** — browser extension for visual accessibility reporting
- **Accessibility Insights** — Microsoft's accessibility testing tool

**Common ARIA testing tools:**

```javascript
// Check if an element has an accessible name
const button = document.querySelector('button');
console.log(button.getAttribute('aria-label'));
console.log(button.getAttribute('aria-labelledby'));
console.log(button.textContent);
```

### Common Mistakes

**Redundant ARIA.** Using ARIA roles that duplicate native semantics:

```html
<!-- Bad -->
<nav role="navigation">...</nav>
<button role="button">...</button>

<!-- Good -->
<nav>...</nav>
<button>...</button>
```

**Missing ARIA on custom widgets.** Building a custom widget without ARIA:

```html
<div class="slider" onclick="changeValue(event)">...</div>
<!-- Screen readers see a generic div -->
```

The fix: add `role="slider"`, `aria-valuenow`, `aria-valuemin`, `aria-valuemax`, and keyboard handling.

**Incorrect use of `aria-label`.** Adding `aria-label` to an element that already has visible text:

```html
<!-- Bad: aria-label replaces visible text for screen readers -->
<button aria-label="Close">X</button>
<!-- Screen reader hears "Close" — correct -->

<!-- Bad: aria-label duplicates visible text -->
<button aria-label="Submit">Submit</button>
<!-- Screen reader hears "Submit Submit" — redundant -->
```

**Using `aria-hidden="true"` on focusable elements.** A focusable element with `aria-hidden="true"` confuses screen readers:

```html
<!-- Bad -->
<button aria-hidden="true" tabindex="0">Hidden button</button>

<!-- Correct: hide the element visually and from accessibility -->
<button hidden>Hidden button</button>
```

**Not testing with actual screen readers.** Automated tools catch about 30% of accessibility issues. Manual testing is essential.

**Overusing `role="alert"`.** Every time the content of an `alert` role changes, it interrupts the screen reader. Use it only for urgent messages.

**Forgetting keyboard handling on custom widgets.** A `role="button"` without keyboard event handlers is not keyboard accessible:

```html
<!-- Bad: no keyboard handling -->
<div role="button" onclick="submit()">Submit</div>

<!-- Good: keyboard and click handling -->
<div role="button" tabindex="0" onclick="submit()"
     onkeydown="if(event.key==='Enter'||event.key===' ') submit()">
    Submit
</div>
```

**`aria-describedby` on hidden elements.** Referencing a hidden element via `aria-describedby` may not be announced:

```html
<!-- May not be announced -->
<input aria-describedby="hint" type="text">
<span id="hint" hidden>Enter your full name.</span>
```

Solution: use CSS to visually hide but keep in accessibility tree, or do not hide the element.

## Study Cases

### Custom Select Menu

A custom select menu requires ARIA to match the semantics of a native `<select>`:

```html
<label id="sort-label">Sort by</label>
<div role="combobox" aria-expanded="false" aria-labelledby="sort-label"
     aria-haspopup="listbox" tabindex="0" id="sort-btn">
    <span id="sort-value">Name</span>
</div>
<ul role="listbox" aria-labelledby="sort-label" id="sort-menu" hidden>
    <li role="option" aria-selected="true" id="opt-name">Name</li>
    <li role="option" aria-selected="false" id="opt-date">Date</li>
    <li role="option" aria-selected="false" id="opt-size">Size</li>
</ul>
```

Key behaviors to implement:
- `aria-expanded` toggles between `true`/`false`
- Arrow keys navigate options and update `aria-activedescendant`
- Enter/Space selects the option and updates `aria-selected`
- `aria-selected` moves from old to new option
- `aria-activedescendant` on the combobox tracks the focused option

### Dynamic Notification System

A toast notification system uses live regions:

```html
<div id="toast-container" aria-live="polite" aria-atomic="true">
    <!-- Notifications appear here dynamically -->
</div>
```

```javascript
function showToast(message, type) {
    const container = document.getElementById('toast-container');
    const toast = document.createElement('div');
    toast.className = `toast toast-${type}`;
    toast.textContent = message;
    container.appendChild(toast);
    setTimeout(() => toast.remove(), 5000);
}
```

By using `aria-live="polite"`, notifications are announced after the current speech output finishes. For critical errors (like payment failures), use `role="alert"` on the specific toast.

## Examples

### Example 1: Progress indicator

```html
<div role="progressbar" aria-valuenow="60" aria-valuemin="0"
     aria-valuemax="100" aria-label="Upload progress">
    <div class="progress-bar" style="width: 60%"></div>
</div>
```

The `aria-valuenow` must be updated programmatically as progress changes.

### Example 2: Tooltip

```html
<button aria-describedby="tip-save">Save</button>
<div role="tooltip" id="tip-save" class="tooltip" hidden>
    Saves the current document (Ctrl+S)
</div>
```

### Example 3: Rating widget

```html
<div role="radiogroup" aria-label="Rating">
    <button role="radio" aria-checked="false" aria-label="1 star">☆</button>
    <button role="radio" aria-checked="false" aria-label="2 stars">☆</button>
    <button role="radio" aria-checked="false" aria-label="3 stars">☆</button>
    <button role="radio" aria-checked="false" aria-label="4 stars">☆</button>
    <button role="radio" aria-checked="false" aria-label="5 stars">☆</button>
</div>
```

## Glossary

| Term | Definition |
|---|---|
| ARIA | Accessible Rich Internet Applications — W3C specification for accessibility semantics |
| Accessibility tree | The subset of the DOM exposed to assistive technologies |
| Landmark role | ARIA role identifying a page region (navigation, main, complementary) |
| Widget role | ARIA role for interactive controls (button, slider, tab) |
| Live region | ARIA mechanism for announcing dynamic content changes |
| `aria-label` | Attribute providing an accessible name string |
| `aria-labelledby` | Attribute referencing another element as the accessible name |
| `aria-describedby` | Attribute referencing an element with a longer description |
| `aria-hidden` | Attribute removing an element from the accessibility tree |
| `aria-expanded` | Attribute indicating if a collapsible region is open |
| `aria-controls` | Attribute referencing the element controlled by a widget |
| `aria-current` | Attribute indicating the current item in a set |
| `tabindex` | HTML attribute controlling keyboard focusability |
| Screen reader | Software that interprets the accessibility tree and outputs speech or braille |
| Assistive technology | Any tool that helps people with disabilities use technology |

## Quick References

- [WAI-ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/) — design patterns and examples for accessible widgets
- [MDN: ARIA guide](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA) — comprehensive ARIA reference
- [HTML Validation: W3C Nu Checker](https://validator.w3.org/nu/) — checks for ARIA attribute misuse
- [axe DevTools](https://www.deque.com/axe/) — automated accessibility testing
- [WebAIM: ARIA Overview](https://webaim.org/intro/aria/) — practical introduction to ARIA

## Next Steps

- [Focus Management](focus-management.md) — managing keyboard focus and tab order
- [Screen Reader Compatibility](screen-reader-compatibility.md) — writing HTML that works with all screen readers
- [Semantic HTML & Document Outline](semantic-html.md) — semantic HTML foundations
