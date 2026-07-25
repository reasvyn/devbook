# Control Flow

## Description

Control flow determines the order in which statements are executed. Without it, a program runs top-to-bottom and stops. Conditionals, loops, and function calls give programs the ability to make decisions, repeat work, and respond dynamically to data.

## Prerequisites

- [Operators & Expressions](../operators-and-expressions.md) — comparison and logical operators are used in every conditional and loop condition

## Table of Contents

- [Three Fundamental Control Structures](#three-fundamental-control-structures)
- [Conditionals (Selection)](#conditionals-selection)
- [Loops (Iteration)](#loops-iteration)
- [Pattern Matching](#pattern-matching)
- [Error Handling as Control Flow](#error-handling-as-control-flow)
- [Control Flow in Functional Style](#control-flow-in-functional-style)
- [Common Control Flow Patterns](#common-control-flow-patterns)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Three Fundamental Control Structures

Structured programming rests on three building blocks. Every program is a combination of them.

**Sequence** — execute statements one after another. This is the default.

```python
name = "Alice"
age = 30
greeting = f"{name} is {age} years old"
print(greeting)
```

```javascript
const name = "Alice";
const age = 30;
const greeting = `${name} is ${age} years old`;
console.log(greeting);
```

**Selection** — choose a path based on a condition. Implemented by `if`/`else`, `switch`, ternary expressions, and pattern matching.

**Iteration** — repeat a block while a condition holds. Implemented by `while`, `for`, and `for`-`in`/`for`-`of` loops, as well as recursion.

### Conditionals (Selection)

#### If / Else If / Else

```python
temperature = 30
if temperature > 35:
    print("Too hot")
elif temperature > 20:
    print("Pleasant")
else:
    print("Cool")
```

```javascript
const temperature = 30;
if (temperature > 35) {
    console.log("Too hot");
} else if (temperature > 20) {
    console.log("Pleasant");
} else {
    console.log("Cool");
}
```

Python uses `elif` and indentation. Java, Go, and JavaScript use `else if` and braces.

#### Nested Conditionals

Every level of nesting adds a mental stack frame the reader must track. Keep depth at most two.

```python
# Hard to read — 3 levels deep
if user.is_authenticated:
    if user.has_permission("admin"):
        if resource.is_available:
            grant_access()
        else:
            show_unavailable()
    else:
        show_forbidden()
else:
    show_login()
```

Refactor with guard clauses or extracted functions when you hit level three.

#### Switch / Case (JavaScript)

JavaScript's `switch` uses strict equality and requires `break` to prevent fall-through:

```javascript
function getDayName(dayNumber) {
    switch (dayNumber) {
        case 0: return "Sunday";
        case 1: return "Monday";
        case 2: return "Tuesday";
        case 3: return "Wednesday";
        case 4: return "Thursday";
        case 5: return "Friday";
        case 6: return "Saturday";
        default: return "Invalid day";
    }
}
```

Fall-through can be intentional when multiple cases share a handler. Go's `switch` does not fall through by default.

#### Match / Case (Python 3.10+)

Python's `match` supports structural pattern matching — branching on the shape and type of data, not just value equality:

```python
def describe_value(value):
    match value:
        case 0:
            return "Zero"
        case 1 | 2 | 3:
            return "Small"
        case int() as n if n < 100:
            return f"Integer under 100: {n}"
        case str() as s:
            return f"String: {s}"
        case (x, y):
            return f"Pair: ({x}, {y})"
        case _:
            return "Something else"
```

`|` combines cases, `if` guards add extra conditions, `int() as n` checks the type and binds the value, `(x, y)` destructures a tuple, and `_` is the wildcard default. Rust's `match` is similar but exhaustive.

#### Ternary Conditional Expression

A ternary produces a value from a binary choice:

```python
status = "adult" if age >= 18 else "minor"
```

```javascript
const status = age >= 18 ? "adult" : "minor";
```

```java
String status = age >= 18 ? "adult" : "minor";
```

Go omits the ternary — it uses `if`/`else` assignment instead. Avoid nested ternaries.

#### Truthy and Falsy Evaluation

Conditions do not require booleans. Each value is classified as truthy or falsy.

**Python falsy values:** `0`, `0.0`, `""`, `[]`, `{}`, `None`, `False`, empty collections.

**JavaScript falsy values:** `0`, `-0`, `0n`, `""`, `null`, `undefined`, `NaN`, `false`.

Critical difference: empty arrays `[]` and empty objects `{}` are **truthy in JavaScript** but **falsy in Python**.

```javascript
const items = [];
if (items) { } // runs — empty array is truthy
if (items.length === 0) { } // check actual emptiness
```

```python
items = []
if not items: pass  # runs — empty list is falsy
```

### Loops (Iteration)

#### While Loops

Repeats while the condition is truthy. Checked before each iteration.

```python
count = 0
while count < 5:
    print(count)
    count += 1
```

```javascript
let count = 0;
while (count < 5) {
    console.log(count);
    count++;
}
```

**Do-while** (JavaScript, Java, Go, Rust) guarantees at least one execution:

```javascript
let i = 0;
do {
    console.log(i);
    i++;
} while (i < 5);
```

Python has no `do`/`while` — use `while True` with a conditional `break`.

#### For Loops

JavaScript has a three-clause `for` loop. Python has a foreach-style `for` over iterables.

**JavaScript — count-controlled:**

```javascript
for (let i = 0; i < 5; i++) {
    console.log(i);
}
```

Clauses: initialisation (`let i = 0`), condition (`i < 5`), increment (`i++`).

**Python — iteration-based:**

```python
for i in range(5):
    print(i)
```

`range(5)` produces an iterable. Python's `for` pulls values from an iterator.

**Go** uses one `for` keyword for all loop forms (count-controlled and `for _, n := range items`).

#### For...Of vs For...In (JavaScript)

`for...of` iterates over **values** of an iterable. `for...in` iterates over enumerable **property keys** (on arrays, that yields string indices). Use `for...of` for arrays, `for...in` for plain objects. Python's dict iteration is equivalent to `for...in`.

#### Break and Continue

`break` terminates the innermost loop. `continue` skips to the next iteration.

```python
for i in range(10):
    if i == 3: continue
    if i == 7: break
    print(i)  # 0, 1, 2, 4, 5, 6
```

To break from an outer loop, use a labelled break (JS/Java/Go) or a flag/return (Python).

#### Else Clause on Loops (Python)

Runs only if the loop completed normally (no `break`). Useful for search loops.

```python
for n in range(2, 10):
    for d in range(2, n):
        if n % d == 0:
            break
    else:
        print(f"{n} is prime")
```

JS/Java/Go use a flag variable instead.

#### Infinite Loops

Common causes: forgetting to increment, wrong condition direction, floating-point sentinel never exactly reached. **Prevention:** use `for` loops when the count is known, verify convergence, add a max-iteration safety guard.

#### Nested Loops

A loop inside another runs `n * m` iterations — O(n^2) when both iterate over the same data. At n=1,000 that is 1,000,000 iterations. Consider hash maps or sets for better complexity, but do not optimise prematurely.

### Pattern Matching

Python 3.10+ `match`/`case` supports destructuring and guards:

```python
def process_command(command):
    match command:
        case "quit" | "exit" | "q":
            return "Quitting"
        case ["move", x, y]:
            return f"Moving to ({x}, {y})"
        case {"action": "login", "user": user}:
            return f"Logging in {user}"
        case _:
            return "Unknown command"
```

Destructuring in match unpacks compound values:

```python
point = (3, 4)
match point:
    case (0, 0): print("Origin")
    case (x, 0): print(f"On x-axis at {x}")
    case (x, y): print(f"At ({x}, {y})")
```

Variables are bound to parts of the matched value. Rust's `match` is exhaustive — the compiler checks all possibilities.

### Error Handling as Control Flow

Exceptions abort normal sequence and jump to the nearest handler:

```python
try:
    data = fetch_from_api()
    result = process(data)
except ConnectionError:
    retry()
except ValidationError as e:
    log_error(e)
    raise
finally:
    cleanup_connection()
```

```javascript
try {
    const data = await fetchFromApi();
    const result = process(data);
} catch (error) {
    if (error instanceof ConnectionError) retry();
    else { logError(error); throw; }
} finally {
    cleanupConnection();
}
```

`finally` always executes — use it for cleanup (close files, release locks).

Python's `try`/`except`/`else`/`finally` separates success-only code:

```python
try:
    config = load_config()
except FileNotFoundError:
    config = DEFAULT_CONFIG
else:
    print("Config loaded")  # only if no exception
finally:
    print("Cleanup done")
```

Exceptions should be exceptional — do not use them for routine control flow.

### Control Flow in Functional Style

#### Recursion

A function calling itself, breaking a problem into smaller instances:

```python
def factorial(n):
    if n <= 1: return 1          # base case
    return n * factorial(n - 1)  # recursive case
```

```javascript
function factorial(n) {
    if (n <= 1) return 1;
    return n * factorial(n - 1);
}
```

Every recursive function needs a **base case** and a **recursive case**. Recursion is natural for tree traversal and divide-and-conquer. The recursive Fibonacci is elegant but O(2^n):

```python
def fib(n):
    if n <= 1: return n
    return fib(n - 1) + fib(n - 2)
```

Prefer iteration for linear problems. Python and JavaScript have recursion depth limits (Python defaults to 1000). **Tail recursion** (recursive call as the last operation) is not optimised by Python or most JS engines.

#### Map, Filter as Alternatives

`map` transforms every element. `filter` selects by a predicate. They replace manual loops.

```python
# Imperative
squares = []
for n in numbers:
    squares.append(n ** 2)

# Comprehension (Pythonic)
squares = [n ** 2 for n in numbers]
```

```javascript
const squares = numbers.map(n => n ** 2);
const evens = numbers.filter(n => n % 2 === 0);
```

Chaining expresses pipelines clearly:

```javascript
const adultNames = users
    .filter(u => u.age >= 18)
    .map(u => u.name);
```

```python
adult_names = [u["name"] for u in users if u["age"] >= 18]
```

Use these for direct mapping or filtering. For complex logic, a full loop may be clearer.

### Common Control Flow Patterns

#### Early Return / Guard Clauses

Check preconditions at the start and return immediately, flattening nested conditionals:

```python
# Before — nested
def process_order(order):
    if order:
        if order.is_valid():
            if order.payment_processed:
                ship_order(order)
            else: raise ValueError("Payment not processed")
        else: raise ValueError("Invalid order")
    else: raise ValueError("No order")

# After — guard clauses
def process_order(order):
    if not order: raise ValueError("No order")
    if not order.is_valid(): raise ValueError("Invalid order")
    if not order.payment_processed: raise ValueError("Payment not processed")
    ship_order(order)  # happy path
```

```javascript
// Before
function processOrder(order) {
    if (order) {
        if (order.isValid()) {
            if (order.paymentProcessed) shipOrder(order);
            else throw new Error("Payment not processed");
        } else throw new Error("Invalid order");
    } else throw new Error("No order");
}

// After
function processOrder(order) {
    if (!order) throw new Error("No order");
    if (!order.isValid()) throw new Error("Invalid order");
    if (!order.paymentProcessed) throw new Error("Payment not processed");
    shipOrder(order);
}
```

#### State Machine with Conditionals

Model a system with finite states and transitions using conditionals:

```python
def run_traffic_light():
    state = "green"
    while True:
        input("Press Enter: ")
        if state == "green":
            print("Go"); state = "yellow"
        elif state == "yellow":
            print("Caution"); state = "red"
        elif state == "red":
            print("Stop"); state = "green"
```

For complex machines, a lookup table is cleaner:

```python
transitions = {
    "green": {"next": "yellow", "msg": "Go"},
    "yellow": {"next": "red", "msg": "Caution"},
    "red": {"next": "green", "msg": "Stop"},
}
```

#### Loop-and-a-Half Pattern

When the exit condition belongs in the middle, use `while True` + `break`:

```python
while True:
    user_input = input("Enter a positive number: ")
    if user_input.isdigit() and int(user_input) > 0:
        break
    print("Invalid, try again")
```

```javascript
while (true) {
    const input = prompt("Enter a positive number:");
    if (input !== null && !isNaN(input) && Number(input) > 0) break;
    console.log("Invalid, try again");
}
```

#### Iteration with Index vs Value

```python
for item in items: pass          # value only
for i in range(len(items)): pass  # index only
for i, item in enumerate(items):  # both
    print(i, item)
```

```javascript
for (const item of items) {}                    # value only
for (let i = 0; i < items.length; i++) {}       # index only
for (const [i, item] of items.entries()) {}     # both
```

Go uses `for i, item := range items` for both; discard index with `_`.

## Glossary

| Term | Definition |
|------|------------|
| Control flow | The order in which statements, instructions, or function calls are executed |
| Sequence | Statements executed one after another in order |
| Selection | Choosing between multiple paths based on a condition |
| Iteration | Repeating a block of code multiple times |
| Conditional | A statement that executes code only if a boolean condition is truthy |
| Branch | One possible path through conditional logic |
| Loop | A construct that repeatedly executes a block of code |
| Recursion | A function calling itself to solve a smaller instance of the same problem |
| Guard clause | An early return when a precondition is not met |
| Early return | Exiting a function before its end, typically for an edge case |
| Short-circuit | Logical operators stop evaluating once the result is determined |
| Switch | Multi-branch conditional comparing a value against cases (JS, Java, Go) |
| Pattern match | Matching a value against structural patterns with destructuring and guards |
| Infinite loop | A loop whose termination condition is never reached |
| Off-by-one | A logic error where a loop iterates one time too many or too few |
| Fencepost | Off-by-one error from misapplying boundary conditions |
| Sentinel | A special value signalling the end of a sequence or loop |
| Accumulator | A variable accumulating a result across iterations |
| State machine | A model with finite states and transitions between them |
| Structured programming | Paradigm based on sequence, selection, and iteration |
| Nesting depth | Levels of indentation from nested control structures |
| Fall-through | In switch, progression from one case to the next without break |
| Destructuring | Unpacking a compound value into individual components |
| Base case | The terminating condition in a recursive function |
| Stack overflow | Error when the call stack exceeds its allocated size |
| Tail recursion | Recursive call as the last operation, eligible for optimisation |
| Happy path | The code path when no errors or edge cases occur |
| Loop-and-a-half | Pattern where loop exit is in the middle of the body |

## Quick References

- [MDN: Control Flow and Error Handling](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Control_flow_and_error_handling) — JavaScript guide to conditionals, loops, switch, try/catch
- [Python: More Control Flow Tools](https://docs.python.org/3/tutorial/controlflow.html) — Python's official tutorial on if, for, while, range, break, continue, and match
- [The Go Programming Language: For Statements](https://go.dev/doc/effective_go#for) — effective Go's unified for loop and range
- [Rust: Match Expressions](https://doc.rust-lang.org/book/ch06-02-match.html) — Rust's exhaustive pattern matching
- [Edsger Dijkstra: Go To Statement Considered Harmful](https://homepages.cwi.nl/~storm/teaching/reader/Dijkstra68.pdf) — the 1968 paper that established structured programming

## Next Steps

- [Functions](../functions.md) — encapsulating control flow into reusable units
- [Collections](../collections.md) — data structures that you iterate over
- [Error Handling](../error-handling.md) — exceptions as a specialised control flow mechanism
