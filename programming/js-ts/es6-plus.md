# ES6+ Features

## Description

ECMAScript 2015 (ES6) and its yearly updates transformed JavaScript from a simple scripting language into a modern, expressive programming language with concise syntax, better scoping, and powerful new data structures.

## Prerequisites

- [Functions in Depth](functions-in-depth.md) — arrow functions already covered
- [Objects & Prototypes](objects-and-prototypes.md) — class syntax already covered

## Table of Contents

- [let & const](#let--const)
- [Destructuring](#destructuring)
- [Spread & Rest Operators](#spread--rest-operators)
- [Template Literals](#template-literals)
- [Enhanced Object Literals](#enhanced-object-literals)
- [Optional Chaining](#optional-chaining)
- [Nullish Coalescing](#nullish-coalescing)
- [Logical Assignment](#logical-assignment)
- [Numeric Separators](#numeric-separators)
- [BigInt](#bigint)
- [Symbol](#symbol)
- [Map & Set](#map--set)
- [WeakMap & WeakSet](#weakmap--weakset)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### let & const

Before ES6, `var` was the only way to declare variables. `let` and `const` fix `var`'s scoping problems.

**Block scoping vs. function scoping:**

```javascript
function example() {
    if (true) {
        var x = 10;        // scoped to function
        let y = 20;        // scoped to block
        const z = 30;      // scoped to block
    }
    console.log(x);        // 10 — accessible
    console.log(y);        // ReferenceError — not accessible
    console.log(z);        // ReferenceError — not accessible
}
```

**Temporal Dead Zone (TDZ):**

```javascript
console.log(a);            // undefined — var hoisted
var a = 1;

console.log(b);            // ReferenceError — TDZ
let b = 2;
```

Variables declared with `let` and `const` exist in the TDZ from the start of the block until the declaration is reached.

**No redeclaration:**

```javascript
var x = 1;
var x = 2;                 // allowed

let y = 1;
let y = 2;                 // SyntaxError

const z = 1;
z = 2;                     // TypeError: Assignment to constant variable
```

**const is not immutable — its binding is constant:**

```javascript
const obj = { name: "Alice" };
obj.name = "Bob";          // allowed — mutating property
obj = {};                  // TypeError — reassigning variable

const arr = [1, 2, 3];
arr.push(4);               // allowed
arr = [];                  // TypeError
```

**When to use what:**

- `const` by default — use for everything that doesn't need reassignment
- `let` when you need to reassign (counters, swaps)
- `var` — never in modern code

### Destructuring

**Array destructuring:**

```javascript
const colors = ["red", "green", "blue", "yellow"];

const [first, second] = colors;
console.log(first);                          // "red"
console.log(second);                         // "green"

// Skip elements
const [first, , third] = colors;
console.log(third);                          // "blue"

// Rest pattern
const [head, ...tail] = colors;
console.log(head);                           // "red"
console.log(tail);                           // ["green", "blue", "yellow"]

// Default values
const [a = 1, b = 2, c = 3] = [10];
console.log(a);                              // 10
console.log(b);                              // 2
console.log(c);                              // 3

// Swapping variables
let x = 1, y = 2;
[x, y] = [y, x];
console.log(x, y);                           // 2 1
```

**Object destructuring:**

```javascript
const user = {
    name: { first: "Alice", last: "Smith" },
    email: "alice@example.com",
    role: "admin",
    age: 30
};

const { name, email, role } = user;
console.log(name);                           // { first: "Alice", last: "Smith" }

// Rename
const { name: fullName, email: userEmail } = user;
console.log(fullName);                       // { first: "Alice", last: "Smith" }

// Default values
const { role, isActive = true } = user;
console.log(isActive);                       // true (default applied)

// Nested destructuring
const { name: { first, last } } = user;
console.log(first);                          // "Alice"

// Rest in objects
const { name, email, ...rest } = user;
console.log(rest);                           // { role: "admin", age: 30 }
```

**Destructuring function parameters:**

```javascript
function printUser({ name, email }) {
    console.log(`${name.first} <${email}>`);
}

printUser(user);

// With defaults
function config({ host = "localhost", port = 8080, tls = true } = {}) {
    return `${host}:${port} (${tls ? "TLS" : "plain"})`;
}

config({ host: "example.com" });             // "example.com:8080 (TLS)"
config();                                    // "localhost:8080 (TLS)"
```

### Spread & Rest Operators

Both use `...` but serve different purposes based on context.

**Rest — collect remaining items:**

```javascript
// Rest parameters
function sum(...numbers) {
    return numbers.reduce((a, b) => a + b, 0);
}
sum(1, 2, 3);                                // 6

// Rest in destructuring (arrays and objects)
const [first, ...rest] = [1, 2, 3, 4];
console.log(rest);                           // [2, 3, 4]

const { a, ...others } = { a: 1, b: 2, c: 3 };
console.log(others);                         // { b: 2, c: 3 }
```

**Spread — expand iterables or objects:**

```javascript
// Arrays
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const merged = [...arr1, ...arr2];
console.log(merged);                         // [1, 2, 3, 4, 5, 6]

const copy = [...arr1];
console.log(copy);                           // [1, 2, 3] (shallow copy)

// Pass array elements as arguments
const numbers = [4, 8, 15, 16, 23, 42];
Math.max(...numbers);                        // 42

// Strings
console.log([..."hello"]);                   // ["h", "e", "l", "l", "o"]

// Objects (ES2018)
const defaults = { theme: "dark", lang: "en" };
const overrides = { theme: "light" };
const config = { ...defaults, ...overrides };
console.log(config);                         // { theme: "light", lang: "en" }

// Create object copy (shallow)
const userCopy = { ...user };
```

**Spread creates shallow copies:**

```javascript
const original = { nested: { a: 1 } };
const copy = { ...original };
copy.nested.a = 99;
console.log(original.nested.a);              // 99 — shared reference
```

For deep cloning, use `structuredClone`:

```javascript
const deepCopy = structuredClone(original);
```

### Template Literals

Backtick strings support interpolation, multiline, and tagging.

**Interpolation:**

```javascript
const name = "Alice";
const age = 30;
console.log(`Hello, ${name}! You are ${age}.`);
// "Hello, Alice! You are 30."

// Expressions
console.log(`2 + 2 = ${2 + 2}`);             // "2 + 2 = 4"
console.log(`Uppercase: ${name.toUpperCase()}`); // "Uppercase: ALICE"
```

**Multiline strings:**

```javascript
const html = `
    <div class="card">
        <h2>${title}</h2>
        <p>${description}</p>
    </div>
`;
```

**Tagged templates — custom processing:**

```javascript
function highlight(strings, ...values) {
    let result = "";
    for (let i = 0; i < strings.length; i++) {
        result += strings[i];
        if (i < values.length) {
            result += `<strong>${values[i]}</strong>`;
        }
    }
    return result;
}

const name = "Alice";
const result = highlight`Hello, ${name}!`;
console.log(result);                         // "Hello, <strong>Alice</strong>!"
```

**Escaping backticks:**

```javascript
const code = "```";
```

### Enhanced Object Literals

**Shorthand properties:**

```javascript
const name = "Alice";
const age = 30;

// Old
const user1 = { name: name, age: age };

// ES6
const user2 = { name, age };
```

**Shorthand methods:**

```javascript
const calculator = {
    // Old
    add: function(a, b) { return a + b; },

    // ES6
    subtract(a, b) { return a - b; },

    // With computed name
    ["multiply"](a, b) { return a * b; }
};
```

**Computed property names:**

```javascript
const key = "dynamicField";
const obj = {
    [key]: "value",
    [`computed_${key}`]: true
};
console.log(obj.dynamicField);               // "value"
console.log(obj.computed_dynamicField);      // true
```

### Optional Chaining

Safe property access without checking each level:

```javascript
const user = {
    profile: {
        address: {
            city: "London"
        }
    }
};

// Without optional chaining
const city = user && user.profile && user.profile.address && user.profile.address.city;

// With optional chaining (ES2020)
const city2 = user?.profile?.address?.city;
console.log(city2);                          // "London"

// Missing path returns undefined
const zip = user?.profile?.address?.zip;
console.log(zip);                            // undefined

// Optional function call
const result = someFunction?.();             // undefined if null/undefined

// Optional dynamic property
const key = "maybeMissing";
const value = obj?.[key];
```

**Short-circuiting:** optional chaining stops evaluating as soon as it hits `null` or `undefined`.

```javascript
function getSafe(obj) {
    return obj?.nested?.array?.[0]?.name ?? "default";
}
```

### Nullish Coalescing

The `??` operator returns the right-hand side only when the left-hand side is `null` or `undefined` (not when it is falsy):

```javascript
const value = null ?? "default";
console.log(value);                          // "default"

const value2 = 0 ?? "default";
console.log(value2);                         // 0 — not replaced!

const value3 = "" ?? "default";
console.log(value3);                         // "" — not replaced!

const value4 = false ?? "default";
console.log(value4);                         // false — not replaced!
```

**Contrast with `||` (logical OR):**

```javascript
const count = 0;
console.log(count || 10);                    // 10 — 0 is falsy
console.log(count ?? 10);                    // 0 — 0 is not null/undefined
```

Use `??` when you want to distinguish between "missing" (`null`/`undefined`) and "falsy-but-valid" (`0`, `""`, `false`).

**Cannot use with `&&` or `||` without parentheses:**

```javascript
// SyntaxError
// const x = a ?? b || c;

// Correct
const x = (a ?? b) || c;
```

### Logical Assignment

Combines logical operators with assignment (ES2021):

```javascript
// Logical OR assignment
let a = null;
a ||= "default";                             // a = a || "default"
console.log(a);                              // "default"

// Logical AND assignment
let b = 1;
b &&= 2;                                     // b = b && 2
console.log(b);                              // 2

// Nullish assignment
let c = null;
c ??= "default";
console.log(c);                              // "default"

// Practical: default values for settings
const settings = {};
settings.theme ??= "light";
settings.fontSize ??= 16;
```

### Numeric Separators

Underscores improve readability of large numbers (ES2021):

```javascript
const billion = 1_000_000_000;
const bytes = 1_024;
const hex = 0xFF_FF_FF;
const binary = 0b1010_0001;
const scientific = 1_000_000.123_456;

console.log(billion);                        // 1000000000
```

### BigInt

Arbitrary precision integers (ES2020):

```javascript
const big = 123456789012345678901234567890n;
const alsoBig = BigInt("123456789012345678901234567890");

// Arithmetic
const result = big + 1n;                     // 123456789012345678901234567891n

// Cannot mix with regular numbers
// console.log(big + 1);                     // TypeError
console.log(big + BigInt(1));                // OK

// Comparisons work
console.log(1n === 1);                       // false (different types)
console.log(1n == 1);                        // true (loose equality)
console.log(2n > 1);                         // true

// Math operations
const power = 2n ** 100n;                    // 1267650600228229401496703205376n

// Division truncates toward zero
console.log(5n / 2n);                        // 2n
```

### Symbol

Unique, immutable values often used as object property keys:

```javascript
const sym1 = Symbol("id");
const sym2 = Symbol("id");
console.log(sym1 === sym2);                  // false — unique

// As object keys (hidden from normal iteration)
const user = {
    name: "Alice",
    [sym1]: 42
};

console.log(user[sym1]);                     // 42
console.log(Object.keys(user));              // ["name"]
console.log(Object.getOwnPropertySymbols(user)); // [Symbol(id)]
```

**Well-known symbols — customize language behavior:**

```javascript
class Range {
    constructor(start, end) {
        this.start = start;
        this.end = end;
    }

    // Make iterable
    [Symbol.iterator]() {
        let current = this.start;
        const end = this.end;
        return {
            next() {
                if (current <= end) {
                    return { value: current++, done: false };
                }
                return { done: true };
            }
        };
    }
}

const range = new Range(1, 5);
console.log([...range]);                     // [1, 2, 3, 4, 5]
```

**Symbol.for — global symbol registry:**

```javascript
const symA = Symbol.for("app.global");
const symB = Symbol.for("app.global");
console.log(symA === symB);                  // true — same symbol
```

### Map & Set

**Map — key-value storage with any key type:**

```javascript
const map = new Map();

// Set
map.set("name", "Alice");
map.set(42, "the answer");
map.set({ id: 1 }, "object key");

// Get
console.log(map.get("name"));                // "Alice"
console.log(map.get(42));                    // "the answer"

// Check existence
console.log(map.has("name"));                // true

// Delete
map.delete(42);

// Size
console.log(map.size);                       // 2

// Iteration
for (const [key, value] of map) {
    console.log(key, value);
}

// Convert to array
const entries = [...map];
const keys = [...map.keys()];
const values = [...map.values()];
```

**Map vs. Object:**

| Feature | Object | Map |
|---------|--------|-----|
| Key type | String/Symbol | Any type |
| Key order | Integer keys first, then insertion | Insertion order |
| Size | Manual (`Object.keys(obj).length`) | `map.size` |
| Iteration | `for...in` (includes prototype) | `for...of` (direct) |
| Performance | Optimized for string keys | Optimized for frequent additions/deletions |
| Serialization | Native JSON support | No native serialization |

**Map from entries array:**

```javascript
const entries = [["a", 1], ["b", 2], ["c", 3]];
const map = new Map(entries);
```

**Set — unique values:**

```javascript
const set = new Set();

set.add(1);
set.add(2);
set.add(2);                                  // ignored — already exists
set.add(3);

console.log(set.size);                       // 3
console.log(set.has(2));                     // true
console.log(set.has(4));                     // false

set.delete(2);
console.log(set.size);                       // 2

set.clear();

// Initialize from array
const numbers = [1, 2, 2, 3, 3, 3, 4];
const unique = new Set(numbers);
console.log([...unique]);                    // [1, 2, 3, 4]

// Iteration
const set = new Set(["a", "b", "c"]);
for (const value of set) {
    console.log(value);
}
```

**Set operations:**

```javascript
const a = new Set([1, 2, 3, 4]);
const b = new Set([3, 4, 5, 6]);

// Union
const union = new Set([...a, ...b]);

// Intersection
const intersection = new Set([...a].filter(x => b.has(x)));

// Difference (a minus b)
const difference = new Set([...a].filter(x => !b.has(x)));
```

### WeakMap & WeakSet

Garbage-collectable key-value pairs (keys must be objects):

```javascript
const weakMap = new WeakMap();
let obj = { id: 1 };

weakMap.set(obj, "private data");
console.log(weakMap.get(obj));               // "private data"

obj = null;                                  // object can be garbage collected
// weakMap entry is automatically removed
```

**Properties of WeakMap/WeakSet:**

- Keys must be objects (not primitives)
- No reference counting — allows garbage collection
- Not iterable — no `.keys()`, `.values()`, `.entries()`, or `.size`
- Only has `.get()`, `.set()`, `.has()`, `.delete()` (WeakMap)
- Only has `.add()`, `.has()`, `.delete()` (WeakSet)

**Use case — private data attached to DOM nodes:**

```javascript
const elementData = new WeakMap();

function trackClick(element) {
    if (!elementData.has(element)) {
        elementData.set(element, { clicks: 0 });
    }
    const data = elementData.get(element);
    data.clicks++;
}

// When element is removed from DOM and lost, the WeakMap entry is GC'd
```

## Glossary

| Term | Definition |
|------|------------|
| ES6 | ECMAScript 2015 — major update that introduced let, const, arrow functions, classes, modules |
| Block scope | Variables scoped to the nearest curly brace block (vs. function scope) |
| TDZ | Temporal Dead Zone — period where let/const variables exist but are not initialized |
| Destructuring | Extracting values from arrays or objects into individual variables |
| Spread | Expanding an iterable or object into individual elements |
| Rest | Collecting remaining elements into an array or object |
| Template literal | Backtick-delimited string with interpolation and multiline support |
| Optional chaining | Safe property access (`?.`) that returns undefined if null/undefined |
| Nullish coalescing | Operator (`??`) that returns RHS only for null/undefined |
| Symbol | Unique primitive value used as object property key |
| BigInt | Arbitrary precision integer type |
| Map | Key-value collection with any key type and insertion order |
| Set | Collection of unique values |
| WeakMap | Map with object keys that allows garbage collection |
| Proxy | Object wrapper that intercepts property access, assignment, and deletion |

## Quick References

- [MDN: JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide) — comprehensive JS guide
- [MDN: Expressions and Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators) — operator reference
- [MDN: Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) — Map reference
- [MDN: Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set) — Set reference
- [MDN: Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy) — Proxy reference
- [MDN: Template Literals](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Template_literals)

## Next Steps

- [JavaScript Modules & Tooling](modules-and-tooling.md) — importing and exporting modern JS
- [Objects & Prototypes](objects-and-prototypes.md) — deeper into objects
- [Promises & Async/Await](promises-async-await.md) — async patterns in modern JS
