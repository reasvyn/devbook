# Variables

## Description

How data is stored, named, and managed in memory — variables, constants, scope, and the rules that govern their use.

## Prerequisites

- [Programming Fundamentals](intro/programming-fundamentals.md) — basic understanding of programming concepts

## Table of Contents

- [What Is a Variable?](#what-is-a-variable)
- [Declaration, Assignment, Initialization](#declaration-assignment-initialization)
- [Naming Conventions](#naming-conventions)
- [Mutable vs Immutable Variables](#mutable-vs-immutable-variables)
- [Constants](#constants)
- [Scope Basics](#scope-basics)
- [Variable Shadowing](#variable-shadowing)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is a Variable?

A variable is a named storage location in memory that holds a value. Instead of working with raw memory addresses, you use descriptive names that make your code readable and maintainable.

Think of a variable as a labeled box. The label is the variable name, and the box contains the value. When you need to use the value, you refer to it by the label.

```python
age = 25
# "age" is the label, 25 is the value stored
print(age)  # 25
```

```javascript
let age = 25;
console.log(age); // 25
```

```java
int age = 25;
System.out.println(age); // 25
```

```go
age := 25
fmt.Println(age)
```

Behind the scenes, the variable name maps to a memory address. When you access the variable, the programming language looks up the address and retrieves the value stored there. You do not need to manage addresses directly — the language handles it for you.

### Declaration, Assignment, Initialization

These three concepts are closely related but distinct:

**Declaration** — introducing a variable name to the program. In some languages (Python, JavaScript, Go), declaration happens automatically on first assignment. In others (Java, C, TypeScript), you must declare the type explicitly.

```python
# Python: no explicit declaration — assignment creates the variable
name = "Alice"
```

```javascript
// JavaScript: let/const/var declares the variable
let name;        // declaration
name = "Alice";  // assignment

// Combined: declaration + initialization
let age = 30;    // declares and initializes
```

```java
// Java: type must be declared
String name;        // declaration
name = "Alice";     // assignment
String fullName = "Alice Smith";  // declaration + initialization
```

**Assignment** — storing a value in a variable. If the variable already exists, assignment overwrites the old value.

```python
x = 10
x = 20  # overwrites the previous value
print(x)  # 20
```

**Initialization** — the first assignment to a variable. Before initialization, a variable has no meaningful value.

```javascript
let x;          // declaration, x is undefined
x = 10;         // initialization
```

Some languages (Java, C#, Go) require variables to be initialized before use. Others (Python, JavaScript) allow uninitialized variables but accessing them causes an error.

### Naming Conventions

Choosing good variable names is one of the most important skills in programming. A good name makes code self-documenting.

**Rules across most languages:**
- Names must start with a letter, underscore (`_`), or sometimes a dollar sign (`$`)
- Names cannot contain spaces
- Names are case-sensitive (`age`, `Age`, and `AGE` are three different variables)
- Names cannot be reserved keywords (`if`, `for`, `class`, `return`)

**Common naming conventions:**

| Style | Example | Used In |
|-------|---------|---------|
| snake_case | `user_name`, `total_count` | Python, Rust |
| camelCase | `userName`, `totalCount` | JavaScript, Java, Go |
| PascalCase | `UserName`, `TotalCount` | Classes / types (most languages) |
| SCREAMING_SNAKE_CASE | `MAX_SIZE`, `API_KEY` | Constants |

```python
# Python style: snake_case
first_name = "Alice"
is_active = True
total_price = 99.99
```

```javascript
// JavaScript style: camelCase
let firstName = "Alice";
let isActive = true;
let totalPrice = 99.99;
```

**Good naming practices:**
- Be descriptive: `user_count` not `uc`
- Be specific: `active_users` not `data`
- Use pronounceable names: `customer_id` not `cust_id`
- Avoid abbreviations: `index` not `idx` (unless widely understood)
- Boolean variables should sound like yes/no questions: `is_active`, `has_permission`, `can_edit`

```python
# Bad names
a = 5
b = "hello"
c = True
d = [1, 2, 3]

# Good names
retry_count = 5
greeting = "hello"
is_verified = True
user_ids = [1, 2, 3]
```

### Mutable vs Immutable Variables

Variables can be mutable (value can change) or immutable (value cannot change after initialization).

```python
# Python: variables are mutable by default
count = 10
count = 20  # allowed

# Python has no const for variables (only for module-level constants by convention)
```

```javascript
// JavaScript: let vs const
let count = 10;
count = 20;    // allowed

const maxUsers = 100;
maxUsers = 200;  // TypeError: assignment to constant variable
```

```java
// Java: final prevents reassignment
final int MAX_USERS = 100;
// MAX_USERS = 200;  // Compile error
```

```rust
// Rust: variables are immutable by default
let count = 10;
// count = 20;  // Compile error: cannot assign twice

let mut mutable = 10;
mutable = 20;  // allowed
```

Immutable variables make code easier to reason about because the value cannot change unexpectedly. Prefer immutability unless you have a specific reason to allow mutation.

Note that immutability of a variable (cannot reassign) is different from immutability of the value it points to:

```javascript
const numbers = [1, 2, 3];
numbers.push(4);    // allowed — the array is mutated, not the variable
// numbers = [5, 6, 7];  // TypeError — cannot reassign a const
```

### Constants

Constants are variables whose value does not change after initialization. Using constants makes code more readable and prevents accidental modification.

```python
# Python: no true constants, but convention uses UPPER_CASE
MAX_LOGIN_ATTEMPTS = 5
API_ENDPOINT = "https://api.example.com"
DEFAULT_TIMEOUT_SECONDS = 30
```

```javascript
// JavaScript: const
const MAX_LOGIN_ATTEMPTS = 5;
const API_ENDPOINT = "https://api.example.com";
```

```java
// Java: final
public static final int MAX_LOGIN_ATTEMPTS = 5;
```

```go
// Go: const
const MaxLoginAttempts = 5
```

```rust
// Rust: let without mut is constant-like
const MAX_LOGIN_ATTEMPTS: u32 = 5;
```

**Why use constants:**
- **Self-documenting:** `MAX_LOGIN_ATTEMPTS` is clearer than `5`
- **Single source of truth:** change in one place
- **Prevents magic numbers:**

```python
# Bad: magic numbers
def calculate_discount(price):
    return price * 0.1  # What is 0.1? Why 10%?

# Good: named constant
DISCOUNT_RATE = 0.10

def calculate_discount(price):
    return price * DISCOUNT_RATE
```

### Scope Basics

Scope determines where a variable is accessible in your code.

**Global scope** — variables declared outside any function or block:

```python
# Python: global scope
count = 0  # global

def increment():
    global count  # must declare to modify global
    count += 1

increment()
print(count)  # 1
```

**Local scope** — variables declared inside a function:

```python
# Python: local scope
def greet(name):
    message = f"Hello, {name}"  # local variable
    return message

print(message)  # NameError: name 'message' is not defined
```

```javascript
// JavaScript: function scope (var) / block scope (let, const)
function greet(name) {
    let message = `Hello, ${name}`;
    return message;
}
// console.log(message);  // ReferenceError
```

**Block scope** — variables limited to the block they are defined in:

```python
# Python: block scope (if/for/while do not create new scope in Python 2,
# but in Python 3, loop variables leak to the enclosing scope)
for i in range(5):
    value = i * 2
print(i)  # 4 — loop variable leaks
```

```javascript
// JavaScript: let and const are block-scoped
{
    let x = 10;
    const y = 20;
}
// console.log(x);  // ReferenceError
// console.log(y);  // ReferenceError

// var is function-scoped, not block-scoped
{
    var z = 30;
}
console.log(z);  // 30 — leaks out of block
```

**Nested scope and the lookup chain:**

When you access a variable, the language looks it up in the current scope, then the enclosing scope, then the global scope:

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

```javascript
let x = "global";

function outer() {
    let x = "outer";
    
    function inner() {
        let x = "inner";
        console.log(x);  // "inner"
    }
    
    inner();
    console.log(x);  // "outer"
}

outer();
console.log(x);  // "global"
```

This chain is called lexical scoping — the scope is determined by the structure of the code, not the call order.

### Variable Shadowing

Shadowing occurs when a variable in an inner scope has the same name as a variable in an outer scope. The inner variable temporarily hides (shadows) the outer one.

```python
x = 10

def my_function():
    x = 20  # shadows the global x
    print(x)  # 20

my_function()
print(x)  # 10 — global unaffected
```

```javascript
let x = 10;

function myFunction() {
    let x = 20;  // shadows the outer x
    console.log(x);  // 20
}

myFunction();
console.log(x);  // 10
```

```rust
// Rust: shadowing is explicit and common
let x = 10;
let x = x + 5;      // shadows previous x with a new binding
println!("{}", x);  // 15
```

Shadowing is useful for transforming a value while keeping the same name:

```python
# Using the same name for transformed values
name = input("Enter your name: ")
name = name.strip()
name = name.capitalize()
print(f"Hello, {name}")
```

But excessive shadowing confuses readers. Avoid shadowing in nested scopes where the shadowed variable is still needed:

```python
# Confusing shadowing
count = 0

def process(items):
    count = len(items)  # shadows global count
    for item in items:
        count = count + 1  # which count is this?
    return count

# Prefer distinct names
def process(items):
    item_count = len(items)
    ...
```

## Glossary

| Term | Definition |
|------|------------|
| Variable | A named storage location holding a value |
| Constant | A variable whose value does not change after initialization |
| Literal | A value written directly in source code, such as `42` or `"hello"` |
| Identifier | The name given to a variable, function, or type |
| Keyword | A reserved word with special meaning in the language (e.g., `if`, `for`, `return`) |
| Declaration | Introducing a variable name to the program, optionally with its type |
| Assignment | Storing a value in a variable, possibly overwriting the previous value |
| Initialization | The first assignment to a variable, giving it a meaningful value |
| Mutation | Changing the value of a variable or the state of an object |
| Immutability | A property where a value or reference cannot be changed after creation |
| Scope | The region of code where a variable is accessible |
| Global scope | Scope accessible throughout the entire program |
| Local scope | Scope limited to a specific function |
| Block scope | Scope limited to a block delimited by `{}` |
| Shadowing | An inner variable hiding an outer variable with the same name |
| Lexical scoping | Scope determined by the structure of the source code, not call order |
| Closure | A function that retains access to variables from its enclosing scope |
| Hoisting | The behavior where variable and function declarations are moved to the top of their scope (JavaScript) |

## Quick References

- [MDN JavaScript Variables](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types) — JavaScript variables and types documentation
- [Python Variables](https://docs.python.org/3/tutorial/introduction.html) — Python official tutorial on variables
- [Rust Shadowing](https://doc.rust-lang.org/book/ch03-01-variables-and-mutability.html#shadowing) — Rust documentation on variable shadowing and mutability
- [JavaScript Scope](https://developer.mozilla.org/en-US/docs/Glossary/Scope) — MDN glossary entry on scope
- [Worst Practice: Mutable Default Arguments](https://docs.python-guide.org/writing/gotchas/) — Python gotchas including mutable default arguments

## Next Steps

- [Data Types](../data-types.md) — explore the kinds of values you can store in variables
