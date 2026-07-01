# Debugging with DevTools

## Description

Browser Developer Tools are the most powerful debugging toolkit for frontend development. Mastering the Elements, Console, Network, and Sources panels transforms debugging from guesswork into systematic investigation.

## Prerequisites

- [DOM Manipulation](dom-manipulation.md) — inspecting and modifying elements
- [Events](events.md) — debugging event listeners
- [Fetch API & HTTP](fetch-api.md) — inspecting network requests

## Table of Contents

- [Opening DevTools](#opening-devtools)
- [Elements Panel](#elements-panel)
- [Console Panel](#console-panel)
- [Console API](#console-api)
- [Network Panel](#network-panel)
- [Sources Panel](#sources-panel)
- [Breakpoints](#breakpoints)
- [Debugging Techniques](#debugging-techniques)
- [Performance Panel](#performance-panel)
- [Application Panel](#application-panel)
- [Memory & Profiling](#memory--profiling)
- [Mobile Debugging](#mobile-debugging)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Opening DevTools

| Shortcut | Action |
|----------|--------|
| `F12` or `Ctrl+Shift+I` | Open last panel |
| `Ctrl+Shift+J` | Open to Console |
| `Ctrl+Shift+C` | Inspect element (pick mode) |

Right-click any element and select **Inspect** to jump to the Elements panel with that element selected.

For keyboard accessibility, use `F12` on all major browsers.

**DevTools is a persistent panel** — it stays open across page navigations and tab switches.

### Elements Panel

The Elements panel shows the DOM tree and computed styles.

**Inspecting and editing the DOM:**

```html
<!-- Double-click any text node to edit in place -->
<ul>
    <li>Double-click me</li>
</ul>
```

**Actions:**

- **Edit as HTML** — right-click → "Edit as HTML" to rewrite the full element
- **Delete element** — press `Delete` key or right-click → "Delete element"
- **Copy** — right-click → "Copy" → Copy outerHTML, Copy JS Path, Copy selector
- **Drag to reorder** — click and drag elements in the tree
- **Scroll into view** — right-click → "Scroll into view"

**Styles panel:**

- **Filter** — type to filter CSS rules (e.g., `color`, `flex`)
- **Toggle pseudo-classes** — click `:hov` to force `:hover`, `:focus`, `:active`, `:visited`
- **Add new style** — click between curly braces in a rule
- **Computed tab** — see the final computed value for any property, including inherited values
- **Box model diagram** — visual representation of margin, border, padding, content

**Inspecting computed styles:**

```
Computed tab shows the final applied value for every CSS property.
Check "Show all" to see every property, or search for a specific one.
```

**Force state on element:**

1. Select the element in Elements
2. Click the `:hov` button in the Styles pane
3. Check the states you want to force (e.g., `:hover`)

This is invaluable for debugging hover menus, focus styles, and active states.

**Event listeners in Elements:**

Select an element, then go to the **Event Listeners** tab to see all registered listeners. You can:

- See which function is attached
- View the source location (click to jump to Sources)
- Check propagation phase (capture vs. bubble)
- Remove listeners on the fly

**Break on...:**

Right-click an element → "Break on..." → choose one of:

- **Subtree modifications** — pauses when child nodes are added/removed
- **Attribute modifications** — pauses when element attributes change
- **Node removal** — pauses when the element is removed

This is extremely useful for finding which code modifies the DOM.

### Console Panel

The Console has two purposes: logging and direct JavaScript execution.

**Evaluating expressions:**

Type any JavaScript expression and press Enter:

```javascript
document.title
document.querySelectorAll("button").length
fetch("/api/data").then(r => r.json())
```

**$ shortcuts (Chrome/Edge):**

```javascript
$("selector");             // document.querySelector()
$$("selector");            // document.querySelectorAll()
$0;                        // currently selected element in Elements
$1;                        // previously selected element
$_ ;                        // last evaluated result
```

**Live expressions:**

Click the eye icon (`👁`) in the Console to create a live expression — a value that updates in real-time:

```
document.querySelector(".notification-badge").textContent
```

This shows the current value at all times, refreshing automatically.

**Console history:**

- `↑` / `↓` — cycle through previous commands
- `Ctrl+L` — clear console output
- `console.clear()` — also clears from code

**Persist log:**

Check "Preserve log" to keep messages across page reloads. Useful when debugging during page load or form submissions that navigate away.

**Filter levels:**

Click the log level icons to filter:

- `Info` — `console.log()`
- `Warnings` — `console.warn()`
- `Errors` — `console.error()`
- `Verbose` — `console.debug()`

### Console API

The Console API provides methods for structured logging.

**Basic logging:**

```javascript
console.log("Plain message");
console.info("Informational");
console.warn("Warning with yellow");
console.error("Error with red and stack trace");
console.debug("Verbose debug message");
```

**Structured output:**

```javascript
console.log("User:", { name: "Alice", age: 30 });
console.table(users, ["name", "email"]);      // tabular display

const users = [
    { name: "Alice", email: "alice@example.com" },
    { name: "Bob", email: "bob@example.com" }
];
console.table(users);
// ┌─────────┬─────────┬──────────────────────┐
// │ (index) │  name   │        email         │
// ├─────────┼─────────┼──────────────────────┤
// │    0    │ 'Alice' │ 'alice@example.com'  │
// │    1    │  'Bob'  │ 'bob@example.com'    │
// └─────────┴─────────┴──────────────────────┘
```

**Grouping:**

```javascript
console.group("User Data");
console.log("Name:", user.name);
console.log("Email:", user.email);
console.group("Address");
console.log("City:", user.address.city);
console.log("Country:", user.address.country);
console.groupEnd();
console.groupEnd();
```

**Timing:**

```javascript
console.time("fetch-users");
await fetch("/api/users");
console.timeEnd("fetch-users");              // "fetch-users: 142ms"

console.timeLog("fetch-users");              // intermediate timing
```

**Counting:**

```javascript
function processButton(button) {
    console.count("button clicks");
    // button clicks: 1
    // button clicks: 2
    // ...

    console.count(`Button: ${button.id}`);
}
```

**Tracing:**

```javascript
function deeplyNested() {
    console.trace("Where was this called from?");
    // Shows call stack in the console
}
```

**Assertions:**

```javascript
function divide(a, b) {
    console.assert(b !== 0, "Division by zero");
    return a / b;
}

divide(10, 0);                               // Assertion failed: Division by zero
```

**Conditional logging with style:**

```javascript
console.log(
    "%cSuccess: %cData loaded",
    "color: green; font-weight: bold",
    "color: normal"
);
```

**CSS-style formatting specifiers:**

```javascript
console.log("%cLarge red text", "font-size: 24px; color: red;");
console.log("%cBackground%cText",
    "background: yellow; padding: 2px 4px;",
    "color: blue;"
);
```

### Network Panel

The Network panel records all HTTP requests made by the page.

**Common columns:**

- **Name** — resource URL
- **Status** — HTTP status code
- **Type** — document, script, fetch, XHR, stylesheet, etc.
- **Initiator** — what caused the request (parser, script, etc.)
- **Size** — transferred vs. actual size
- **Time** — total duration
- **Waterfall** — timeline showing request phases

**Filtering requests:**

- Click `Fetch/XHR` to see only API calls
- Click `JS` to see only scripts
- Click `Img` to see only images
- Type in the filter box to search by URL

**Request details — click a request:**

| Tab | Purpose |
|-----|---------|
| Headers | Request/response headers, status, query params |
| Payload | POST/PUT request body data |
| Preview | Parsed response (JSON rendered, images shown) |
| Response | Raw response body text |
| Initiator | Call stack that triggered the request |
| Timing | DNS, TCP, TLS, Request, Response latency breakdown |
| Cookies | Cookie data sent and received |

**Network throttling:**

Simulate slow connections:

1. Open the Network panel
2. Select throttling preset: "Slow 3G", "Fast 3G", "Offline"
3. Or add custom profiles (e.g., 100 kbps, 500ms latency)

**Blocking requests:**

Right-click a request → "Block request URL" to simulate missing resources or test fallback behavior.

**Replay requests:**

Right-click a request → "Replay XHR" or "Replay fetch" to re-issue the exact same request.

**Export/Import HAR files:**

- **Export** — click the download icon to save all network data as HAR
- **Import** — drag a HAR file into the Network panel to analyze logs from another session

**Preserve log:**

Check "Preserve log" to keep network entries across page navigations.

**Capture screenshots:**

Check "Screenshots" to capture page renders at each network request. Useful for seeing what the page looked like at each stage of loading.

### Sources Panel

The Sources panel is a full debugger for JavaScript, CSS, and even DOM changes.

**Navigating files:**

The left pane shows all loaded files organized by origin. Use `Ctrl+P` to search for files by name.

**The debugger controls:**

```
▶ Continue     (F8)    — resume execution until next breakpoint
↻ Step Over    (F10)   — execute current line, step to next
⬇ Step Into    (F11)   — enter the current function call
⬆ Step Out     (Shift) — finish current function, return to caller
```

**Snippet execution:**

Create and run small JavaScript snippets from DevTools:

1. Open Sources → Snippets tab
2. Right-click → New
3. Write code and run with `Ctrl+Enter`

Snippets persist across page loads. Useful for utility functions (e.g., "Extract all links").

**Watch expressions:**

Add variables to the Watch pane to monitor their values as you step through code:

```
user.profile.name
document.querySelector(".active")
```

**Scope panel:**

Shows all variables in the current scope: Local, Closure, Global. Values update in real-time as you step.

**Call stack:**

Shows the current execution stack. Each frame is clickable to jump to that location in the source. Useful for understanding how you arrived at a buggy line.

**XHR/fetch breakpoints:**

Sources → Event Listener Breakpoints → XHR → break on `readystatechange` or `load`

Or: Sources → XHR Breakpoints → click `+` to add URL patterns that trigger a breakpoint when fetched.

### Breakpoints

Breakpoints pause JavaScript execution at a specific line.

**Setting a breakpoint:**

Click the line number in the Sources panel. A blue arrow appears.

**Conditional breakpoints:**

Right-click a line number → "Add conditional breakpoint" → enter an expression:

```javascript
user.role === "admin"                        // only break for admins
count > 10                                   // only break after 10 iterations
```

The breakpoint triggers only when the expression evaluates to true.

**Logpoints:**

Right-click a line number → "Add logpoint" → enter a message:

```javascript
'Button clicked, text: ' + event.target.textContent
```

Logpoints print to the console without pausing execution. Useful for debugging code you cannot modify.

**DOM breakpoints:**

Set from the Elements panel:

1. Right-click an element
2. "Break on..." → Subtree modifications / Attribute modifications / Node removal

**Event listener breakpoints:**

Sources → Event Listener Breakpoints → expand a category (Mouse, Keyboard, etc.) → check the event.

Pauses on the first line of any matching event handler.

**Global event listener breakpoints:**

Sources → Event Listener Breakpoints → Script → "Script First Statement" — breaks immediately when any script runs.

### Debugging Techniques

**Blackboxing:**

Right-click a script in Sources → "Blackbox script". This hides the script from the call stack and prevents stepping into it. Useful for skipping library/framework code.

**Pretty-print:**

Click `{}` (Pretty Print) in Sources to format minified code into readable JavaScript with proper indentation.

**Overrides:**

Sources → Overrides tab → select a folder to save modified files. Changes persist across page loads. Useful for testing CSS/JS fixes on production sites.

**Local overrides workflow:**

1. Create a folder to store overrides
2. Open Sources → Overrides → select the folder
3. Edit any file in Sources — it saves to the override folder
4. Refresh the page — your overrides apply automatically

**Workspace mapping:**

Map local filesystem files to network resources. Edits in DevTools are saved directly to your local project files.

1. Sources → Filesystem → "Add folder to workspace"
2. Right-click a network file → "Map to filesystem resource..."
3. Select the local file

**Disable JavaScript:**

`Ctrl+Shift+P` → type "Disable JavaScript" → reload. Tests how the page behaves without JS (progressive enhancement testing).

**Emulate CSS features:**

`Ctrl+Shift+P` → "Emulate CSS prefers-color-scheme: dark" to test dark mode without changing OS settings.

### Performance Panel

Record and analyze runtime performance.

**Recording a profile:**

1. Open Performance panel
2. Click the record button (circle) or press `Ctrl+E`
3. Interact with the page
4. Click stop

**Reading the timeline:**

```
Top section: FPS, CPU, Network usage
┌──────────────────────────────────────────────┐
│ FPS  ████░░░░░░░░░░░░░░░░  ██████████          │
│ CPU  ██████████░░░░░░░░░░░░░░░░░░  ██████     │
│ NET  ░░░░░░░░░░░░░░░░░░░░░░  ████              │
├──────────────────────────────────────────────┤
│ Main thread flame chart                        │
│ ┌───┬────┬───┬────┬───┬────┬───┬────┬───┐     │
│ │   │    │   │    │   │    │   │    │   │     │
│ └───┴────┴───┴────┴───┴────┴───┴────┴───┘     │
└──────────────────────────────────────────────┘
```

**Interpreting frames:**

- **Green FPS bar** — smooth (60 fps)
- **Red FPS bar** — jank (frames dropped)
- **Flame chart** — shows function calls on the main thread
- **Summary** — categories: Scripting, Rendering, Painting, System, Idle, Loading

**Identifying layout thrashing:**

Look for "Layout" events (purple) followed immediately by script execution. The pattern "write → read → write → read" forces synchronous reflows.

**Long tasks:**

Tasks over 50ms block the main thread. The Performance panel highlights these in red. Click to see what caused the blockage.

### Application Panel

Inspect and manage client-side storage.

**Storage tabs:**

| Tab | Content |
|-----|---------|
| localStorage | Key-value pairs per origin |
| sessionStorage | Tab-scoped key-value pairs |
| IndexedDB | Browser database tables and data |
| Cookies | All cookies for the current origin |
| Cache Storage | Cache API entries (Service Workers) |
| Back/forward cache | Recently navigated pages in cache |

**Actions:**

- **Double-click to edit** any value in place
- **Delete** individual entries or clear all
- **Filter** by key or value

### Memory & Profiling

**Memory panel:**

Three profiling options:

| Profile | Use Case |
|---------|----------|
| Heap snapshot | Compare heap snapshots to find detached DOM nodes and memory leaks |
| Allocation instrumentation | Record allocations in real-time |
| Allocation sampling | Low-overhead allocation profiling |

**Common memory leak patterns:**

1. **Detached DOM nodes** — elements removed from the DOM but still referenced in JavaScript
2. **Event listener leaks** — listeners on removed elements that were never cleaned up
3. **Closure references** — closures holding references to entire scopes
4. **Timers** — `setInterval` not cleared

### Mobile Debugging

**Device emulation:**

Toggle Device Toolbar (`Ctrl+Shift+M`) to emulate:

- Device dimensions (iPhone, Pixel, iPad)
- Device pixel ratio (DPR)
- Touch events
- User agent string
- Network throttling

**Remote debugging (Android):**

1. Connect device via USB
2. Enable USB debugging in Developer Options
3. Chrome → `chrome://inspect`
4. Click "Inspect" on the device tab

**Safari remote debugging (iOS):**

1. Enable Web Inspector in Safari settings
2. Connect via USB
3. Safari on Mac → Develop → [Device name]

## Glossary

| Term | Definition |
|------|------------|
| DevTools | Browser's built-in debugging and inspection tools |
| Breakpoint | A point in code where execution pauses for inspection |
| Conditional breakpoint | Breakpoint that triggers only when a condition is true |
| Logpoint | Non-breaking breakpoint that logs a message to the console |
| Watch expression | Variable or expression monitored in the debugger |
| Call stack | The chain of function calls leading to the current execution point |
| Blackboxing | Hiding a script from the debugger's call stack |
| Pretty-print | Formatting minified code into readable form |
| Flame chart | Visual representation of function calls over time |
| Layout thrashing | Repeated forced reflows causing performance issues |
| Heap snapshot | Memory dump showing all objects and their references |
| Detached node | DOM element removed from the tree but still referenced in JS |
| HAR file | HTTP Archive — exported network log format |
| HMR | Hot Module Replacement — live module updates without full reload |
| Throttling | Simulating slower network or CPU conditions |

## Quick References

- [Chrome DevTools Docs](https://developer.chrome.com/docs/devtools/) — complete documentation
- [MDN: Console](https://developer.mozilla.org/en-US/docs/Web/API/console) — Console API reference
- [Firefox DevTools](https://firefox-source-docs.mozilla.org/devtools-user/) — Firefox DevTools docs
- [Safari Web Inspector](https://webkit.org/web-inspector/) — Safari DevTools docs
- [Get Started with DevTools](https://developer.chrome.com/docs/devtools/overview/) — official getting-started guide

## Next Steps

- [Performance & Memory Optimization](../performance/optimization.md) — deeper into performance tuning
- [TypeScript Basics](typescript-basics.md) — type-safe JavaScript for larger projects
- [JavaScript Modules & Tooling](modules-and-tooling.md) — build tools and bundlers
