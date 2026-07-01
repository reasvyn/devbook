# JavaScript Modules & Tooling

## Description

ES Modules (ESM) are the official JavaScript module system built into the language and modern browsers. Alongside npm for dependency management and bundlers for production builds, modules form the backbone of any real-world JavaScript project.

## Prerequisites

- [ES6+ Features](es6-plus.md) — syntax foundation including spread, destructuring, and arrow functions
- [Values & Types](values-and-types.md) — understanding scope and closures

## Table of Contents

- [The Module Pattern](#the-module-pattern)
- [ES Modules Basics](#es-modules-basics)
- [Named Exports & Imports](#named-exports--imports)
- [Default Exports & Imports](#default-exports--imports)
- [Re-exporting & Aggregating](#re-exporting--aggregating)
- [Dynamic Imports](#dynamic-imports)
- [Module Resolution](#module-resolution)
- [npm: The Package Manager](#npm-the-package-manager)
- [package.json](#packagejson)
- [Bundlers: Why They Exist](#bundlers-why-they-exist)
- [Build Scripts](#build-scripts)
- [Environment Variables](#environment-variables)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Module Pattern

Before ES Modules, JavaScript had no built-in module system. Developers invented patterns:

**IIFE module (Revealing Module Pattern):**

```javascript
const CounterModule = (function() {
    let count = 0;                           // private

    function increment() { count++; }
    function decrement() { count--; }
    function getCount() { return count; }

    return { increment, decrement, getCount };
})();

CounterModule.increment();
console.log(CounterModule.getCount());       // 1
console.log(CounterModule.count);            // undefined — private
```

**Problems with this approach:**

- No dependency management
- Script order matters in HTML
- Global namespace pollution from script tags
- No tree-shaking (dead code elimination)

ES Modules solve all of these.

### ES Modules Basics

An ES Module is a file with `export` and `import` statements. The browser or runtime treats any file containing these as a module.

**In browsers — use `<script type="module">`:**

```html
<script type="module" src="app.js"></script>
```

Module scripts are **deferred by default** — they wait for HTML parsing, then execute in order:

```html
<script type="module" src="main.js"></script>   <!-- deferred -->
<script src="legacy.js"></script>                <!-- blocks parsing -->
```

**Module features:**

```javascript
// module.js
export const VALUE = 42;
export function utility() { /* ... */ }

// Each module has its own top-level scope
const localVariable = "not exported";            // not visible outside
```

- Modules are **strict mode** by default
- Each module has its own scope — variables are not global
- Modules are **singletons** — imported once, cached, and reused
- Circular dependencies are handled (partially loaded references)
- Modules can be **static** (import/export at top level) or **dynamic** (import())

### Named Exports & Imports

**Named exports — multiple per file:**

```javascript
// math.js
export const PI = 3.14159;
export function add(a, b) { return a + b; }
export function multiply(a, b) { return a * b; }
```

**Named imports — use exact names:**

```javascript
// app.js
import { PI, add, multiply } from "./math.js";

console.log(PI);                               // 3.14159
console.log(add(2, 3));                        // 5
```

**Import all as namespace:**

```javascript
import * as MathUtils from "./math.js";

console.log(MathUtils.PI);                     // 3.14159
console.log(MathUtils.add(2, 3));              // 5
```

**Alias named imports:**

```javascript
import { add as sum, multiply as product } from "./math.js";
```

**Inline export — export directly at declaration:**

```javascript
export const config = { theme: "dark" };
export class User { /* ... */ }
export default function() { /* ... */ }
```

### Default Exports & Imports

Each module can have at most one default export:

```javascript
// logger.js
export default function log(message) {
    console.log(`[LOG] ${message}`);
}
```

**Default import — any name:**

```javascript
import logger from "./logger.js";
logger("App started");

// Any name works
import myLogger from "./logger.js";
myLogger("Hello");
```

**Combining default and named:**

```javascript
// utils.js
export default function main() { /* ... */ }
export function helper() { /* ... */ }
export const VERSION = "1.0";
```

```javascript
import main, { helper, VERSION } from "./utils.js";
```

**Re-exporting default:**

```javascript
export { default } from "./utils.js";
```

### Re-exporting & Aggregating

**Barrel files — aggregate exports from multiple modules:**

```javascript
// components/index.js
export { Button } from "./Button.js";
export { Input } from "./Input.js";
export { Modal } from "./Modal.js";
```

```javascript
// app.js — import from barrel
import { Button, Input, Modal } from "./components/index.js";
```

**Re-export with rename:**

```javascript
export { Button as AppButton } from "./components/Button.js";
```

**Re-export all (except default):**

```javascript
export * from "./math.js";                     // re-exports all named exports
```

### Dynamic Imports

Static imports (`import ... from ...`) are evaluated at parse time. Dynamic imports return a Promise and load on demand:

```javascript
// Static — loaded eagerly
import { heavyLibrary } from "./heavy.js";

// Dynamic — loaded when needed
async function loadHeavyFeature() {
    try {
        const module = await import("./heavy.js");
        module.heavyLibrary.doSomething();
    } catch (error) {
        console.error("Failed to load module:", error);
    }
}
```

**Use cases for dynamic imports:**

- **Code splitting** — load code only for specific routes
- **Conditional loading** — load polyfills only when needed
- **Lazy initialization** — defer expensive module initialization

```javascript
// Route-based code splitting
const routes = {
    "/dashboard": () => import("./pages/Dashboard.js"),
    "/settings": () => import("./pages/Settings.js"),
    "/profile": () => import("./pages/Profile.js")
};

async function navigate(path) {
    const loadPage = routes[path];
    if (loadPage) {
        const page = await loadPage();
        page.render();
    }
}
```

**Dynamic import with template literal:**

```javascript
const locale = navigator.language.slice(0, 2);
const i18n = await import(`./locales/${locale}.js`);
```

But this restricts bundler optimization — explicit strings are preferred.

**Import assertions (import attributes):**

```javascript
// Import JSON with assertion (modern syntax)
const data = await import("./data.json", {
    assert: { type: "json" }
});
```

### Module Resolution

**Relative paths** — start with `.` or `..`:

```javascript
import { helper } from "./utils/helpers.js";
import { config } from "../config.js";
```

**Absolute paths (bare specifiers)** — used for npm packages:

```javascript
import React from "react";
import { debounce } from "lodash-es";
```

Bare specifiers are resolved by the bundler or Node.js's module resolution:

1. Check `node_modules/` for the package name
2. Use the `"exports"` or `"main"` field in the package's `package.json`
3. Resolve to the actual `.js` file

**File extensions in browsers:**

Browsers require full paths including extensions:

```javascript
// Browser (must include .js)
import { helper } from "./helpers.js";

// Bundlers (usually resolve without extension)
import { helper } from "./helpers";
```

**Node.js ESM resolution** — requires extensions:

```javascript
// Node.js ESM
import { helper } from "./helpers.js";    // required
import { helper } from "./helpers/index.js";
```

### npm: The Package Manager

npm installs and manages third-party packages from the npm registry.

**Essential commands:**

```bash
# Initialize a project
npm init
npm init -y                                  # skip prompts

# Install packages
npm install react                            # production dependency
npm install -D vitest                        # dev dependency
npm install -g typescript                    # global install

# Remove
npm uninstall lodash

# Update
npm update react

# List installed
npm list --depth=0
```

**Semantic versioning:**

```
"react": "^18.2.0"
         │││└── patch (bug fixes)
         ││└── minor (new features, backward compatible)
         │└── major (breaking changes)
         └── range prefix
```

| Prefix | Behavior |
|--------|----------|
| `^18.2.0` | Compatible with minor updates (`>=18.2.0 <19.0.0`) |
| `~18.2.0` | Only patch updates (`>=18.2.0 <18.3.0`) |
| `18.2.0` | Exact version only |
| `*` | Any version |

**package-lock.json:**

- Records exact versions of every dependency and its transitive dependencies
- Ensures reproducible installs across environments
- Always commit this file to version control

### package.json

The heart of any Node.js project:

```json
{
    "name": "my-app",
    "version": "1.0.0",
    "description": "My application",
    "type": "module",
    "main": "dist/index.js",
    "scripts": {
        "dev": "vite",
        "build": "vite build",
        "test": "vitest run",
        "lint": "eslint src/"
    },
    "dependencies": {
        "react": "^18.2.0"
    },
    "devDependencies": {
        "vite": "^5.0.0",
        "vitest": "^1.0.0",
        "eslint": "^8.0.0"
    }
}
```

**`"type": "module"`** — tells Node.js to treat `.js` files as ES Modules.

**Scripts — alias commands:**

```bash
npm run dev     # runs "vite"
npm run build   # runs "vite build"
npm test        # runs "vitest run" (test can omit "run")
```

### Bundlers: Why They Exist

Browsers ship ES Modules natively, so why bundlers?

**Problems with native ESM in browsers:**

1. **Too many HTTP requests** — each `import` is a separate network request
2. **Bare specifiers** — browsers do not understand `import React from "react"`
3. **No optimization** — no minification, tree-shaking, or code splitting
4. **Development features** — no HMR (Hot Module Replacement)

**Bundlers solve these by:**

- Combining modules into fewer files
- Resolving bare specifiers to `node_modules`
- Tree-shaking — removing unused exports
- Minifying and optimizing output
- Providing dev servers with HMR

**Popular bundlers:**

| Tool | Type | Key Feature |
|------|------|-------------|
| Vite | Bundler + Dev Server | Fast HMR via native ESM in dev, Rollup for build |
| Webpack | Bundler | Highly configurable, mature ecosystem |
| Rollup | Bundler | Optimized for library builds, tree-shaking |
| esbuild | Bundler + Minifier | Extremely fast (Go-based) |
| Parcel | Bundler | Zero-config, fast |

**Minimal Vite setup:**

```javascript
// vite.config.js
import { defineConfig } from "vite";

export default defineConfig({
    build: {
        outDir: "dist",
        sourcemap: true
    }
});
```

### Build Scripts

Typical project scripts for development and production:

```json
{
    "scripts": {
        "dev": "vite",
        "build": "vite build",
        "preview": "vite preview",
        "test": "vitest run",
        "test:watch": "vitest",
        "lint": "eslint src/",
        "lint:fix": "eslint src/ --fix",
        "format": "prettier --write src/",
        "typecheck": "tsc --noEmit",
        "check-all": "npm run lint && npm run typecheck && npm run test",
        "clean": "rm -rf dist"
    }
}
```

**Pre/Post hooks:**

```bash
# Running "npm run build" also runs:
npm run prebuild     # before build (if defined)
npm run build
npm run postbuild    # after build (if defined)
```

**Environment-specific scripts:**

```bash
NODE_ENV=production npm run build       # set environment
npm run build -- --mode staging         # pass flags
```

### Environment Variables

```bash
# .env
API_URL=https://api.example.com
API_KEY=sk-abc123
NODE_ENV=production
```

```bash
# .env.development
API_URL=http://localhost:3000
```

```javascript
// Using Vite's env handling
const apiUrl = import.meta.env.VITE_API_URL;
```

**Convention (Vite-based):**

- `.env` — loaded in all cases
- `.env.development` — dev only
- `.env.production` — production only
- Only variables prefixed with `VITE_` are exposed to client code

**Security note:** Environment variables in `.env` files are **bundled into the client code** at build time. Never store secrets that should remain server-side.

## Glossary

| Term | Definition |
|------|------------|
| ES Module | The official JavaScript module system using `import`/`export` |
| Named export | An export with a specific name, imported by matching name |
| Default export | The main export of a module, imported with any name |
| Bare specifier | An import path without `./` or `../` — resolved from node_modules |
| Dynamic import | Runtime import using `import()` that returns a Promise |
| Code splitting | Splitting code into separate bundles loaded on demand |
| Tree-shaking | Removing unused exports from the final bundle |
| Bundle | A single file combining multiple modules for production |
| Bundler | A tool that resolves, transforms, and combines modules |
| Barrel file | An index.js that re-exports from multiple files |
| npm | Node Package Manager — package registry and CLI tool |
| package.json | Project metadata including dependencies and scripts |
| Semantic versioning | Version scheme: major.minor.patch |
| HMR | Hot Module Replacement — updating modules without full reload |
| Transpilation | Converting modern JS to older syntax for compatibility |

## Quick References

- [MDN: JavaScript Modules](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Modules) — guide to ES modules
- [MDN: import](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) — import statement reference
- [MDN: export](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/export) — export statement reference
- [npm Documentation](https://docs.npmjs.com/) — full npm CLI and registry docs
- [Vite Guide](https://vite.dev/guide/) — Vite build tool documentation
- [Node.js ESM Docs](https://nodejs.org/api/esm.html) — ESM in Node.js

## Next Steps

- [TypeScript Basics](typescript-basics.md) — adding types to your modules
- [Debugging with DevTools](debugging-devtools.md) — debugging module loading issues
- [ES6+ Features](es6-plus.md) — syntax used in modern module code
