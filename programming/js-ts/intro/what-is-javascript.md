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

### Modern JavaScript Development

A typical modern JavaScript development workflow includes:

1. **Write** code in `.js` or `.ts` files using an editor like VS Code
2. **Develop** locally with hot reloading via Vite or Next.js
3. **Build** with a bundler that transpiles, minifies, and optimises
4. **Test** with Vitest, Jest, or Playwright
5. **Lint** with ESLint and format with Prettier
6. **Deploy** to a hosting platform like Vercel, Netlify, or AWS

This workflow is standardized across most modern JavaScript projects, regardless of framework choice.

### Glossary

| Term | Definition |
|------|------------|
| API | Application Programming Interface — methods and properties exposed by the runtime |
| Bundler | A tool that combines multiple source files into a single output file for the browser |
| DOM | Document Object Model — the browser's tree representation of an HTML document |
| ECMAScript | The standardised specification that JavaScript implements (ES6, ES2024, etc.) |
| Engine | The program that parses, compiles, and executes JavaScript (V8, SpiderMonkey) |
| Event Loop | The mechanism that coordinates synchronous execution with async callbacks |
| Framework | A pre-built structure for building applications (React, Vue, Angular) |
| npm | Node Package Manager — the default package manager for JavaScript |
| Runtime | The environment where JavaScript executes (browser, Node.js, Deno) |
| Superset | A language that extends another — all valid JavaScript is valid TypeScript |
| Transpile | To convert source code from one language to another (e.g., TypeScript to JavaScript) |

### Quick References

- [MDN JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide) — comprehensive reference
- [JavaScript.info](https://javascript.info/) — modern tutorial with examples
- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/) — official TypeScript documentation
- [Node.js Documentation](https://nodejs.org/en/docs/) — server-side runtime reference
- [ECMAScript Specification](https://tc39.es/ecma262/) — the language specification

### Next Steps

- [DOM Manipulation](dom-manipulation.md) — interacting with the page
- [Events](events.md) — responding to user input
- [Promises & Async/Await](promises-async-await.md) — asynchronous programming
