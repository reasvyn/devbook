# Programming Fundamentals

## Description

The essential concepts that form the foundation of all programming — understanding these makes learning any language or paradigm significantly easier.

## Prerequisites

- [What Is Programming?](../../intro/what-is-programming.md) — basic understanding of what programming is

## Table of Contents

- [Why Fundamentals Matter](#why-fundamentals-matter)
- [Data and Code](#data-and-code)
- [Values, Types, and Operations](#values-types-and-operations)
- [Expressions and Statements](#expressions-and-statements)
- [Variables and Mutation](#variables-and-mutation)
- [Control Flow Primitives](#control-flow-primitives)
- [Functions as Abstractions](#functions-as-abstractions)
- [Composition](#composition)
- [State and Side Effects](#state-and-side-effects)
- [The Evaluation Model](#the-evaluation-model)
- [Abstraction Layers](#abstraction-layers)
- [Common Patterns](#common-patterns)
- [Writing Readable Code](#writing-readable-code)
- [The Debugging Mindset](#the-debugging-mindset)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Why Fundamentals Matter

Programming languages come and go. Every few years a new language gains popularity, and developers who tied their identity to the previous one find themselves struggling to adapt. Fundamentals are the concepts that transfer across all languages.

When you understand what a variable is at a conceptual level, you can work with variables in Python, JavaScript, Java, Go, Rust, or any other language. The syntax changes, but the concept stays the same. This is what makes fundamentals valuable.

Language features are implementations of fundamental concepts:
- Python's list comprehensions are a specific syntax for map/filter
- JavaScript's promises are an implementation of async composition
- Rust's ownership system is a compile-time approach to memory management
- Go's goroutines are a language-level concurrency primitive

Without understanding the underlying concepts, each new feature looks like magic. With fundamentals, you recognize the pattern behind the syntax.

### Data and Code

Every program consists of two things: data and code. Data is the information the program works with. Code is the instructions that transform the data.

```
Data (input) → Code (transformation) → Data (output)
```

This model applies at every level:
- A function takes input data and returns output data
- A module takes data from other modules and produces new data
- A program takes user input and system input and produces output

Data can be simple (a single number) or complex (a graph of interconnected objects). Code can be a single expression or a million-line codebase. The fundamental relationship stays the same.

```python
# Data: a list of numbers
prices = [10.99, 24.99, 5.99, 39.99]

# Code: transformation that computes total
total = sum(prices)
```

### Values, Types, and Operations

A value is a concrete piece of data — the number `42`, the string `"hello"`, the boolean `true`. Every value has a type that determines what operations can be performed on it.

The fundamental types across most languages:

| Type | Examples | Common Operations |
|------|----------|-------------------|
| Integer | `42`, `-3`, `0` | +, -, *, /, %, comparison |
| Float | `3.14`, `-0.5` | +, -, *, /, comparison |
| String | `"hello"`, `"42"` | concatenation, indexing, length |
| Boolean | `true`, `false` | AND, OR, NOT, comparison |
| Null/None | `null`, `None`, `undefined` | presence check |

Operations are specific to types. You cannot divide a string by a number (the operation is not defined). You cannot add a number to a boolean (in most languages without coercion). The type system enforces these rules.

```python
# Type-appropriate operations
print(10 + 5)        # 15 (integer addition)
print("hello" + " " + "world")  # "hello world" (string concatenation)
print(3 * 7)         # 21 (integer multiplication)
print("ha" * 3)      # "hahaha" (string repetition)
print(10 / 3)        # 3.333... (float division)
print(10 // 3)       # 3 (integer division)
```

```javascript
// Type-appropriate and type-coerced operations
console.log(10 + 5);           // 15
console.log("hello " + "world");  // "hello world"
console.log("10" + 5);         // "105" (coercion: number → string)
console.log("10" - 5);         // 5 (coercion: string → number)
console.log(10 / 3);           // 3.333...
```

### Expressions and Statements

An expression is any piece of code that produces a value. A statement is an instruction that performs an action.

```python
# Expressions (produce values)
42
3 + 4
len("hello")
max([1, 2, 3])

# Statements (perform actions)
x = 42          # assignment statement
print("hello")  # function call statement
if x > 10:      # conditional statement
    print("big")
```

The distinction matters because expressions can be composed — you can put one expression inside another. Statements cannot be nested the same way.

```python
# Expression composition
result = max(len("hello"), len("world")) * 2 + 1
```

This works because `max(...)`, `len(...)`, and `*` are all expressions. The interpreter evaluates from the inside out.

### Variables and Mutation

A variable is a named reference to a value. You can think of it as a label attached to a box in memory.

```python
name = "Alice"    # bind the value "Alice" to the name "name"
age = 25          # bind the value 25 to the name "age"
age = 26          # rebind: now "age" refers to 26
```

```javascript
let name = "Alice";
let age = 25;
age = 26;         // reassignment
```

Languages differ in their mutation philosophy:
- Python: variables are references. Rebinding changes what the name points to. Mutable objects (lists, dicts) can be modified in place.
- JavaScript: `let` allows reassignment, `const` does not. Objects declared with `const` can still be mutated.
- Java: primitives are values, objects are references. `final` prevents rebinding.
- Rust: variables are immutable by default. Use `let mut` for mutation.

```python
# Mutation in Python
numbers = [1, 2, 3]
numbers.append(4)    # mutates the list in place
numbers[0] = 99      # mutates an element
numbers = [5, 6, 7]  # rebinds the variable (different list)
```

```javascript
// Mutation in JavaScript
const numbers = [1, 2, 3];
numbers.push(4);      // mutates the array (allowed even with const)
numbers[0] = 99;      // mutates an element (allowed)
// numbers = [5, 6, 7];  // TypeError: assignment to constant variable
```

Understanding mutation is critical because it is the source of many bugs. When multiple variables reference the same mutable object, changing it through one variable affects all references.

### Control Flow Primitives

Every program uses three fundamental control structures:

**Sequence** — instructions execute in order, one after another:

```python
print("First")
print("Second")
print("Third")
```

By default, code runs from top to bottom. Sequence is the default mode.

**Selection** — choosing which path to execute based on a condition:

```python
temperature = 30
if temperature > 25:
    print("Hot day")
elif temperature > 15:
    print("Warm day")
else:
    print("Cool day")
```

```javascript
const temperature = 30;
if (temperature > 25) {
    console.log("Hot day");
} else if (temperature > 15) {
    console.log("Warm day");
} else {
    console.log("Cool day");
}
```

**Iteration** — repeating a block of code:

```python
# Count-controlled loop
for i in range(5):
    print(i)       # prints 0, 1, 2, 3, 4

# Condition-controlled loop
count = 0
while count < 5:
    print(count)
    count += 1
```

Every program you will ever write — from a simple script to a distributed system — is built from these three structures combined in different ways.

### Functions as Abstractions

A function is a named, reusable block of code that takes inputs and produces outputs. Functions are the primary mechanism for abstraction in most languages.

```python
def celsius_to_fahrenheit(c):
    return (c * 9/5) + 32

# Using the function
temp_f = celsius_to_fahrenheit(25)
print(temp_f)  # 77.0
```

Functions provide several benefits:

**Reusability** — write once, use many times. If you need to convert Celsius to Fahrenheit in ten places, you write the formula once as a function.

**Abstraction** — the caller does not need to know how the function works. They only need to know what it does (its interface) and what to pass (its parameters).

**Composability** — small functions can be combined to build complex behavior:

```python
def is_even(n):
    return n % 2 == 0

def square(n):
    return n * n

numbers = [1, 2, 3, 4, 5, 6]
result = sum(square(n) for n in numbers if is_even(n))
print(result)  # 4 + 16 + 36 = 56
```

**Testability** — functions with well-defined inputs and outputs can be tested in isolation.

A function should do one thing and do it well. If a function is doing multiple things, split it into smaller functions.

### Composition

Composition is building complex behavior by combining simple pieces. It is one of the most powerful ideas in programming.

**Function composition** — the output of one function becomes the input of another:

```python
def strip_punctuation(text):
    return "".join(c for c in text if c.isalnum() or c.isspace())

def word_count(text):
    return len(text.split())

text = "Hello, world! This is Python."
count = word_count(strip_punctuation(text))
print(count)  # 5
```

**Data composition** — combining simple data types into complex structures:

```python
# Simple values composed into a structured record
person = {
    "name": "Alice",
    "age": 30,
    "address": {
        "city": "New York",
        "zip": "10001"
    }
}
print(person["address"]["city"])  # "New York"
```

**Behavior composition** — combining objects or modules to create systems:

```python
class Logger:
    def log(self, message):
        print(f"[LOG] {message}")

class Database:
    def __init__(self, logger):
        self.logger = logger
    
    def save(self, data):
        self.logger.log("Saving data...")
        # save logic here

logger = Logger()
db = Database(logger)
db.save({"key": "value"})
```

Composition is preferred over inheritance in most modern design. Small, composable pieces are easier to understand, test, and reuse than large monolithic structures.

### State and Side Effects

State is the set of values that a program holds at a given point in time. A side effect is any change to state or interaction with the outside world that occurs during computation.

**Examples of side effects:**
- Modifying a variable
- Printing to the console
- Writing to a file
- Making a network request
- Updating a database
- Modifying a data structure in place

A pure function has no side effects and returns the same output for the same input every time:

```python
def add(a, b):
    return a + b  # pure: no side effects, deterministic
```

An impure function has side effects:

```python
counter = 0

def increment():
    global counter
    counter += 1  # side effect: modifies external state
    return counter
```

Managing state is one of the hardest parts of programming. Bugs often come from unexpected state changes — a variable changed in one part of the code affects behavior in another part.

Strategies for managing state:
- **Minimize mutable state** — prefer immutable data structures
- **Localize state** — keep state as local as possible
- **Encapsulate state** — limit who can read and write state
- **Thread state explicitly** — pass state as parameters instead of using globals

### The Evaluation Model

Understanding how your code executes is essential. The evaluation model describes how the language processes your program.

In most imperative languages, evaluation works as follows:

1. The interpreter/compiler processes code in sequence (top to bottom, left to right within an expression)
2. Expressions are evaluated to produce values
3. Statements are executed for their effect
4. Function calls transfer control to the function body, evaluate arguments, and return a value

```python
result = (3 + 4) * (5 - 2)
```

Step-by-step evaluation:
1. Evaluate `3 + 4` → `7`
2. Evaluate `5 - 2` → `3`
3. Evaluate `7 * 3` → `21`
4. Bind `result` to `21`

```python
def double(x):
    return x * 2

def add_one(x):
    return x + 1

result = double(add_one(5))
```

Step-by-step:
1. Evaluate `add_one(5)` → enter function, compute `5 + 1` → `6`, return
2. Evaluate `double(6)` → enter function, compute `6 * 2` → `12`, return
3. Bind `result` to `12`

This model is called **applicative order evaluation** — arguments are evaluated before the function is called. It is the default in most languages.

Understanding this model helps you reason about your code. When you see a complex expression, you can mentally trace through it step by step.

### Abstraction Layers

Programming happens at multiple levels of abstraction:

```
┌────────────────────────────────────┐
│  Application / Domain Logic        │  ← highest level
├────────────────────────────────────┤
│  Framework / Library Code          │
├────────────────────────────────────┤
│  Standard Library                  │
├────────────────────────────────────┤
│  Language Runtime / VM             │
├────────────────────────────────────┤
│  Operating System                  │
├────────────────────────────────────┤
│  Hardware (CPU, Memory, I/O)       │  ← lowest level
└────────────────────────────────────┘
```

As a programmer, you work at different abstraction levels depending on your task:
- Building a web app: you work at the application and framework level
- Writing a device driver: you work near the hardware level
- Building a database: you work across multiple levels

Each abstraction layer hides the complexity below it. When you call `print("hello")`, you do not need to think about how text is rendered on screen, how the OS schedules the process, or how the CPU executes the instructions.

Good programmers understand the layer they work at and the layer immediately below it. This helps them debug problems and make informed performance decisions.

### Common Patterns

Certain patterns appear repeatedly in programming. Recognizing them helps you write better code faster.

**Accumulator pattern** — building a result by processing elements one by one:

```python
def sum_even(numbers):
    total = 0
    for n in numbers:
        if n % 2 == 0:
            total += n
    return total

print(sum_even([1, 2, 3, 4, 5, 6]))  # 12
```

**Filter-map-reduce** — the three most common collection operations:

```python
items = [1, 2, 3, 4, 5, 6]

# Filter: keep only even numbers
evens = [n for n in items if n % 2 == 0]  # [2, 4, 6]

# Map: transform each number
squares = [n * n for n in evens]  # [4, 16, 36]

# Reduce: combine all values
from functools import reduce
total = reduce(lambda a, b: a + b, squares)  # 56
```

```javascript
const items = [1, 2, 3, 4, 5, 6];

const evens = items.filter(n => n % 2 === 0);
const squares = evens.map(n => n * n);
const total = squares.reduce((a, b) => a + b, 0);
console.log(total); // 56
```

**Early return / guard clause** — exit early when the preconditions are not met:

```python
def process_order(order):
    if not order:
        return None           # guard: no order
    if not order["items"]:
        return {"total": 0}    # guard: empty order
    if order["status"] != "active":
        raise ValueError("Order is not active")
    
    # Main logic only runs when all guards pass
    total = sum(item["price"] for item in order["items"])
    return {"total": total}
```

This pattern reduces nesting and makes the main logic clear.

### Writing Readable Code

Code is read far more often than it is written. Readable code reduces bugs, makes reviews faster, and helps your future self understand what you were thinking.

**Use meaningful names:**

```python
# Bad
x = 10
y = 3.14
z = x * y * y

# Good
radius = 10
pi = 3.14
area = pi * radius ** 2
```

**Keep functions small:**

```python
# Bad: function does too much
def process(data):
    # validate, transform, save, log — all in one function
    pass

# Good: each function has one responsibility
def validate(data): ...
def transform(data): ...
def save(data): ...
def log(data): ...
```

**Avoid deep nesting:**

```python
# Bad: deeply nested
def check_access(user, resource):
    if user:
        if user.is_active:
            if resource:
                if resource.is_public:
                    return True
    return False

# Good: guard clauses
def check_access(user, resource):
    if not user or not user.is_active:
        return False
    if not resource:
        return False
    if resource.is_public:
        return True
    return False
```

**Be consistent:**
- Follow the language's style guide (PEP 8 for Python, Prettier for JavaScript)
- Use consistent naming conventions (snake_case, camelCase, PascalCase)
- Format code consistently (use automatic formatters)

**Comment the why, not the what:**

```python
# Bad: explains what the code already says
# Multiply price by quantity to get total
total = price * quantity

# Good: explains why this approach was chosen
# Use integer arithmetic to avoid floating-point rounding errors
total_cents = price_cents * quantity
```

### The Debugging Mindset

Bugs are inevitable. The skill is not avoiding them entirely but fixing them efficiently.

**The scientific method for debugging:**

1. **Observe** — what is the symptom? What should happen vs what actually happens?
2. **Hypothesize** — what could cause this? Form a specific, testable hypothesis.
3. **Experiment** — change one thing and observe the result.
4. **Repeat** — narrow down the cause until you find the exact line.

**Common debugging techniques:**

**Print debugging** — add print statements to see intermediate values:

```python
def factorial(n):
    print(f"factorial called with n={n}")
    if n <= 1:
        return 1
    result = n * factorial(n - 1)
    print(f"factorial({n}) = {result}")
    return result
```

**Rubber duck debugging** — explain the problem out loud to a rubber duck (or a coworker). The act of explaining often reveals the solution.

**Binary search through code** — comment out half the code to narrow down where the bug is. If the bug disappears, it is in the commented half. Repeat.

**Read the error message carefully.** Error messages tell you:
- What went wrong (the error type)
- Where it happened (the file and line number)
- What was happening (the call stack)

```python
Traceback (most recent call last):
  File "main.py", line 10, in <module>
    result = divide(10, 0)
  File "main.py", line 5, in divide
    return a / b
ZeroDivisionError: division by zero
```

This tells you: at line 5 in `divide`, we tried to divide `a` by `b` where `b` was `0`. The call came from line 10 in the main module.

**Reproduce the bug consistently** before trying to fix it. A bug you can reproduce is a bug you can fix. A bug you cannot reproduce might not be a bug at all.

## Glossary

| Term | Definition |
|------|------------|
| Expression | Code that produces a value |
| Statement | Code that performs an action |
| Value | A concrete piece of data (42, "hello", true) |
| Type | A classification of values that determines valid operations |
| Variable | A named reference to a value |
| Mutation | Changing the value or state of a variable or object |
| State | The set of all values held by a program at a point in time |
| Side effect | Any change to state or interaction with the outside world |
| Composition | Building complex behavior from simple pieces |
| Abstraction | Hiding complexity behind a simple interface |
| Evaluation | The process of computing the value of an expression |
| Accumulator | A variable that collects results across iterations |
| Recursion | A function that calls itself |
| Iteration | Repeating a block of code (looping) |
| Deterministic | Always produces the same output for the same input |
| Pure function | A function with no side effects |
| Scope | The region of code where a variable is accessible |
| Namespace | A container that maps names to objects |
| Binding | The association between a name and a value |
| Reference | A value that points to an object in memory |
| Literal | A value written directly in code (42, "hello") |
| Operator | A symbol that performs an operation (+, -, *) |
| Operand | A value that an operator acts upon |
| Precedence | Rules that determine the order of operator evaluation |
| Associativity | Rules that determine evaluation order for operators with equal precedence |
| Guard clause | An early return that handles a precondition check |

## Quick References

- [MDN JavaScript Guide](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide) — comprehensive JavaScript reference
- [Python Official Tutorial](https://docs.python.org/3/tutorial/) — official Python introduction
- [Structure and Interpretation of Computer Programs](https://mitpress.mit.edu/sicp/) — classic computer science text
- [Codecademy Learn to Code](https://www.codecademy.com/learn/learn-how-to-code) — interactive programming fundamentals
- [Teach Yourself Programming in Ten Years](https://www.norvig.com/21-days.html) — Peter Norvig's essay on learning programming

## Next Steps

- [Variables & Data Types](../variables-and-data-types.md) — dive into the fundamental data building blocks
- [Operators & Expressions](../operators-and-expressions.md) — learn how to combine values into computations
- [Control Flow](../control-flow.md) — make decisions and repeat actions in your code
