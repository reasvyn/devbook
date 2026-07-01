# Screen Reader Compatibility

## Description

Screen readers interpret HTML and the accessibility tree to produce speech or braille output. Writing HTML that works across different screen readers requires understanding how each engine parses semantics, ARIA, and dynamic content.

## Prerequisites

- [ARIA Basics](aria-basics.md) — ARIA roles, states, and properties
- [Focus Management](focus-management.md) — keyboard focus fundamentals
- [Semantic HTML & Document Outline](semantic-html.md) — semantic structure basics

## Table of Contents

- [How Screen Readers Work](#how-screen-readers-work)
- [Major Screen Readers](#major-screen-readers)
- [What Screen Readers Announce](#what-screen-readers-announce)
- [Screen Reader Mode Switching](#screen-reader-mode-switching)
- [Semantic HTML Compatibility](#semantic-html-compatibility)
- [ARIA Compatibility Across Readers](#aria-compatibility-across-readers)
- [Dynamic Content and Live Regions](#dynamic-content-and-live-regions)
- [Forms and Screen Readers](#forms-and-screen-readers)
- [Tables and Screen Readers](#tables-and-screen-readers)
- [Images and Screen Readers](#images-and-screen-readers)
- [CSS That Affects Screen Readers](#css-that-affects-screen-readers)
- [Testing Strategies](#testing-strategies)
- [Common Screen Reader Bugs](#common-screen-reader-bugs)
- [The Accessibility Tree](#the-accessibility-tree)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### How Screen Readers Work

Screen readers are software applications that interpret the browser's accessibility tree and convert it to speech (text-to-speech) or braille (refreshable braille display).

**The pipeline:**

```
HTML → DOM → Accessibility Tree → Platform API → Screen Reader → User
```

1. The browser parses HTML into the DOM
2. The browser computes the accessibility tree from the DOM (adding implicit roles, accessible names, states)
3. The accessibility tree is exposed through platform-specific accessibility APIs
4. The screen reader reads the accessibility tree and presents it to the user

**Screen readers do not read the visual layout.** They read the accessibility tree. This means:
- CSS styles (colors, fonts, margins) are generally ignored
- CSS `content` pseudo-elements may or may not be announced
- `display: none` removes elements from the accessibility tree
- `visibility: hidden` also removes elements
- `opacity: 0` does not remove elements (they remain focusable)

**Screen reader navigation modes:**

- **Browse mode** (default in most readers) — user moves through content by arrow keys, headings, links, landmarks
- **Focus mode** — user interacts with form controls and interactive widgets
- **Forms mode** — auto-activated on form fields for text input
- **Quick navigation** — single-key shortcuts to jump between elements (H for heading, T for table, L for list)

### Major Screen Readers

| Reader | Platform | Market share | Cost | Notes |
|---|---|---|---|---|
| NVDA | Windows | ~40% | Free | Open-source, very popular |
| JAWS | Windows | ~50% | $1,200+/year | Most common in enterprise |
| VoiceOver | macOS, iOS | Built-in | Free with macOS/iOS | Also on iPadOS |
| TalkBack | Android | Built-in | Free with Android | Screen reader for Android |
| Narrator | Windows | Built-in | Free with Windows | Improving rapidly |
| Orca | Linux | Built-in | Free with Linux/GNOME | GNOME desktop screen reader |
| ChromeVox | ChromeOS | Built-in | Free with ChromeOS | Primarily for Chromebooks |

**NVDA** (NonVisual Desktop Access) is the most popular free screen reader. Test with NVDA first if budget is limited.

**JAWS** (Job Access With Speech) has the most advanced features but is expensive. Many enterprise users rely on it.

**VoiceOver** is the only option on macOS/iOS. Its behavior differs from Windows readers in several ways.

### What Screen Readers Announce

When focusing an element, screen readers announce in order:

1. **Role** — what the element is (button, link, heading, image)
2. **Name** — the accessible name (from text content, `aria-label`, `alt`, or `aria-labelledby`)
3. **State** — current state (expanded, selected, disabled, checked)
4. **Description** — additional description (from `aria-describedby` or `title`)

**Example announcement for a button:**

```html
<button aria-pressed="false" aria-label="Mute">
    <span class="icon">🔊</span>
</button>
```

Screen reader: *"Mute, toggle button, not pressed"*

**Reading order of element attributes:**

| Priority | Attribute | Source |
|---|---|---|
| 1 | Role | Native HTML element or ARIA role |
| 2 | Name | `aria-labelledby` > `aria-label` > text content > `alt` > `title` |
| 3 | State | `aria-expanded`, `aria-selected`, `aria-pressed`, `aria-checked`, `disabled` |
| 4 | Description | `aria-describedby` > `title` (when not used as name) |

**Heading announcements:**

```html
<h1>Getting Started Guide</h1>
```

Screen reader: *"Heading level 1, Getting Started Guide"*

**Link announcements:**

```html
<a href="/docs">Read the documentation</a>
```

Screen reader: *"Read the documentation, link"*

### Screen Reader Mode Switching

Screen readers operate in different modes depending on the type of content.

**Browse mode** (also called scan mode, virtual cursor mode):

- The entire page is navigable with Arrow keys
- Single-key shortcuts work: H (heading), T (table), L (list), G (graphic)
- Clicking a link navigates to the URL
- Default mode for most content pages

**Focus mode** (also called forms mode, application mode):

- Arrow keys do not navigate — they control the widget
- Single-key shortcuts are disabled
- Activated automatically on form elements (`<input>`, `<select>`, `<textarea>`)
- Also activated by `role="application"`

**Why mode switching matters for ARIA:**

```html
<!-- Bad: custom widget in browse mode — arrow keys do not work -->
<div role="slider" tabindex="0">...</div>

<!-- Add role="application" wrapper if needed -->
<div role="application">
    <div role="slider" tabindex="0">...</div>
</div>
```

When a user enters a widget with `role="application"`, the screen reader switches to focus mode, allowing custom keyboard handling to work.

**When `role="application"` is appropriate:**

- Custom interactive widgets with complex keyboard navigation (like a spreadsheet)
- Drawing applications, game canvases
- Interactive data visualizations

**When `role="application"` is NOT appropriate:**

- Standard form controls (they auto-switch)
- Tab panels with roving tabindex (browse mode handles arrow keys fine)
- Navigation menus (landmark roles handle them)

### Semantic HTML Compatibility

Native HTML elements have the highest screen reader compatibility across all platforms.

**Form controls — always prefer native:**

```html
<!-- Works everywhere -->
<button>Submit</button>
<input type="text" placeholder="Name">
<select><option>A</option></select>
```

**Landmarks — use semantic HTML:**

```html
<!-- Native: works in all readers -->
<header>, <nav>, <main>, <aside>, <footer>

<!-- ARIA equivalent: works but redundant -->
<div role="banner">, <div role="navigation">, etc.
```

**Headings — hierarchy matters:**

```html
<!-- Correct hierarchy -->
<h1>Page Title</h1>
<h2>Section</h2>
<h3>Subsection</h3>

<!-- Wrong hierarchy — screen readers still announce but users may be confused -->
<h1>Title</h1>
<h3>Subsection</h3>  <!-- skipped level -->
```

Screen readers offer heading navigation (H key). Users navigate by heading level. A skipped level makes the outline confusing, but does not break functionality.

**Lists — native vs ARIA:**

```html
<!-- Native: screen reader announces "list with 3 items" -->
<ul>
    <li>A</li>
    <li>B</li>
    <li>C</li>
</ul>

<!-- ARIA: less reliable list announcements -->
<div role="list">
    <div role="listitem">A</div>
</div>
```

Some screen readers do not announce `role="list"` on a `<div>`. Always prefer `<ul>` and `<ol>` for lists.

**Tables — use native elements:**

```html
<!-- Screen reader announces table structure and navigation -->
<table>
    <caption>Monthly Sales</caption>
    <thead>
        <tr>
            <th scope="col">Month</th>
            <th scope="col">Revenue</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <th scope="row">January</th>
            <td>$10,000</td>
        </tr>
    </tbody>
</table>
```

ARIA table roles (`role="table"`, `role="row"`, `role="gridcell"`) are less reliable than native `<table>`.

### ARIA Compatibility Across Readers

ARIA support varies across screen readers and browser combinations.

**Well-supported ARIA (works in all modern readers):**

| Attribute | Notes |
|---|---|
| `aria-label` | Works reliably on interactive elements; less reliable on static elements in some readers |
| `aria-labelledby` | Good support; works across readers |
| `aria-describedby` | Good support; JAWS may require extra configuration |
| `aria-expanded` | Announced by all major readers |
| `aria-selected` | Announced in tab and grid patterns |
| `aria-pressed` | Toggle buttons work across readers |
| `aria-checked` | Checkboxes and switches work reliably |
| `aria-hidden` | Supported by all readers |
| `aria-current` | Works on links and list items |
| `aria-required` | Announced on form fields |
| `aria-invalid` | Announced on form fields |
| `aria-live` | `polite` and `assertive` supported |

**Partially supported ARIA (varies between readers):**

| Attribute | Issue |
|---|---|
| `aria-errormessage` | Not supported in VoiceOver; limited in JAWS |
| `role="alert"` | Works but NVDA and JAWS behave differently (NVDA announces immediately, JAWS may queue) |
| `aria-details` | Not widely supported |
| `aria-keyshortcuts` | VoiceOver does not announce keyboard shortcuts |
| `aria-roledescription` | Not supported in all readers |
| `aria-busy` | Inconsistent across readers |
| `aria-posinset` / `aria-setsize` | Works in most but with varying reliability |

**ARIA that needs testing:**

- `role="dialog"` vs `role="alertdialog"` — some readers treat them identically
- `role="status"` — similar to `aria-live="polite"` but with subtle differences
- `role="menubar"` — complex keyboard patterns; test with all readers
- `aria-activedescendant` — supported but behavior differs between readers

### Dynamic Content and Live Regions

**`aria-live` behavior differences:**

| Reader | `polite` | `assertive` |
|---|---|---|
| NVDA | Announces after current speech | Interrupts immediately |
| JAWS | May queue or suppress duplicates | Interrupts but may deduplicate |
| VoiceOver | Announce after current speech | Interrupts; may not announce if speaking fast |
| TalkBack | Announces after current speech | Interrupts |

**Best practices for live regions:**

```html
<!-- Always set initial content before making live -->
<div aria-live="polite" id="notifications"></div>
```

```javascript
// Set content only after element is in DOM
const live = document.getElementById('notifications');
live.textContent = 'New message received';
```

**Do not reuse the same live region for multiple updates without delay.** Screen readers may concatenate or skip rapid updates:

```javascript
// Bad: rapid updates may be skipped
for (const msg of messages) {
    liveRegion.textContent = msg;
}

// Good: use a timeout or batch updates
messages.forEach((msg, i) => {
    setTimeout(() => { liveRegion.textContent = msg; }, i * 1000);
});
```

**Announcing dynamic content when not in a live region:**

```html
<!-- Bad: screen reader will not announce this -->
<div id="new-content">
    <!-- Content added dynamically -->
</div>

<!-- Good: use a live region to announce -->
<div aria-live="polite" id="announcer"></div>
```

### Forms and Screen Readers

**Labeling form controls:**

```html
<!-- Method 1: explicit label (most reliable) -->
<label for="email">Email</label>
<input type="email" id="email">

<!-- Method 2: implicit label -->
<label>Email <input type="email"></label>

<!-- Method 3: aria-label (no visible label) -->
<input type="email" aria-label="Email">

<!-- Method 4: aria-labelledby -->
<h2 id="section-label">Contact Information</h2>
<input type="email" aria-labelledby="section-label email-hint">
<span id="email-hint">Email</span>
```

**Screen reader behavior with required fields:**

```html
<input type="text" required aria-required="true" aria-describedby="hint">
<span id="hint">Must be at least 3 characters.</span>
```

Announcement: *"Required, text, Must be at least 3 characters."*

VoiceOver uses the `required` attribute natively. JAWS and NVDA use both `required` and `aria-required`.

**Error handling:**

```html
<label for="username">Username</label>
<input type="text" id="username" aria-invalid="true"
       aria-describedby="username-error">
<span id="username-error" role="alert">Username is already taken.</span>
```

Best approach: associate the error with `aria-describedby` and wrap it in an element with `role="alert"` for immediate announcement.

**Screen reader form navigation tips:**

- Tab moves between form controls
- Screen readers announce label, type, required state, and current value
- Arrows in browse mode do not interact with form controls
- Auto-activation of forms mode depends on the screen reader

### Tables and Screen Readers

**Table navigation commands:**

| Key | Function |
|---|---|
| T | Jump to next table |
| Ctrl+Alt+Arrow | Navigate cells (JAWS and NVDA) |
| Ctrl+Cmd+Arrow | Navigate cells (VoiceOver) |

**Requirements for accessible tables:**

| Feature | Why | Example |
|---|---|---|
| `<caption>` | Provides a name for the table | `<caption>Q1 Sales</caption>` |
| `<th>` with `scope` | Identifies row/column headers | `<th scope="col">Month</th>` |
| `<thead>` / `<tbody>` | Structural grouping | Separates headers from data |
| `aria-describedby` | Additional table description | `<table aria-describedby="table-desc">` |

**Complex tables with merged cells:**

```html
<table>
    <caption>Employee Schedule</caption>
    <tr>
        <th rowspan="2">Employee</th>
        <th colspan="5">Weekdays</th>
    </tr>
    <tr>
        <th>Mon</th><th>Tue</th><th>Wed</th><th>Thu</th><th>Fri</th>
    </tr>
    <tr>
        <th scope="row">Alice</th>
        <td>Office</td><td>Remote</td><td>Office</td><td>Office</td><td>Remote</td>
    </tr>
</table>
```

`rowspan` and `colspan` are handled by most screen readers, but complex layouts can confuse cell navigation. Keep tables as simple as possible.

### Images and Screen Readers

**Screen reader behavior for images:**

```html
<!-- Announcement: "Cat sleeping on keyboard, image" -->
<img src="cat.jpg" alt="Cat sleeping on keyboard">

<!-- Decorative: ignored entirely -->
<img src="divider.png" alt="" role="presentation">

<!-- With title attribute adds extra info -->
<img src="chart.png" alt="Q1 revenue chart shows 20% growth"
     title="Generated January 2026">
```

**Image types and their announcements:**

| Image type | Announcement |
|---|---|
| `<img alt="description">` | "Description, image" |
| `<img alt="">` | Ignored entirely (decorative) |
| `<img>` (no alt) | File name or URL, "image" (bad) |
| CSS background image | Not announced |
| `<svg>` with ARIA | Depends on `role` and `aria-label` |
| `<figure>` with `<figcaption>` | "Figure, caption text, end figure" |

**SVG accessibility:**

```html
<!-- Accessible SVG -->
<svg role="img" aria-label="Search icon" width="24" height="24">
    <circle cx="12" cy="12" r="8"/>
</svg>
```

### CSS That Affects Screen Readers

**Properties that affect the accessibility tree:**

| CSS | Accessibility tree effect |
|---|---|
| `display: none` | Element removed entirely |
| `visibility: hidden` | Element removed entirely |
| `content` (pseudo) | May or may not be announced (varies by reader) |
| `opacity: 0; height: 0` | May still be usable by screen readers |
| `position: absolute; left: -9999px` | Element in tree but visually hidden |

**Visually hidden (screen reader only) text:**

```css
.visually-hidden {
    position: absolute;
    width: 1px;
    height: 1px;
    padding: 0;
    margin: -1px;
    overflow: hidden;
    clip: rect(0, 0, 0, 0);
    white-space: nowrap;
    border: 0;
}
```

This keeps content in the accessibility tree but invisible on screen. Use for:
- Skip links (hidden until focused)
- Additional description for icons
- Table captions that should not display visually
- Form instruction text for screen readers only

**CSS generated content (`::before`, `::after`):**

```css
.icon::before {
    content: "Warning: ";
}
```

Support for CSS content in screen readers varies:
- VoiceOver: announces CSS content
- NVDA: does not announce CSS content by default
- JAWS: configurable, generally does not announce

Do not rely on CSS `content` for critical information.

### Testing Strategies

**Test with at least three combinations:**

1. NVDA + Firefox (best free combination)
2. VoiceOver + Safari (macOS)
3. JAWS + Chrome (enterprise combination)

**What to test:**

| Test | What to check |
|---|---|
| Tab navigation | All interactive elements reachable in logical order |
| Arrow key navigation | Browse mode works on static content |
| Heading navigation | All content sections discoverable with H key |
| Landmark navigation | All page regions discoverable with D key (NVDA) |
| Form input | Labels announced, required state, error messages |
| Dynamic content | Live regions announce changes |

**Automated testing that augments manual testing:**

```javascript
// Check for accessible names
function checkAccessibleNames() {
    const interactive = document.querySelectorAll('button, a, input, select, textarea');
    interactive.forEach(el => {
        const name = el.getAttribute('aria-label')
            || el.getAttribute('aria-labelledby')
            || el.textContent?.trim();
        if (!name) {
            console.warn('No accessible name:', el);
        }
    });
}
```

### Common Screen Reader Bugs

**Empty `alt` on images with `title`:**

```html
<!-- Bug: screen reader reads title instead of ignoring the image -->
<img src="icon.png" alt="" title="Icon">
```

Fix: remove `title` when `alt` is empty.

**`aria-label` on `<div>` with `contenteditable`:**

```html
<!-- NVDA may not announce aria-label on contenteditable -->
<div contenteditable aria-label="Editor"></div>
```

Fix: use `<textarea>` or add `role="textbox"`.

**`aria-describedby` pointing to a hidden element:**

```html
<!-- May not be announced if target is hidden -->
<input aria-describedby="hidden-desc">
<span id="hidden-desc" style="display: none">Description</span>
```

Fix: use visually hidden class instead of `display: none`.

**Missing `role="alert"` on dynamically updated errors:**

```html
<!-- Bug: NVDA and JAWS may miss the update -->
<span id="error" aria-live="polite">Error message</span>

<!-- Fix: use role="alert" -->
<span id="error" role="alert">Error message</span>
```

**Focus management after AJAX update:**

```html
<!-- Bug: screen reader stays on old content after update -->
<div id="content">Old content</div>
<script>
    document.getElementById('content').innerHTML = 'New content';
</script>
```

Fix: move focus to the new content heading after update.

**`tabindex="0"` on non-interactive elements:**

```html
<!-- Bug: screen reader focuses but does not announce interaction -->
<h2 tabindex="0">Clickable heading</h2>
```

Fix: do not make non-interactive elements focusable unless they have a role.

### The Accessibility Tree

The accessibility tree is the browser's internal model of the page for assistive technologies.

**Viewing the accessibility tree:**

Chrome DevTools → Elements → Accessibility tab → Accessibility Tree

**What the tree contains:**

```html
<!-- HTML -->
<button aria-label="Close" class="btn-close">×</button>
```

Accessibility tree node:
```
role: button
name: "Close"
states: [focusable]
```

**Differences between DOM and accessibility tree:**

| State | DOM | Accessibility tree |
|---|---|---|
| `hidden` attribute | Present | Removed |
| `aria-hidden="true"` | Present | Removed |
| `display: none` | Present | Removed |
| `visibility: hidden` | Present | Removed |
| `opacity: 0` | Present | Present (still focusable) |

**How the tree is computed:**

1. Start with the DOM
2. Remove elements with `aria-hidden="true"` or `hidden`
3. Compute implicit roles from HTML elements (e.g., `<a href>` → `link`)
4. Apply explicit ARIA roles
5. Compute accessible names from `aria-labelledby`, `aria-label`, or text content
6. Compute accessible descriptions from `aria-describedby` or `title`
7. Compute states from attributes (`disabled`, `checked`, `aria-expanded`)

## Glossary

| Term | Definition |
|---|---|
| Screen reader | Software that renders the accessibility tree as speech or braille |
| Browse mode | Screen reader mode for reading content with arrow keys and shortcuts |
| Focus mode | Screen reader mode for interacting with form controls and widgets |
| Accessibility tree | Browser's internal model of the page for assistive technologies |
| VoiceOver | macOS and iOS built-in screen reader |
| NVDA | Free and open-source Windows screen reader |
| JAWS | Commercial Windows screen reader |
| TalkBack | Android built-in screen reader |
| Live region | ARIA mechanism for announcing dynamic content changes |
| Visually hidden | CSS pattern that hides content visually but keeps it in the accessibility tree |
| Platform API | Operating system accessibility API (UIA, IAccessible2, AX API) |

## Quick References

- [WebAIM Screen Reader Survey](https://webaim.org/projects/screenreadersurvey9/) — annual survey of screen reader usage statistics
- [NVDA Download](https://www.nvaccess.org/download/) — free screen reader for Windows
- [VoiceOver Getting Started](https://help.apple.com/voiceover/info/guide/) — macOS screen reader guide
- [Accessibility Tree Viewer](https://developer.chrome.com/docs/devtools/accessibility/reference/) — Chrome DevTools accessibility panel
- [ARIA Authoring Practices](https://www.w3.org/WAI/ARIA/apg/) — design patterns and examples

## Next Steps

- [Form Design Patterns](form-design-patterns.md) — accessible form layouts
- [Semantic HTML & Document Outline](semantic-html.md) — semantic structure fundamentals
- [ARIA Basics](aria-basics.md) — ARIA roles and states