# Functions in Depth

## Description

Functions are the fundamental building block of JavaScript. They are values, they create scope, they capture closures, and they behave differently depending on how they are called. Mastering functions is mastering JavaScript.

## Prerequisites

- [Values & Types](values-and-types.md) — type system fundamentals
- [Programming Fundamentals: Functions](../fundamentals/functions.md) — basic function syntax

## Table of Contents

- [Functions Are Values](#functions-are-values)
- [Function Declarations vs. Expressions](#function-declarations-vs-expressions)
- [Arrow Functions](#arrow-functions)
- [Scope & the Scope Chain](#scope--the-scope-chain)
- [Closures](#closures)
- [The `this` Keyword](#the-this-keyword)
- [Call, Apply, and Bind](#call-apply-and-bind)
- [Default Parameters](#default-parameters)
- [Rest Parameters & Arguments](#rest-parameters--arguments)
- [IIFE: Immediately Invoked Function Expressions](#iife-immediately-invoked-function-expressions)
- [Higher-Order Functions](#higher-order-functions)
- [Recursion](#recursion)
- [Pure Functions](#pure-functions)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Functions Are Values

In JavaScript, functions are first-class objects. They can be assigned to variables, passed as arguments, returned from other functions, and stored in data structures.

```javascript
// Assign to variable
const greet = function(name) {
    return `Hello, ${name}!`;
};

// Pass as argument
function callWithHello(fn) {
    fn("World");
}

// Return from function
function createMultiplier(factor) {
    return function(x) {
        return x * factor;
    };
}

const double = createMultiplier(2);
double(5);                 // 10

// Store in array
const operations = [
    x => x + 1,
    x => x * 2,
    x => x ** 2
];

operations[0](5);          // 6
operations[1](5);          // 10
```

Functions have a `name` property and a `length` property (number of parameters):

```javascript
function add(a, b) {}
add.name;                  // "add"
add.length;                // 2

const fn = function() {};
fn.name;                   // "fn"
```

### Function Declarations vs. Expressions

**Function declarations** are hoisted — they can be called before they appear in the code:

```javascript
hoisted();                 // Works — "Hoisted!"
function hoisted() {
    console.log("Hoisted!");
}
```

**Function expressions** are not hoisted. The variable is hoisted but not the assignment:

```javascript
notHoisted();              // TypeError: notHoisted is not a function
const notHoisted = function() {
    console.log("This won't work");
};
```

**Named function expressions** provide better stack traces and enable recursion:

```javascript
const factorial = function calc(n) {
    if (n <= 1) return 1;
    return n * calc(n - 1); // name used inside
};

factorial(5);              // 120
// calc is not accessible outside
```

### Arrow Functions

Arrow functions provide concise syntax and inherit `this` from the surrounding scope:

```javascript
// Concise body — implicit return
const add = (a, b) => a + b;

// Block body — explicit return
const greet = (name) => {
    const time = new Date().getHours();
    return `Good ${time < 12 ? "morning" : "afternoon"}, ${name}!`;
};

// Single parameter — parentheses optional
const square = x => x * x;

// No parameters — parentheses required
const now = () => Date.now();
```

Arrow functions differ from regular functions in key ways:

- No `this` binding — they capture `this` from the enclosing scope
- No `arguments` object
- Cannot be used as constructors (no `new`)
- No `prototype` property
- Cannot use `super`

```javascript
const obj = {
    name: "Alice",
    greet: function() {
        // Regular function — has its own this
        setTimeout(function() {
            console.log(this.name); // undefined (window/global)
        }, 100);
    },
    greetArrow: function() {
        // Arrow inherits this from greetArrow's this
        setTimeout(() => {
            console.log(this.name); // "Alice"
        }, 100);
    }
};
```

### Scope & the Scope Chain

JavaScript uses lexical scoping — where a variable is accessible depends on where it is declared in the source code.

**Global scope:** Variables declared outside any function or block are global.

```javascript
const globalVar = "I'm global";

function test() {
    console.log(globalVar); // accessible
}
```

**Function scope:** `var` declarations are scoped to the function. `let` and `const` are scoped to the block.

```javascript
function example() {
    if (true) {
        var x = 1;          // function-scoped
        let y = 2;          // block-scoped
        const z = 3;        // block-scoped
    }
    console.log(x);         // 1 — var is function-scoped
    console.log(y);         // ReferenceError: y is not defined
}
```

**Scope chain:** Inner scopes can access outer scopes, but not vice versa:

```javascript
const global = "global";

function outer() {
    const outerVar = "outer";

    function inner() {
        const innerVar = "inner";
        console.log(global);  // "global"
        console.log(outerVar); // "outer"
        console.log(innerVar); // "inner"
    }

    console.log(innerVar); // ReferenceError
}
```

### Closures

A closure is a function that remembers the variables from its lexical scope even when the function is executed outside that scope:

```javascript
function createCounter() {
    let count = 0;

    return function() {
        count++;
        return count;
    };
}

const counter = createCounter();
counter();                 // 1
counter();                 // 2
counter();                 // 3

// Each call to createCounter creates a new closure
const counter2 = createCounter();
counter2();                // 1
```

Closures are the foundation of many JavaScript patterns:

**Data privacy:**

```javascript
function createBankAccount(initialBalance) {
    let balance = initialBalance;

    return {
        deposit(amount) {
            balance += amount;
            return balance;
        },
        withdraw(amount) {
            if (amount > balance) throw new Error("Insufficient funds");
            balance -= amount;
            return balance;
        },
        getBalance() {
            return balance;
        }
    };
}

const account = createBankAccount(1000);
account.deposit(500);      // 1500
account.balance;           // undefined — private!
account.getBalance();      // 1500
```

**Function factories:**

```javascript
function createGreeting(greeting) {
    return function(name) {
        return `${greeting}, ${name}!`;
    };
}

const sayHello = createGreeting("Hello");
const sayHi = createGreeting("Hi");

sayHello("Alice");         // "Hello, Alice!"
sayHi("Bob");              // "Hi, Bob!"
```

### The `this` Keyword

`this` is determined by how a function is called, not where it is defined.

**1. Default binding — `this` is the global object (or `undefined` in strict mode):**

```javascript
function showThis() {
    console.log(this);
}

showThis();                // window (browser) / global (Node)
                           // undefined in strict mode
```

**2. Implicit binding — `this` is the object the method is called on:**

```javascript
const user = {
    name: "Alice",
    greet() {
        console.log(`Hello, I'm ${this.name}`);
    }
};

user.greet();              // "Hello, I'm Alice"

const greet = user.greet;
greet();                   // "Hello, I'm undefined" — this is now global
```

**3. Explicit binding — `call`, `apply`, `bind` set `this` explicitly:**

```javascript
function introduce(greeting) {
    console.log(`${greeting}, I'm ${this.name}`);
}

const user1 = { name: "Alice" };
const user2 = { name: "Bob" };

introduce.call(user1, "Hi");    // "Hi, I'm Alice"
introduce.apply(user2, ["Hey"]); // "Hey, I'm Bob"

const bound = introduce.bind(user1);
bound("Hello");                 // "Hello, I'm Alice"
```

**4. Constructor binding — `this` is the new instance:**

```javascript
function Person(name) {
    this.name = name;
}

const alice = new Person("Alice");
alice.name;                // "Alice"
```

**5. Arrow functions — `this` is captured from the enclosing scope:**

```javascript
function Timer() {
    this.seconds = 0;
    setInterval(() => {
        this.seconds++;     // this refers to Timer instance
    }, 1000);
}
```

### Call, Apply, and Bind

These methods control `this` explicitly:

```javascript
function logInfo(prefix, suffix) {
    console.log(`${prefix} ${this.name} ${suffix}`);
}

const user = { name: "Alice" };

// call — arguments listed individually
logInfo.call(user, "Mr.", "Esq.");
// "Mr. Alice Esq."

// apply — arguments as an array
logInfo.apply(user, ["Ms.", "PhD"]);
// "Ms. Alice PhD"

// bind — returns a new function with bound this
const boundLog = logInfo.bind(user, "Dr.");
boundLog("MD");
// "Dr. Alice MD"
```

`bind` is commonly used for event handlers and setTimeout:

```javascript
class Button {
    constructor(text) {
        this.text = text;
    }

    handleClick() {
        console.log(`${this.text} clicked`);
    }

    render() {
        const button = document.createElement("button");
        button.textContent = this.text;
        button.addEventListener("click", this.handleClick.bind(this));
        return button;
    }
}
```

### Default Parameters

ES6 introduced default parameter values:

```javascript
function greet(name = "Guest") {
    return `Hello, ${name}!`;
}

greet("Alice");            // "Hello, Alice!"
greet();                   // "Hello, Guest!"

// Defaults are evaluated at call time
function createUrl(path, base = window.location.origin) {
    return `${base}${path}`;
}

// Earlier params can be used in later defaults
function multiply(a, b = a * 2) {
    return a * b;
}

multiply(3);               // 18 (3 * 6)
multiply(3, 4);            // 12
```

Defaults handle `undefined` but not `null`:

```javascript
function test(x = 10) {
    console.log(x);
}

test(undefined);           // 10 — undefined triggers default
test(null);                // null — null does not trigger default
test();                    // 10 — missing arg is undefined
```

### Rest Parameters & Arguments

**Rest parameters** collect remaining arguments into an array:

```javascript
function sum(...numbers) {
    return numbers.reduce((total, n) => total + n, 0);
}

sum(1, 2, 3);              // 6
sum(1, 2, 3, 4, 5);       // 15

// Rest must be the last parameter
function log(level, ...messages) {
    console.log(`[${level}]`, ...messages);
}
```

**The `arguments` object** is an array-like object available in regular functions (not arrows):

```javascript
function oldSum() {
    // arguments is array-like, not a real array
    let total = 0;
    for (let i = 0; i < arguments.length; i++) {
        total += arguments[i];
    }
    return total;
}

// Convert to real array
function convertArgs() {
    const args = Array.from(arguments);
    // or: const args = [...arguments];
    return args.slice(1);
}
```

### IIFE: Immediately Invoked Function Expressions

IIFEs execute immediately after definition:

```javascript
(function() {
    const private = "This is scoped";
    console.log(private);
})();

// With arrow
(() => {
    console.log("IIFE with arrow");
})();

// With parameters
((name) => {
    console.log(`Hello, ${name}`);
})("World");
```

IIFEs were historically used for:
- Creating private scope (before `let`/`const`)
- Module pattern (before ES modules)
- Avoiding global variable pollution

```javascript
const module = (function() {
    const privateVar = "secret";

    function privateMethod() {
        console.log(privateVar);
    }

    return {
        publicMethod() {
            privateMethod();
        }
    };
})();
```

### Higher-Order Functions

A higher-order function takes a function as an argument, returns a function, or both:

```javascript
// Takes a function
function withLogging(fn) {
    return function(...args) {
        console.log(`Calling with:`, args);
        const result = fn(...args);
        console.log(`Result:`, result);
        return result;
    };
}

const addWithLog = withLogging((a, b) => a + b);
addWithLog(3, 4);
// Calling with: [3, 4]
// Result: 7

// Returns a function
function memoize(fn) {
    const cache = new Map();
    return function(...args) {
        const key = JSON.stringify(args);
        if (cache.has(key)) return cache.get(key);
        const result = fn(...args);
        cache.set(key, result);
        return result;
    };
}

const expensive = memoize((n) => {
    console.log("Computing...");
    return n * n;
});
```

Built-in higher-order functions:

```javascript
// Array methods
[1, 2, 3].map(x => x * 2);     // [2, 4, 6]
[1, 2, 3].filter(x => x > 1);  // [2, 3]
[1, 2, 3].reduce((a, b) => a + b); // 6

// Function composition
const compose = (...fns) => (x) => fns.reduceRight((v, f) => f(v), x);
const pipe = (...fns) => (x) => fns.reduce((v, f) => f(v), x);

const addOne = x => x + 1;
const double = x => x * 2;

compose(double, addOne)(5);     // 12 — double(addOne(5))
pipe(addOne, double)(5);        // 12 — double(addOne(5))
```

### Recursion

A recursive function calls itself:

```javascript
function factorial(n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}

factorial(5);              // 120
```

**Tree traversal:**

```javascript
const tree = {
    value: 1,
    children: [
        { value: 2, children: [] },
        {
            value: 3,
            children: [
                { value: 4, children: [] },
                { value: 5, children: [] }
            ]
        }
    ]
};

function sumTree(node) {
    let total = node.value;
    for (const child of node.children) {
        total += sumTree(child);
    }
    return total;
}

sumTree(tree);             // 15
```

**Deep object cloning:**

```javascript
function deepClone(obj) {
    if (obj === null || typeof obj !== "object") return obj;
    if (Array.isArray(obj)) return obj.map(deepClone);
    const cloned = {};
    for (const key of Object.keys(obj)) {
        cloned[key] = deepClone(obj[key]);
    }
    return cloned;
}
```

### Pure Functions

A pure function depends only on its inputs and has no side effects:

```javascript
// Pure
function add(a, b) {
    return a + b;
}

// Impure — depends on external state
let taxRate = 0.1;
function calcTax(amount) {
    return amount * taxRate;
}

// Impure — mutates input
function addItem(cart, item) {
    cart.push(item);
    return cart;
}

// Pure — returns new array
function addItemPure(cart, item) {
    return [...cart, item];
}
```

Benefits of pure functions:
- Easier to test (no setup needed)
- Cacheable (same input → same output)
- Parallelizable (no shared state)
- Self-documenting (all dependencies are explicit)

## Study Cases

### Case 1: Debouncing User Input

**Scenario:** A search input fires an API call on every keystroke. Debounce limits how often the function runs.

```javascript
function debounce(fn, delay) {
    let timer = null;
    return function(...args) {
        clearTimeout(timer);
        timer = setTimeout(() => fn.apply(this, args), delay);
    };
}

const search = debounce(async (query) => {
    const results = await fetch(`/api/search?q=${query}`);
    // update UI
}, 300);

searchInput.addEventListener("input", (e) => search(e.target.value));
```

### Case 2: Throttling Scroll Events

**Scenario:** A scroll handler fires too frequently, causing performance issues.

```javascript
function throttle(fn, limit) {
    let inThrottle = false;
    return function(...args) {
        if (!inThrottle) {
            fn.apply(this, args);
            inThrottle = true;
            setTimeout(() => { inThrottle = false; }, limit);
        }
    };
}

window.addEventListener("scroll", throttle(() => {
    console.log("Scrolled at", window.scrollY);
}, 100));
```

## Examples

### Example 1: Function Composition Pipeline

```javascript
const pipeline = (...fns) => (input) =>
    fns.reduce((acc, fn) => fn(acc), input);

const trim = s => s.trim();
const capitalize = s => s[0].toUpperCase() + s.slice(1);
const exclaim = s => `${s}!`;

const greet = pipeline(trim, capitalize, exclaim);
greet("  hello world ");   // "Hello world!"
```

## Glossary

| Term | Definition |
|------|------------|
| Arrow function | A concise function syntax with lexical `this` |
| Closure | A function that retains access to its lexical scope |
| Currying | Converting a multi-argument function into nested single-argument functions |
| Debounce | Limiting function execution until a pause in calls |
| First-class function | A function that can be assigned, passed, and returned |
| Higher-order function | A function that takes or returns another function |
| Hoisting | Behaviour where declarations are moved to the top of their scope |
| IIFE | Immediately Invoked Function Expression |
| Lexical scope | Where a variable is accessible based on source code position |
| Pure function | A function with no side effects and deterministic output |
| Rest parameters | `...args` syntax collecting remaining arguments into an array |
| Scope chain | The hierarchy of nested scopes JavaScript traverses to resolve variables |
| Throttle | Limiting function execution to once per time interval |

## Quick References

- [MDN: Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions) — comprehensive guide
- [MDN: Arrow Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) — arrow function reference
- [MDN: Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) — closure guide
- [MDN: this](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/this) — this keyword reference
- [You Don't Know JS: Scope & Closures](https://github.com/getify/You-Dont-Know-JS/tree/2nd-ed/scope-closures) — in-depth book

## Next Steps

- [Objects & Prototypes](objects-and-prototypes.md) — inheritance and classes
- [Arrays & Collections](arrays-and-collections.md) — array methods and data structures
- [Promises & Async/Await](promises-async-await.md) — asynchronous programming
