# Advanced Functions

## Description

First-class functions, closures, recursion, composition, and functional patterns that unlock more expressive and reusable code.

## Prerequisites

- [Functions](../functions.md) — function declaration, parameters, scope, and pure vs impure

## Table of Contents

- [First-Class Functions](#first-class-functions)
- [Higher-Order Functions](#higher-order-functions)
- [Closures](#closures)
- [Recursion](#recursion)
- [Lambda / Anonymous Functions](#lambda--anonymous-functions)
- [Function Composition](#function-composition)
- [Methods vs Functions](#methods-vs-functions)
- [Overloading](#overloading)
- [Decorators / Wrappers](#decorators--wrappers)
- [Callbacks](#callbacks)
- [Async Basics](#async-basics)

## Content / Material

### First-Class Functions

Functions are first-class when they can be assigned, passed, and returned.

```python
greet = lambda name: f"Hello, {name}"
ops = {"add": lambda a, b: a + b}
print(ops["add"](5, 3))  # 8
```

```javascript
const greet = (name) => `Hello, ${name}`;
const ops = [x => x * 2];
console.log(ops[0](5)); // 10
```

```java
Function<Integer, Integer> square = x -> x * x;
```

```go
var square func(int) int = func(x int) int { return x * x }
```

### Higher-Order Functions

A higher-order function takes or returns a function.

```python
def apply_twice(func, value):
    return func(func(value))

print(apply_twice(lambda x: x + 1, 5))  # 7
```

```javascript
const applyTwice = (func, value) => func(func(value));
```

**Built-in `map`, `filter`:**

```python
nums = [1, 2, 3, 4, 5]
doubled = list(map(lambda x: x * 2, nums))
evens = list(filter(lambda x: x % 2 == 0, nums))
```

```javascript
const doubled = nums.map(x => x * 2);
const evens = nums.filter(x => x % 2 === 0);
```

**Function factories — returning functions:**

```python
def make_multiplier(factor):
    def multiply(x): return x * factor
    return multiply

double = make_multiplier(2)
print(double(5))  # 10
```

```javascript
const makeMultiplier = (factor) => (x) => x * factor;
```

### Closures

A closure remembers variables from its defining scope, even after that scope exits.

```python
def make_counter(start=0):
    count = start
    def counter():
        nonlocal count
        count += 1
        return count
    return counter

next_id = make_counter(1000)
print(next_id())  # 1001
```

```javascript
function makeCounter(start = 0) {
    let count = start;
    return function() { count += 1; return count; };
}
const nextId = makeCounter(1000);
console.log(nextId()); // 1001
```

Closures enable **private state** — captured variables are only accessible through the returned function.

**Loop variable capture pitfall:**

```javascript
for (var i = 0; i < 3; i++) {
    setTimeout(function() { console.log(i); }, 100);
}
// All log 3 — same i for all closures

for (let i = 0; i < 3; i++) {  // fix: let per-iteration binding
    setTimeout(function() { console.log(i); }, 100);
}
// 0, 1, 2
```

### Recursion

A function calling itself. Needs a **base case** (stops) and a **recursive case** (calls itself with modified arguments).

```python
def factorial(n):
    if n <= 1:        # base case
        return 1
    return n * factorial(n - 1)  # recursive case
```

```javascript
function factorial(n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
```

**The call stack:** each call adds a **stack frame**. Frames unwind when base case returns:

```
factorial(1) → 1
factorial(2) → 2 * 1 = 2
factorial(3) → 3 * 2 = 6
```

**Stack overflow** — recursion depth is limited (~1000 Python, ~10000 JS):

```python
def infinite(n): return infinite(n + 1)
# RecursionError: maximum recursion depth exceeded
```

**Tail recursion** — the recursive call is the last operation. Python does not optimise it.

```python
def factorial_tail(n, acc=1):
    if n <= 1: return acc
    return factorial_tail(n - 1, n * acc)  # tail call
```

Use recursion for tree/graph traversal. Use iteration for linear problems and unbounded depth.

### Lambda / Anonymous Functions

Small inline functions passed to higher-order functions.

```python
nums = [1, 2, 3, 4, 5]
doubled = list(map(lambda x: x * 2, nums))
sorted_people = sorted(people, key=lambda p: p["age"])
```

Python lambdas are limited to a single expression.

```javascript
const doubled = nums.map(x => x * 2);
const sorted = people.sort((a, b) => a.age - b.age);
const result = nums.map(x => { const s = x * x; return s + 1; });  // multi-stmt
```

```java
List<Integer> doubled = nums.stream().map(x -> x * 2).collect(Collectors.toList());
```

### Function Composition

Passing output of one function as input to the next.

```python
def compose(f, g):
    return lambda x: f(g(x))

def double(x): return x * 2
def increment(x): return x + 1

print(compose(increment, double)(5))  # 11
```

```javascript
const compose = (f, g) => (x) => f(g(x));
console.log(compose(x => x + 1, x => x * 2)(5)); // 11
```

**Data pipeline:**

```python
from functools import reduce

def compose_all(*funcs):
    return reduce(lambda f, g: lambda x: f(g(x)), funcs)

clean = compose_all(lambda t: t.strip(), lambda t: t.lower(), lambda t: t.replace("!", ""))
print(clean("  Hello, World!  "))  # "hello, world"
```

```javascript
const composeAll = (...funcs) => (x) => funcs.reduceRight((acc, f) => f(acc), x);

const clean = composeAll(t => t.replace(/[!.,?]/g, ""), t => t.toLowerCase(), t => t.trim());
console.log(clean("  Hello, World!  ")); // "hello world"
```

### Methods vs Functions

A **method** is a function attached to an object or class, receiving implicit context (`self` in Python, `this` in JS/Java).

```python
class Person:
    def __init__(self, name): self.name = name
    def greet(self): return f"Hello, I'm {self.name}"

p = Person("Alice")
print(p.greet())  # p becomes self
```

```javascript
class Person {
    constructor(name) { this.name = name; }
    greet() { return `Hello, I'm ${this.name}`; }
}

p = new Person("Alice");
console.log(p.greet());

const detached = p.greet;
detached(); // "Hello, undefined" — this lost

const bound = p.greet.bind(p);
bound(); // "Hello, Alice"
```

Arrow functions inherit `this` from enclosing scope, making them safe as callbacks inside methods.

### Overloading

Multiple functions with same name but different parameters. Python and JS lack native support.

```python
def add(a, b, c=None):
    if c is not None: return a + b + c
    return a + b
```

```javascript
function add(a, b, c) {
    if (c !== undefined) return a + b + c;
    return a + b;
}
```

**Python `singledispatch`:**

```python
from functools import singledispatch

@singledispatch
def process(value): raise TypeError(f"Unsupported: {type(value)}")

@process.register(int)
def _(value): return value * 2

@process.register(str)
def _(value): return value.upper()
```

**Java — true overloading at compile time:**

```java
public static int add(int a, int b) { return a + b; }
public static double add(double a, double b) { return a + b; }
```

Go does not support overloading — every function needs a unique name.

### Decorators / Wrappers

A decorator wraps a function to extend its behaviour (Python syntax sugar).

```python
def log_calls(func):
    def wrapper(*args, **kwargs):
        print(f"Calling {func.__name__}")
        result = func(*args, **kwargs)
        print(f"Returned {result}")
        return result
    return wrapper

@log_calls
def add(a, b): return a + b
add(3, 5)
# Calling add
# Returned 8
```

`@log_calls` = `add = log_calls(add)`.

**Timing and memoisation:**

```python
import time

def timer(func):
    def wrapper(*args, **kwargs):
        start = time.perf_counter()
        result = func(*args, **kwargs)
        print(f"{func.__name__}: {time.perf_counter() - start:.4f}s")
        return result
    return wrapper

from functools import lru_cache

@lru_cache(maxsize=128)
def fibonacci(n):
    if n <= 1: return n
    return fibonacci(n - 1) + fibonacci(n - 2)
```

**Decorators with arguments:**

```python
def repeat(times):
    def decorator(func):
        def wrapper(*args, **kwargs):
            for _ in range(times): func(*args, **kwargs)
        return wrapper
    return decorator

@repeat(times=3)
def say_hello(name): print(f"Hello, {name}")
```

```javascript
// Same pattern without syntax sugar
function logCalls(func) {
    return function(...args) {
        console.log(`Calling ${func.name}`);
        return func(...args);
    };
}
const add = logCalls((a, b) => a + b);
```

### Callbacks

A function passed to another for later execution.

```python
def process(items, callback):
    for item in items: print(f"Result: {callback(item)}")

process([1, 2, 3], lambda x: x * 2)
```

```javascript
function process(items, callback) {
    items.forEach(item => console.log(`Result: ${callback(item)}`));
}

button.addEventListener("click", (e) => console.log("Clicked at", e.clientX));
```

**Callback hell:**

```javascript
getUser(id, (err, user) => {
    getPosts(user.id, (err, posts) => {
        getComments(posts[0].id, (err, comments) => {
            console.log(comments);
        });
    });
});
```

Promises and async/await solve this.

### Async Basics

Programs wait for I/O. Async programming lets other code run while waiting.

**Promises (JavaScript):**

```javascript
function fetchUser(id) {
    return new Promise((resolve, reject) => {
        setTimeout(() => {
            if (id < 0) reject(new Error("Invalid id"));
            else resolve({ id, name: "Alice" });
        }, 1000);
    });
}

fetchUser(42)
    .then(user => console.log(user.name))
    .catch(err => console.error(err));
```

States: **pending** → **fulfilled** (value) or **rejected** (error). Chaining:

```javascript
fetchUser(id)
    .then(user => fetchPosts(user.id))
    .then(posts => fetchComments(posts[0].id))
    .catch(err => console.error(err));
```

**Async/await** — syntactic sugar over promises:

```python
import asyncio

async def fetch_user(id):
    await asyncio.sleep(1)
    if id < 0: raise ValueError("Invalid id")
    return {"id": id, "name": "Alice"}

async def main():
    user = await fetch_user(42)
    print(user["name"])

asyncio.run(main())
```

```javascript
async function fetchUser(id) {
    await new Promise(r => setTimeout(r, 1000));
    if (id < 0) throw new Error("Invalid id");
    return { id, name: "Alice" };
}

(async () => {
    const user = await fetchUser(42);
    console.log(user.name);
})();
```

`async` functions return a Promise/coroutine. `await` pauses without blocking. Error handling uses `try`/`catch`.

## Study Cases

### Case 1: Closure capturing a loop variable incorrectly

```javascript
function setupButtons() {
    for (var i = 0; i < 3; i++) {
        var btn = document.createElement("button");
        btn.textContent = "Button " + i;
        btn.addEventListener("click", function() {
            alert("You clicked button " + i);
        });
        document.body.appendChild(btn);
    }
}
```

Every button alerts "3" — all closures share the same `i`. **Fix:** `let` or IIFE.

```javascript
for (let i = 0; i < 3; i++) { /* per-iteration binding */ }

for (var i = 0; i < 3; i++) {
    (function(j) {
        btn.addEventListener("click", function() { alert(j); });
    })(i);
}
```

### Case 2: Loop replaced with higher-order functions

```python
orders = [
    {"id": 1, "items": [{"price": 10}, {"price": 20}], "status": "pending"},
    {"id": 2, "items": [{"price": 5}], "status": "shipped"},
    {"id": 3, "items": [{"price": 15}, {"price": 25}, {"price": 30}], "status": "pending"},
]

# Imperative
result = []
for o in orders:
    if o["status"] == "shipped": continue
    total = sum(i["price"] for i in o["items"])
    if total > 20: result.append({"id": o["id"], "total": total})
```

Refactored:

```javascript
const result = orders
    .filter(o => o.status === "pending")
    .map(o => ({ id: o.id, total: o.items.reduce((s, i) => s + i.price, 0) }))
    .filter(o => o.total > 20);
```

### Case 3: Recursive vs iterative directory traversal

Recursive — clean but risks stack overflow for deep trees:

```python
import os

def list_files_recursive(path):
    entries = []
    for entry in os.listdir(path):
        full = os.path.join(path, entry)
        if os.path.isdir(full):
            entries.extend(list_files_recursive(full))
        else:
            entries.append(full)
    return entries
```

Iterative with explicit stack — no recursion limit:

```python
def list_files_iterative(path):
    entries = []
    stack = [path]
    while stack:
        current = stack.pop()
        for entry in os.listdir(current):
            full = os.path.join(current, entry)
            if os.path.isdir(full): stack.append(full)
            else: entries.append(full)
    return entries
```

### Case 4: Decorator for rate-limiting an API call

A decorator that throttles repeated calls within a time window:

```python
import time

def rate_limit(period=1.0):
    last_called = [0.0]
    def decorator(func):
        def wrapper(*args, **kwargs):
            elapsed = time.time() - last_called[0]
            if elapsed < period:
                print(f"Rate limited. Wait {period - elapsed:.2f}s")
                return None
            last_called[0] = time.time()
            return func(*args, **kwargs)
        return wrapper
    return decorator

@rate_limit(period=0.5)
def fetch_data():
    print("Data fetched")

for _ in range(5):
    fetch_data()
    time.sleep(0.3)
```

The decorator closes over `last_called` and enforces a minimum interval between invocations.

## Examples

### Call Stack Diagram

When `factorial(3)` executes:

```
factorial(1) → 1
factorial(2) → 2 * 1 = 2
factorial(3) → 3 * 2 = 6
```

Each frame holds local variables and return address.

### Closure Variable Capture Visualised

```python
def make_actions():
    actions = []
    for i in range(3):
        actions.append(lambda: print(i))
    return actions

for a in make_actions(): a()
# 2, 2, 2 — all see final i

def make_actions_fixed():
    actions = []
    for i in range(3):
        actions.append(lambda i=i: print(i))  # capture by value
    return actions

for a in make_actions_fixed(): a()
# 0, 1, 2
```

### Composition Pipeline Visualisation

```python
def compose(f, g):
    return lambda x: f(g(x))

def double(x): return x * 2
def increment(x): return x + 1

f = compose(increment, double)
# f(5) = increment(double(5)) = increment(10) = 11

g = compose(double, increment)
# g(5) = double(increment(5)) = double(6) = 12
```

Order matters in composition — `f ∘ g` applies `g` first, then `f`.

### Async Error Handling

```javascript
async function riskyOperation() {
    if (Math.random() < 0.5) throw new Error("Random failure");
    return "Success";
}

async function run() {
    try {
        const result = await riskyOperation();
        console.log(result);
    } catch (err) {
        console.error("Operation failed:", err.message);
    }
}
```

```python
import asyncio
import random

async def risky_operation():
    if random.random() < 0.5:
        raise ValueError("Random failure")
    return "Success"

async def run():
    try:
        result = await risky_operation()
        print(result)
    except ValueError as e:
        print(f"Operation failed: {e}")
```

Always use `try`/`catch` (or `except`) around `await` expressions that can fail. Unhandled promise rejections crash the process in Node.js.

## Glossary

| Term | Definition |
|------|------------|
| First-class function | Function assignable, passable, returnable like any value |
| Higher-order function | Function that takes or returns another function |
| Closure | Function retaining access to its defining scope's variables |
| Lambda | Anonymous function defined inline |
| Anonymous function | Function without a name |
| Recursion | A function calling itself |
| Base case | Condition stopping further recursive calls |
| Recursive case | Part where the function calls itself |
| Stack overflow | Exceeding the maximum call stack depth |
| Call stack | Data structure tracking pending function calls |
| Stack frame | Entry on the call stack with local variables and return address |
| Tail recursion | Recursive call as the last operation in a function |
| Tail call optimisation | Compiler optimisation reusing the current stack frame for a tail call |
| Composition | Passing output of one function as input to another |
| Pipeline | Sequence of composed functions transforming data through stages |
| Method | Function attached to an object or class |
| Overloading | Multiple functions sharing a name but differing in parameters |
| Singledispatch | Python's single-dispatch generic function mechanism |
| Decorator | Python function wrapping another to extend behaviour |
| Wrapper | Function adding behaviour around another |
| Callback | Function passed to another for later execution |
| Memoisation | Caching function results based on input |
| Currying | Transforming a multi-argument function into a sequence of single-argument functions |
| Promise | Object representing eventual result of an async operation |
| Async / await | Syntactic sugar over promises for readable async code |
| Coroutine | A function declared with `async` that can be suspended and resumed |
| Pending | Initial state of a Promise before resolution |
| Fulfilled | State of a successfully resolved Promise |
| Rejected | State of a Promise that encountered an error |
| Temporal dead zone | Period between entering scope and variable initialisation where access throws |
| Function factory | Function that creates and returns new functions |
| Arity | Number of parameters a function expects (advanced usage: variadic generics) |
| Variadic | Function accepting a variable number of arguments |
| IIFE | Immediately Invoked Function Expression — function defined and called in one step |

## Quick References

- [MDN Arrow Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions) — arrow function syntax and `this` behaviour
- [Python `functools` Module](https://docs.python.org/3/library/functools.html) — standard library tools for higher-order functions and caching
- [Recursion (CS Primer)](https://www.cs.cmu.edu/~adamchik/15-121/lectures/Recursions/recursions.html) — recursion with call stack visualisation
- [MDN Closures Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures) — closures explained with interactive examples
- [Python Decorators Guide](https://realpython.com/primer-on-python-decorators/) — comprehensive tutorial on decorators from basics to advanced
- [MDN Using Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises) — promise chaining, error handling, and composition
- [JavaScript Async/Await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) — async function reference with examples
- [Python `asyncio` Documentation](https://docs.python.org/3/library/asyncio.html) — official async/await documentation for Python

## Next Steps

- [Higher-Order Array Methods](../collections.md#higher-order-array-methods) — applying functional patterns to collections
- [Functional Programming Concepts](../functional-programming/index.md) — deeper dive into FP paradigms
- [Error Handling](../error-handling.md) — robust patterns for handling failures in callbacks and async code
