# Functions

## Description

Functions are the primary mechanism for organising, reusing, and abstracting behaviour in code. Every program you write will use them — understanding them deeply separates novice from professional.

## Prerequisites

- [Control Flow](../control-flow.md) — conditionals and loops for writing function bodies

## Table of Contents

- [What Is a Function?](#what-is-a-function)
- [Function Declaration](#function-declaration)
- [Parameters and Arguments](#parameters-and-arguments)
- [Return Values](#return-values)
- [Scope Rules](#scope-rules)
- [Pure Functions vs Impure Functions](#pure-functions-vs-impure-functions)

## Content / Material

### What Is a Function?

A function is a named, reusable block of code that takes input, performs a computation, and produces output.

```python
def add(a, b):
    return a + b
result = add(3, 5)  # 8
```

```javascript
function add(a, b) { return a + b; }
const result = add(3, 5); // 8
```

Every function has a **signature** (name, parameters, return type) and a **body** (the code that runs). Calling a function is **invocation**. Functions provide **abstraction** (use `send_email` without knowing how mail works), **reusability** (write once, use anywhere), and **testability** (clear inputs and outputs).

### Function Declaration

**Python — `def`:**

```python
def greet(name):
    return f"Hello, {name}"

def add(a: int, b: int) -> int:  # type annotations (not enforced at runtime)
    return a + b
```

**JavaScript — `function` keyword and arrow functions (ES6+):**

```javascript
function greet(name) { return `Hello, ${name}`; }
const add = (a, b) => a + b;    // arrow, implicit return
const double = x => x * 2;      // single param, no parens
```

Arrow functions lack their own `this` and cannot be constructors.

**Statically-typed comparisons:**

```java
public static String greet(String name) { return "Hello, " + name; }
```

```go
func greet(name string) string { return "Hello, " + name }
```

```rust
fn greet(name: &str) -> String { format!("Hello, {}", name) }
```

### Parameters and Arguments

Parameters are variables in the definition. Arguments are values passed when calling.

**Positional** — matched by order:

```python
def subtract(a, b): return a - b
print(subtract(10, 3))  # 7, order matters
```

**Keyword (Python):**

```python
def describe(name, age, city):
    return f"{name}, {age}, from {city}"

describe("Alice", 30, "London")                   # positional
describe(city="London", age=30, name="Alice")     # keyword, any order
```

```javascript
function describe({ name, age, city }) {
    return `${name}, ${age}, from ${city}`;
}
describe({ city: "London", age: 30, name: "Alice" });
```

**Default values:**

```python
def greet(name, greeting="Hello"): return f"{greeting}, {name}"
```

```javascript
function greet(name, greeting = "Hello") { return `${greeting}, ${name}`; }
```

Mutable defaults in Python are evaluated once at definition:

```python
def add_item(item, items=[]):
    items.append(item)
    return items  # bug: second call sees first call's list

def add_item(item, items=None):  # fix
    if items is None: items = []
    items.append(item)
    return items
```

**Rest / `*args`:**

```python
def sum_all(*args):
    total = 0
    for n in args: total += n
    return total
```

```javascript
function sumAll(...args) { return args.reduce((t, n) => t + n, 0); }
```

**`**kwargs` (Python):**

```python
def print_config(**kwargs):
    for k, v in kwargs.items(): print(f"{k} = {v}")

print_config(host="localhost", port=8080)
```

**Spread / unpacking:**

```python
def multiply(a, b, c): return a * b * c
print(multiply(*[2, 3, 5]))  # 30
```

```javascript
function multiply(a, b, c) { return a * b * c; }
console.log(multiply(...[2, 3, 5])); // 30
```

**Arity** is the number of parameters. **Variadic** functions accept any number.

### Return Values

Every function returns something — even without `return`.

```python
def square(x): return x * x
result = square(5)  # 25
def noop(): pass
print(noop())  # None
```

```javascript
function square(x) { return x * x; }
function noop() {}
console.log(noop()); // undefined
```

**Multiple returns** — Python returns a tuple, automatically unpacked:

```python
def divide_and_remainder(a, b):
    return a // b, a % b  # tuple
q, r = divide_and_remainder(17, 5)
```

```javascript
function divideAndRemainder(a, b) {
    return [Math.floor(a / b), a % b];
}
const [q, r] = divideAndRemainder(17, 5);
```

**Early return** for edge cases:

```python
def sqrt(value):
    if value < 0: return None
    return value ** 0.5
```

Statically-typed languages enforce return types at compile time:

```java
public static int square(int x) { return x * x; }
```

### Scope Rules

Scope determines where a variable is accessible. Languages use **lexical scoping** — code structure determines visibility.

```python
x = 10  # global
def my_func():
    y = 20  # local
    print(x)  # 10 — global readable
```

**LEGB rule (Python):** variable lookup: **L**ocal → **E**nclosing → **G**lobal → **B**uilt-in.

```python
x = "global"
def outer():
    x = "outer"
    def inner():
        x = "inner"
        print(x)  # "inner"
    inner()
    print(x)  # "outer"
outer()
print(x)  # "global"
```

**Modifying outer scope:**

```python
counter = 0
def increment():
    global counter  # required for globals
    counter += 1

def make_counter():
    count = 0
    def increment():
        nonlocal count  # modifies enclosing scope
        count += 1
        return count
    return increment
```

**Hoisting (JavaScript):** `function` declarations are fully hoisted. `var` is hoisted as `undefined`. `let`/`const` are hoisted but not initialised — accessing before declaration throws `ReferenceError` (temporal dead zone).

```javascript
console.log(greet("Alice"));  // "Hello, Alice" — hoisted
function greet(name) { return `Hello, ${name}`; }

console.log(x);  // undefined
var x = 5;
```

### Pure Functions vs Impure Functions

A **pure function** is deterministic (same input always same output) and has no side effects.

```python
# Pure
def add(a, b): return a + b

# Impure — modifies global state
total = 0
def add_to_total(x):
    global total
    total += x

# Impure — I/O
def greet(name): print(f"Hello, {name}")
```

```javascript
// Pure
const add = (a, b) => a + b;

// Impure — reads mutable external state
let taxRate = 0.2;
const calcTax = (a) => a * taxRate;  // changes if taxRate changes

// Impure — mutates input
const applyDiscount = (prices) => { prices.forEach((_, i) => prices[i] *= 0.9); };
```

Pure functions are easy to test, cache (memoisation), and parallelise. Isolate impurity at boundaries.

## Glossary

| Term | Definition |
|------|------------|
| Function | Named, reusable block of code taking input and producing output |
| Parameter | Variable in a function definition receiving an argument |
| Argument | Value passed to a function when calling |
| Return value | Value a function produces and sends back |
| Signature | Name, parameter list, and return type |
| Body | Code inside that executes when called |
| Call | Executing a function (also invocation) |
| Invocation | Another term for calling a function |
| Scope | Region where a variable or function is accessible |
| Pure function | Deterministic function with no side effects |
| Side effect | State change outside the function's local scope |
| Default parameter | Parameter with a fallback value when no argument is passed |
| Keyword argument | Argument identified by parameter name, not position |
| Positional argument | Argument matched by position in the parameter list |
| Rest parameter | Parameter capturing remaining arguments into an array or tuple |
| Spread | Unpacking an iterable into individual arguments |
| Arity | Number of parameters a function expects |
| Variadic | Function accepting a variable number of arguments |
| Lexical scoping | Variable visibility determined by code structure at write time |
| LEGB | Python's lookup order: Local, Enclosing, Global, Built-in |
| Hoisting | JavaScript mechanism moving declarations to top of their scope |
| Temporal dead zone | Period between entering scope and variable initialisation where access throws ReferenceError |
| Guard clause | Early return that handles an edge case, reducing nesting |
| Sentinel value | Special value used to terminate or guard logic (e.g., `None`) |
| Mutable default | Default parameter value that is a mutable object, evaluated once at definition time |

## Quick References

- [Python Functions (Official Docs)](https://docs.python.org/3/tutorial/controlflow.html#defining-functions) — Python's official tutorial on function definition, arguments, and scope
- [MDN Functions Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Functions) — comprehensive JavaScript function documentation with examples
- [Real Python — Defining Your Own Python Function](https://realpython.com/defining-your-own-python-function/) — thorough walkthrough with best practices
- [JavaScript Function Parameters](https://www.w3schools.com/js/js_function_parameters.asp) — parameters, arguments, and defaults in JavaScript
- [Python Scope & LEGB Rule](https://realpython.com/python-scope-legb-rule-same-name/) — deep dive into Python's scoping rules

## Next Steps

- [Advanced Functions](../advanced-functions.md) — first-class functions, closures, recursion, and functional patterns
- [Collections](../collections.md) — lists, sets, dictionaries, and how functions interact with data structures
