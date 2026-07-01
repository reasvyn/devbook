# Data Types

## Description

The fundamental types that all programs use — numbers, text, booleans, and the absence of value — and how they behave across languages.

## Prerequisites

- [Variables](../variables.md) — understanding of how data is stored and named
- [Programming Fundamentals](intro/programming-fundamentals.md) — basic understanding of programming concepts

## Table of Contents

- [Primitive Data Types](#primitive-data-types)
- [Type Systems](#type-systems)
- [Type Conversion](#type-conversion)
- [Strings](#strings)
- [Numbers](#numbers)
- [Booleans](#booleans)
- [Null, Undefined, None](#null-undefined-none)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Primitive Data Types

Primitive types are the simplest building blocks provided by the language. Everything else is built from these.

**Integer** — whole numbers, positive or negative:

```python
age = 30
count = -5
large = 1_000_000  # underscore for readability
```

```javascript
let age = 30;
let count = -5;
let large = 1_000_000;
```

```java
int age = 30;        // 32-bit
long big = 1_000_000_000L;  // 64-bit
```

Languages differ in integer range:
- Python: arbitrary precision (grows as needed)
- JavaScript: 64-bit floating point, integers up to 2^53 are exact
- Java: multiple integer types (byte, short, int, long) with different ranges
- Go: int8, int16, int32, int64, uint variants

**Float / Double** — numbers with decimal parts:

```python
price = 19.99
pi = 3.14159
scientific = 1.5e-10
```

```javascript
let price = 19.99;
let pi = 3.14159;
let scientific = 1.5e-10;
```

```go
var price float64 = 19.99
```

Floating-point numbers have limited precision. They cannot represent all decimal values exactly:

```python
print(0.1 + 0.2)  # 0.30000000000000004, not 0.3
```

```javascript
console.log(0.1 + 0.2);  // 0.30000000000000004
```

This is not a bug — it is a consequence of representing decimal numbers in binary. Financial calculations should use integer arithmetic (cents) or special decimal types.

**String** — text data:

```python
name = "Alice"
greeting = 'Hello'  # single quotes also work in Python
```

```javascript
let name = "Alice";
let greeting = 'Hello';  // single and double quotes are equivalent
let template = `Hello, ${name}!`;  // template literal (ES6+)
```

```java
String name = "Alice";  // Note: String is not a primitive in Java
```

**Boolean** — true or false:

```python
is_active = True    # capital T in Python
has_permission = False
```

```javascript
let isActive = true;
let hasPermission = false;
```

```go
var isActive bool = true
```

**Null / None / undefined** — representing absence of a value:

```python
result = None  # Python's null value
```

```javascript
let result = null;       // explicit absence
let notAssigned;         // undefined (declared but not assigned)
```

```java
Object result = null;    // null reference
```

### Type Systems

A type system defines how types are enforced in a language. There are two main dimensions:

**Static vs Dynamic Typing:**

| Aspect | Static Typing | Dynamic Typing |
|--------|---------------|----------------|
| Types checked | At compile time | At runtime |
| Examples | Java, Go, Rust, TypeScript | Python, JavaScript, Ruby |
| Error detection | Earlier (during compilation) | Later (during execution) |
| Flexibility | Less flexible, more safety | More flexible, less safety |

```java
// Java: static typing — type is declared and checked at compile time
String name = "Alice";
name = 42;  // Compile error: incompatible types
```

```python
# Python: dynamic typing — type is checked at runtime
name = "Alice"
name = 42  # No error (at this line), but may cause issues later
```

Static typing catches type errors early, which is valuable in large codebases. Dynamic typing allows more rapid prototyping and flexibility.

**Strong vs Weak Typing:**

| Aspect | Strong Typing | Weak Typing |
|--------|---------------|-------------|
| Implicit coercion | Rare or restricted | Common and automatic |
| Examples | Python, Java, Go, Rust | JavaScript, PHP, C |
| Safety | Fewer type surprises | More flexible but riskier |

```python
# Python: strong typing — no implicit coercion between unrelated types
result = "hello" + 5  # TypeError: can only concatenate str (not "int") to str
```

```javascript
// JavaScript: weak typing — implicit coercion is common
let result = "hello" + 5;  // "hello5" (number coerced to string)
let sum = "10" - 5;        // 5 (string coerced to number)
```

**Type inference** is a feature of some statically-typed languages where the compiler deduces the type from the context:

```go
name := "Alice"    // Go infers type string
count := 42        // Go infers type int
```

```typescript
let name = "Alice";  // TypeScript infers type string
let count = 42;      // TypeScript infers type number
```

Type inference gives you the safety of static typing with less syntactic overhead.

### Type Conversion

Sometimes you need to convert a value from one type to another.

**Explicit conversion (casting):**

```python
# Python: explicit conversion functions
num_str = "42"
num_int = int(num_str)       # "42" → 42
num_float = float(num_str)   # "42" → 42.0
text = str(42)               # 42 → "42"
```

```javascript
// JavaScript: explicit conversion
let numStr = "42";
let numInt = parseInt(numStr, 10);   // "42" → 42
let numFloat = parseFloat("3.14");   // "3.14" → 3.14
let text = String(42);               // 42 → "42"
let bool = Boolean(1);               // 1 → true
```

```java
// Java: explicit conversion
int num = Integer.parseInt("42");      // String → int
double d = Double.parseDouble("3.14"); // String → double
String text = Integer.toString(42);    // int → String
```

**Implicit coercion (automatic conversion):**

Languages with weak typing (JavaScript, PHP) perform implicit coercion:

```javascript
// JavaScript: implicit coercion
let result = "5" - 3;     // 2 (string coerced to number)
let concat = "5" + 3;     // "53" (number coerced to string)
let bool = !"hello";      // false (non-empty string coerced to true, then negated)
```

Implicit coercion can cause subtle bugs:

```javascript
console.log(1 + "2" + 3);   // "123"
console.log(1 + 2 + "3");   // "33"
```

The `+` operator is left-associative. In the first case, `1 + "2"` produces `"12"`, then `"12" + 3` produces `"123"`. In the second case, `1 + 2` produces `3`, then `3 + "3"` produces `"33"`.

Strongly-typed languages (Python, Java) rarely coerce implicitly, which prevents these surprises.

### Strings

Strings represent text. They are one of the most commonly used types.

**String creation:**

```python
# Python
single = 'Hello'
double = "Hello"
multi_line = """This spans
multiple lines"""
```

```javascript
// JavaScript
let single = 'Hello';
let double = "Hello";
let multiLine = `This spans
multiple lines`;
```

**String interpolation (embedding expressions in strings):**

```python
# Python: f-strings (Python 3.6+)
name = "Alice"
age = 30
message = f"{name} is {age} years old"
print(message)  # "Alice is 30 years old"
```

```javascript
// JavaScript: template literals (ES6+)
let name = "Alice";
let age = 30;
let message = `${name} is ${age} years old`;
console.log(message);  // "Alice is 30 years old"
```

```java
// Java: string formatting
String message = String.format("%s is %d years old", name, age);
```

```go
message := fmt.Sprintf("%s is %d years old", name, age)
```

**Common string operations:**

```python
# Python
text = "Hello, World!"
print(len(text))           # 13
print(text.upper())        # "HELLO, WORLD!"
print(text.lower())        # "hello, world!"
print(text.split(","))     # ["Hello", " World!"]
print(text.startswith("Hello"))  # True
print(text.replace("World", "Python"))  # "Hello, Python!"
print(text[0:5])           # "Hello" (slicing)
```

```javascript
// JavaScript
let text = "Hello, World!";
console.log(text.length);             // 13
console.log(text.toUpperCase());      // "HELLO, WORLD!"
console.log(text.toLowerCase());      // "hello, world!"
console.log(text.split(","));         // ["Hello", " World!"]
console.log(text.startsWith("Hello")); // true
console.log(text.replace("World", "JS")); // "Hello, JS!"
console.log(text.slice(0, 5));        // "Hello"
```

**Escape sequences:**

```python
text = "She said \"hello\""   # She said "hello"
path = "C:\\Users\\Alice"      # C:\Users\Alice
newline = "Line1\nLine2"       # Line1
                               # Line2
```

```javascript
let text = "She said \"hello\"";
let path = "C:\\Users\\Alice";
let newline = "Line1\nLine2";
```

### Numbers

Numbers come in different flavors across languages.

**Integer types:**

```python
# Python: unified int type (arbitrary precision)
small = 42
big = 10 ** 100      # works fine in Python
negative = -7
```

```javascript
// JavaScript: single number type (64-bit floating point, IEEE 754)
let int = 42;
let hex = 0xFF;         // 255 (hexadecimal)
let binary = 0b1010;    // 10 (binary)
let octal = 0o77;       // 63 (octal)
```

**Floating-point precision:**

```python
print(0.1 + 0.2)  # 0.30000000000000004
print(0.1 + 0.2 == 0.3)  # False
```

```javascript
console.log(0.1 + 0.2);  // 0.30000000000000004
console.log(0.1 + 0.2 === 0.3);  // false
```

Solutions for precise decimal arithmetic:

```python
# Python: use decimal module
from decimal import Decimal
result = Decimal("0.10") + Decimal("0.20")
print(result)  # 0.30

# Or: use integer arithmetic (cents)
total_cents = 10 + 20  # 30 cents
print(total_cents / 100)  # 0.30
```

```javascript
// JavaScript: use toFixed for display, or integer arithmetic
let result = (0.10 * 100 + 0.20 * 100) / 100;
console.log(result);  // 0.30
```

**Special number values:**

```javascript
// JavaScript
console.log(NaN);           // Not a Number — result of invalid math
console.log(Infinity);      // Division by zero
console.log(-Infinity);     // Negative division by zero
console.log(Number.MAX_SAFE_INTEGER);  // 9007199254740991
```

```python
# Python
float("nan")     # NaN
float("inf")     # Infinity
```

### Booleans

Booleans represent logical truth values: `true` or `false`.

```python
is_active = True
is_admin = False
print(is_active and is_admin)  # False
print(is_active or is_admin)   # True
print(not is_active)            # False
```

```javascript
let isActive = true;
let isAdmin = false;
console.log(isActive && isAdmin);  // false
console.log(isActive || isAdmin);  // true
console.log(!isActive);            // false
```

**Truthy and falsy values:**

In conditional contexts (if statements, while loops), non-boolean values are evaluated as if they were booleans.

```python
# Python: falsy values
if 0:          # False
if "" or ''':   # False (empty string)
if None:       # False
if []:         # False (empty list)
if {}:         # False (empty dict)

# Everything else is truthy
if 42:         # True
if "hello":    # True
if [1, 2]:     # True
```

```javascript
// JavaScript: falsy values
if (false) {}      // false
if (0) {}          // false
if ("") {}         // false (empty string)
if (null) {}       // false
if (undefined) {}  // false
if (NaN) {}        // false

// Everything else is truthy
if (42) {}         // true
if ("hello") {}    // true
if ([]) {}         // true (empty array is truthy in JS!)
if ({}) {}         // true (empty object is truthy in JS!)
```

Note the JavaScript quirk: empty arrays and objects are truthy, unlike Python where they are falsy. These differences matter when writing conditional logic.

**Short-circuit evaluation:**

Logical operators short-circuit — they stop evaluating as soon as the result is determined:

```python
# Python: short-circuit with and/or
def get_user():
    print("get_user called")
    return {"name": "Alice"}

user = None or get_user()  # get_user is called because None is falsy
print(user)  # {"name": "Alice"}

# If the first value determines the result, the second is never evaluated
safe = False and expensive_function()  # expensive_function is never called
```

```javascript
// JavaScript: short-circuit with &&/||
const user = null || { name: "Alice" };
console.log(user);  // { name: "Alice" }

// Common pattern: default value
const name = inputName || "Guest";
```

### Null, Undefined, None

Every language has a way to represent "no value." The specific keyword varies.

```python
# Python: None
result = None
if result is None:
    print("No result")
```

```javascript
// JavaScript: null (explicit) and undefined (implicit)
let explicit = null;          // developer says "no value"
let implicit;                 // undefined — declared but not assigned
let missing = obj?.prop;      // undefined if obj is null/undefined

console.log(null == undefined);   // true (loose equality)
console.log(null === undefined);  // false (strict equality)
```

```java
// Java: null for reference types
String name = null;
// Accessing null.name throws NullPointerException
```

```go
// Go: nil for pointers, slices, maps, interfaces
var ptr *int = nil
```

**Common null-related bugs:**

```python
# Python: AttributeError on None
user = get_user()
print(user["name"])  # TypeError: 'NoneType' object is not subscriptable
```

```javascript
// JavaScript: TypeError on null
const user = getUser();
console.log(user.name);  // TypeError: Cannot read properties of null
```

Defensive patterns:

```python
# Python: check before access
user = get_user()
if user is not None:
    print(user["name"])
```

```javascript
// JavaScript: optional chaining (ES2020)
const user = getUser();
console.log(user?.name);  // undefined if user is null/undefined

// Nullish coalescing (ES2020)
const name = user?.name ?? "Guest";
```

## Glossary

| Term | Definition |
|------|------------|
| Type | A classification of values determining valid operations |
| Primitive type | A basic type built into the language (e.g., int, bool, string) |
| Reference type | A type whose value points to an object in memory |
| Type system | Rules governing how types are used and enforced in a language |
| Static typing | Types are checked at compile time |
| Dynamic typing | Types are checked at runtime |
| Strong typing | Limited or restricted implicit type conversion |
| Weak typing | Automatic and common implicit type conversion |
| Type inference | The compiler or runtime deduces the type from context |
| Type coercion | Implicit conversion of a value to a different type |
| Type casting | Explicit conversion of a value to a different type |
| Truthy | A value that evaluates to true in a boolean context |
| Falsy | A value that evaluates to false in a boolean context |
| NaN | "Not a Number" — the result of an invalid numeric operation |
| Null / nil | A value representing the intentional absence of an object |
| Undefined | A value indicating a variable has not been assigned a value |
| None | Python's singleton null value |
| String | A sequence of characters representing text |
| Integer | A whole number, positive or negative |
| Float | A number with a decimal part, typically IEEE 754 double-precision |
| Short-circuit | The behavior where logical operators stop evaluating once the result is determined |

## Quick References

- [MDN JavaScript Data Types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures) — JavaScript data types and structures
- [Python Built-in Types](https://docs.python.org/3/library/stdtypes.html) — Python standard type hierarchy
- [Understanding Type Systems](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html) — TypeScript handbook explaining static typing
- [IEEE 754 Floating Point](https://floating-point-gui.de/) — what every developer should know about floating-point arithmetic
- [JavaScript Equality Table](https://dorey.github.io/JavaScript-Equality-Table/) — visual guide to == vs ===
- [Java Primitive Types](https://docs.oracle.com/javase/tutorial/java/nutsandbolts/datatypes.html) — Java primitive data types documentation

## Next Steps

- [Operators & Expressions](../operators-and-expressions.md) — combining values into computations
