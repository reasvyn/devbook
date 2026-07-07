# What Is JavaScript?

## Description

JavaScript is a high-level, interpreted programming language that runs in every web browser and on servers via Node.js. It is the only language natively supported by browsers, making it the universal language of the web.

## Prerequisites

- [Programming Fundamentals](../fundamentals/index.md) — core programming concepts

## Table of Contents

- [JavaScript vs. Other Languages](#javascript-vs-other-languages)
- [Where JavaScript Runs](#where-javascript-runs)
- [The JavaScript Ecosystem](#the-javascript-ecosystem)
- [Language Characteristics](#language-characteristics)
- [JavaScript and the Browser](#javascript-and-the-browser)
- [JavaScript Outside the Browser](#javascript-outside-the-browser)
- [TypeScript: JavaScript with Types](#typescript-javascript-with-types)
- [Modern JavaScript Development](#modern-javascript-development)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### JavaScript vs. Other Languages

JavaScript occupies a unique position among programming languages. It is not the fastest, the most elegant, or the most consistent language. But it is the most ubiquitous.

Unlike Python which is dominant in data science, or Rust which excels in systems programming, JavaScript's strength is its universal runtime environment — the web browser. Every device with a browser can execute JavaScript without any additional tooling.

This ubiquity created a virtuous cycle: more developers learned JavaScript, which led to more frameworks, which attracted more developers, which made the ecosystem richer. Today, JavaScript has the largest package ecosystem of any language with over two million packages on npm.

### Where JavaScript Runs

JavaScript runs in three main environments:

**Web browsers:** Every major browser includes a JavaScript engine. Chrome uses V8, Firefox uses SpiderMonkey, Safari uses JavaScriptCore. These engines parse, compile, and execute JavaScript code embedded in or linked from HTML pages.

**Node.js:** A server-side runtime built on V8 that allows JavaScript to run outside the browser. Node provides APIs for file system access, networking, HTTP servers, and process management.

**Other runtimes:** Deno (a modern Node alternative), Bun (a fast all-in-one runtime), Electron (desktop apps with web technologies), React Native (mobile apps).

### The JavaScript Ecosystem

JavaScript's ecosystem is vast and rapidly evolving. Key components include:

**Languages that compile to JavaScript:**

| Language | Description |
|----------|-------------|
| TypeScript | JavaScript with static types — adds type checking at compile time |
| JSX | JavaScript with XML-like syntax — used by React for component templates |
| CoffeeScript | Ruby-inspired syntax that compiles to JavaScript |

**Frameworks and libraries:**

- **React** — component-based UI library by Meta
- **Vue** — progressive framework for building UIs
- **Angular** — full-featured framework by Google
- **Svelte** — compile-time framework with minimal runtime
- **Express** — HTTP server framework for Node.js
- **Next.js** — React framework with server-side rendering
- **Remix** — full-stack web framework

**Build tools:**

- **Vite** — fast dev server and bundler
- **Webpack** — module bundler with rich plugin ecosystem
- **esbuild** — extremely fast bundler written in Go
- **Rollup** — efficient bundler for libraries
- **Parcel** — zero-configuration bundler

### Language Characteristics

JavaScript has several defining characteristics:

**Dynamically typed:** Variables can hold any type of value. Types are checked at runtime, not compile time.

```javascript
let value = 42;       // number
value = "hello";      // now a string
value = true;         // now a boolean
value = { key: "val" }; // now an object
```

**Loosely typed:** JavaScript performs implicit type coercion, which can be both convenient and surprising.

```javascript
"5" - 3;    // 2 (string coerced to number)
"5" + 3;    // "53" (number coerced to string — concatenation)
!"hello";   // false (string coerced to boolean)
```

**First-class functions:** Functions are values that can be assigned to variables, passed as arguments, and returned from other functions.

```javascript
const greet = function(name) {
    return `Hello, ${name}!`;
};

function callWith(name, fn) {
    return fn(name);
}

callWith("World", greet); // "Hello, World!"
```

**Prototype-based inheritance:** Objects inherit from other objects through prototype chains, unlike the class-based inheritance in Java or C++.

```javascript
const parent = { greet() { return "Hello"; } };
const child = Object.create(parent);
child.greet(); // "Hello" — inherited from parent
```

**Event-driven and asynchronous:** JavaScript uses an event loop to handle concurrent operations without threads. Callbacks, promises, and async/await manage asynchronous behaviour.

**Single-threaded:** JavaScript executes one operation at a time. Long-running operations block the main thread, which is why asynchronous patterns are essential.

### JavaScript and the Browser

In the browser, JavaScript has access to the Document Object Model (DOM), a tree representation of the HTML document. This enables dynamic manipulation of page content, structure, and style.

The browser environment provides several key APIs:

**DOM API:** Methods for selecting, creating, modifying, and removing elements. The interface between JavaScript and the HTML document.

```javascript
// Select an element
const header = document.querySelector("header");

// Create an element
const alert = document.createElement("div");
alert.textContent = "Hello!";
document.body.appendChild(alert);

// Modify styles
header.style.background = "#0066cc";
```

**Event API:** A system for responding to user interactions — clicks, key presses, form submissions, touch gestures.

```javascript
button.addEventListener("click", (event) => {
    console.log("Button clicked at", event.clientX, event.clientY);
});
```

**Fetch API:** A modern interface for making HTTP requests. Replaces the older XMLHttpRequest.

```javascript
const response = await fetch("/api/users");
const users = await response.json();
```

**Web APIs:** Browsers expose dozens of additional APIs — localStorage for client-side storage, Canvas for 2D graphics, WebSockets for real-time communication, Geolocation for location data, and many more.

### JavaScript Outside the Browser

Node.js brought JavaScript to the server, enabling full-stack development with a single language. Node's runtime provides APIs for:

```javascript
const fs = require("fs");
const http = require("http");

// Read a file
const data = fs.readFileSync("config.json", "utf-8");

// Create an HTTP server
const server = http.createServer((req, res) => {
    res.writeHead(200);
    res.end("Hello from Node!");
});

server.listen(3000);
```

Beyond servers, JavaScript powers desktop apps (Electron), mobile apps (React Native), command-line tools, databases (MongoDB uses JavaScript for queries), and even IoT devices.

### TypeScript: JavaScript with Types

TypeScript is a superset of JavaScript that adds static type checking. It was created by Microsoft and released in 2012. TypeScript code compiles to plain JavaScript, meaning it runs anywhere JavaScript runs.

```typescript
function greet(name: string): string {
    return `Hello, ${name}!`;
}

// Type error: Argument of type 'number' is not assignable to parameter of type 'string'
greet(42);
```

TypeScript catches entire categories of bugs at compile time — type mismatches, null references, missing properties, incorrect argument counts. This makes code more reliable, self-documenting, and refactorable at scale.

Most modern frontend projects use TypeScript by default. React, Vue, Angular, and Next.js all have first-class TypeScript support.

### The Event Loop in Depth

JavaScript's concurrency model is based on an event loop, which makes single-threaded execution work efficiently for I/O-bound applications.

```javascript
console.log("Start");

setTimeout(() => {
    console.log("Timeout callback");
}, 0);

Promise.resolve().then(() => {
    console.log("Promise microtask");
});

console.log("End");

// Output:
// Start
// End
// Promise microtask
// Timeout callback
```

The event loop processes tasks in phases:

1. Execute synchronous code on the call stack until it is empty.
2. Process all microtasks (Promise callbacks, queueMicrotask) in FIFO order.
3. Process one macrotask (setTimeout, setInterval, I/O callbacks) from the macrotask queue.
4. Render any pending UI updates (in browser environments).
5. Repeat.

This means microtasks always execute before the next macrotask, even if the macrotask was scheduled earlier. Promise-based code runs before timer-based code. Understanding this ordering is essential for debugging async code and avoiding subtle timing bugs.

```javascript
// Practical example: ensuring DOM updates before heavy computation
button.addEventListener("click", () => {
    button.textContent = "Processing...";

    // The DOM update will not render until the current task completes.
    // Use a microtask to yield to the rendering step:
    setTimeout(() => {
        // Heavy computation here
        const result = expensiveCalculation();
        button.textContent = `Result: ${result}`;
    }, 0);
});
```

### The Module System

JavaScript's module system evolved significantly over time. Understanding the different formats is essential for working with modern codebases.

**IIFE pattern (immediately invoked function expression)** was the pre-module approach to encapsulation:

```javascript
const myModule = (function() {
    let privateCounter = 0;

    return {
        increment() { privateCounter++; },
        getCount() { return privateCounter; }
    };
})();
```

**CommonJS** is the module system used by Node.js. It uses `require` for imports and `module.exports` for exports. Modules are loaded synchronously, which works fine for server-side code but not for browsers.

```javascript
// In file math.js
function add(a, b) { return a + b; }
module.exports = { add };

// In another file
const math = require("./math");
console.log(math.add(2, 3));
```

**ES modules (ESM)** are the standardized module system defined in ES6. They use `import` and `export` keywords and are loaded asynchronously with support for static analysis, tree-shaking, and cyclic dependencies.

```javascript
// lib.js
export const PI = 3.14159;
export function circumference(radius) {
    return 2 * PI * radius;
}

// app.js
import { circumference } from "./lib.js";
console.log(circumference(5));
```

ES modules are supported in all modern browsers and Node.js (since version 12). The ecosystem is transitioning from CommonJS to ESM, but the migration is gradual because it requires changing file extensions (`package.json` type field, `.mjs` extensions).

### Memory Management and Garbage Collection

JavaScript automatically manages memory through garbage collection. The V8 engine (used by Chrome and Node.js) uses a generational garbage collector:

- **Young generation** — newly allocated objects. Most objects die young (function-local variables, temporary objects). The young generation is collected frequently with a fast, scavenging collector.
- **Old generation** — objects that survive multiple young-generation collections. These are collected less frequently with a mark-sweep-compact collector that can cause noticeable pauses for very large heaps.

Memory leaks occur when objects are unintentionally retained:

```javascript
// Memory leak example: retained event listeners
class LeakyComponent {
    constructor(element) {
        this.element = element;
        this.handler = () => console.log("clicked");
        element.addEventListener("click", this.handler);
        // If the component is destroyed without removing the listener,
        // the component and its handler are retained by the element.
    }

    destroy() {
        this.element.removeEventListener("click", this.handler);
    }
}
```

Other common leak sources: global variables, forgotten timers (`setInterval` without `clearInterval`), closures that capture large objects, and detached DOM nodes held by JavaScript references.

Tools like Chrome DevTools Memory tab and Node.js `--inspect` help identify leaks by taking heap snapshots and comparing object counts between GC cycles.

### Modern JavaScript Development

A typical modern JavaScript development workflow includes:

1. **Write** code in `.js` or `.ts` files using an editor like VS Code
2. **Develop** locally with hot reloading via Vite or Next.js
3. **Build** with a bundler that transpiles, minifies, and optimises
4. **Test** with Vitest, Jest, or Playwright
5. **Lint** with ESLint and format with Prettier
6. **Deploy** to a hosting platform like Vercel, Netlify, or AWS

This workflow is standardized across most modern JavaScript projects, regardless of framework choice.

### JavaScript's Relationship with the Web Platform

JavaScript's unique position as the browser's native language means it is deeply intertwined with web standards. When the web platform gains a new capability — a graphics API, a sensor interface, a storage mechanism — JavaScript is how developers access it.

The Web Hypertext Application Technology Working Group (WHATWG) and the World Wide Web Consortium (W3C) maintain web standards that specify JavaScript APIs. The ECMAScript specification (maintained by TC39) defines the language itself. These standards evolve independently but in coordination: a new JavaScript feature might enable new kinds of APIs, and new platform capabilities might motivate new language features.

This relationship creates a tension. JavaScript must evolve to meet the demands of modern applications, but it cannot break the web — there are billions of existing pages that must continue to work. TC39 takes backward compatibility extremely seriously. No feature has ever been removed from JavaScript. Even deprecated features like `with` statements and `arguments.callee` remain supported for compatibility. This "don't break the web" principle constrains JavaScript's evolution in ways that other languages do not experience.

### Common Criticisms and Defenses

JavaScript attracts both passionate loyalty and intense criticism. Understanding both sides helps you use the language effectively:

**Criticism: Implicit type coercion is unpredictable.** The expression `[] + []` evaluates to an empty string. `[] + {}` evaluates to `"[object Object]"`. `{} + []` evaluates to `0` (because the `{}` is parsed as an empty code block, not an object). These behaviors are technically specified but counterintuitive.

**Defense:** Good JavaScript style avoids relying on coercion quirks. Linters (ESLint) and strict mode catch most problematic patterns. TypeScript eliminates the entire category of coercion bugs.

**Criticism: The event loop is confusing.** Understanding how the event loop, microtasks, and macrotasks interact requires mental models that are not obvious to beginners. The `setTimeout(fn, 0)` pattern for deferring execution seems like a hack.

**Defense:** The event loop makes JavaScript highly predictable for I/O-bound workloads. No other mainstream language makes concurrency as approachable as the async/await pattern does.

**Criticism: The ecosystem changes too fast.** JavaScript frameworks have a reputation for rapid churn. A project started with AngularJS in 2014 would need a complete rewrite to use modern Angular. Tools, patterns, and best practices shift every few years.

**Defense:** The core language changes slowly. Frameworks come and go, but the underlying JavaScript skills — closures, prototypes, the event model — remain relevant. The ecosystem's rapid evolution reflects a healthy, competitive community.

**Criticism: No type safety at scale.** A large JavaScript codebase without types becomes difficult to refactor. Renaming a function parameter requires hunting through every call site manually.

**Defense:** TypeScript solves this problem entirely. Most teams that reach the pain point of large-scale JavaScript adopt TypeScript and never look back.

### Learning Tips

- Practice JavaScript in the browser console (F12 → Console tab) — it is the most immediate REPL environment available.
- Focus on understanding `this`, closures, and the event loop before learning any framework. These three concepts are the foundation everything else builds on.
- Use strict mode (`"use strict"`) in all code — it eliminates silent errors and prepares you for modern JavaScript.
- Read the MDN documentation rather than blog posts when possible. MDN is maintained by Mozilla and reflects the actual specification.
- Write small programs that exercise each language feature — do not skip to frameworks until you understand the language itself.

### Glossary

| Term | Definition |
|------|------------|
| API | Application Programming Interface — methods and properties exposed by the runtime |
| Bundler | A tool that combines multiple source files into a single output file for the browser |
| Closure | A function that retains access to its lexical scope even when executed outside that scope |
| DOM | Document Object Model — the browser's tree representation of an HTML document |
| ECMAScript | The standardised specification that JavaScript implements (ES6, ES2024, etc.) |
| Engine | The program that parses, compiles, and executes JavaScript (V8, SpiderMonkey) |
| Event Loop | The mechanism that coordinates synchronous execution with async callbacks |
| First-class function | A function that can be assigned to variables, passed as arguments, and returned from functions |
| Framework | A pre-built structure for building applications (React, Vue, Angular) |
| Hoisting | The behavior of moving variable and function declarations to the top of their scope |
| Microtask | A task queued during event loop processing, executed before the next macrotask |
| npm | Node Package Manager — the default package manager for JavaScript |
| Polyfill | Code that implements a newer feature in environments that do not support it |
| Prototype chain | The inheritance mechanism where objects delegate property lookups to their prototype |
| Runtime | The environment where JavaScript executes (browser, Node.js, Deno) |
| Strict mode | A restricted variant of JavaScript that eliminates silent errors and enables optimizations |
| Superset | A language that extends another — all valid JavaScript is valid TypeScript |
| Transpile | To convert source code from one language to another (e.g., TypeScript to JavaScript) |
| Type coercion | The automatic or explicit conversion of a value from one type to another |
| Web API | Browser-provided interfaces accessible from JavaScript (Fetch, DOM, Canvas, etc.) |

### Quick References

- [MDN JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide) — comprehensive reference
- [JavaScript.info](https://javascript.info/) — modern tutorial with examples
- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/) — official TypeScript documentation
- [Node.js Documentation](https://nodejs.org/en/docs/) — server-side runtime reference
- [ECMAScript Specification](https://tc39.es/ecma262/) — the language specification
- [You Don't Know JS (book series)](https://github.com/getify/You-Dont-Know-JS) — deep dive into JavaScript's internals
- [JavaScript Visualized](https://www.lydiahallie.com/blog) — animated explanations of event loop, scope, and closures

### Next Steps

- [DOM Manipulation](dom-manipulation.md) — interacting with the page
- [Events](events.md) — responding to user input
- [Promises & Async/Await](promises-async-await.md) — asynchronous programming
