# Advanced Operators

## Description

Type-specific operators, bitwise manipulation, spread/rest syntax, operator overloading, and other specialized operator patterns beyond basic arithmetic and logic.

## Prerequisites

- [Operators & Expressions](operators-and-expressions.md) — basic operators, precedence, associativity, short-circuit evaluation

## Table of Contents

- [Type-Specific Operators](#type-specific-operators)
- [The Walrus Operator](#the-walrus-operator)
- [Nullish Coalescing and the Or Pattern](#nullish-coalescing-and-the-or-pattern)
- [Optional Chaining](#optional-chaining)
- [Spread and Rest](#spread-and-rest)
- [Bitwise Operators](#bitwise-operators)
- [Operator Overloading in Python](#operator-overloading-in-python)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Type-Specific Operators

**Membership `in` (Python):**

```python
fruits = ["apple", "banana", "cherry"]
print("banana" in fruits)   # True
print("grape" in fruits)    # False
print("a" in "hello")       # True (strings are iterable)
print(3 in range(1, 10))    # True
print("x" not in "hello")   # True
```

**Typeof (JavaScript):**

```javascript
console.log(typeof 42);           // "number"
console.log(typeof "hello");      // "string"
console.log(typeof true);         // "boolean"
console.log(typeof undefined);    // "undefined"
console.log(typeof null);         // "object"  (historical bug!)
console.log(typeof {});           // "object"
console.log(typeof []);           // "object"
console.log(typeof function(){}); // "function"
console.log(typeof Symbol());     // "symbol"
```

`typeof` is always safe — it never throws an error, even on undeclared variables.

**Identity `is` (Python):**

```python
a = [1, 2, 3]
b = [1, 2, 3]
print(a is a)   # True  (same object)
print(a is b)   # False (different objects)
print(a == b)   # True  (same value)

if result is None:
    print("no result")
```

Use `is` for `None` comparison — it is faster and more idiomatic than `==`.

**Instanceof (JavaScript):**

```javascript
console.log([] instanceof Array);       // true
console.log({} instanceof Object);      // true
console.log(new Date() instanceof Date); // true
console.log("hello" instanceof String); // false (primitive)
console.log(42 instanceof Number);      // false (primitive)
```

**Type checks in Java and Go:**

```java
// Java
if (obj instanceof String) { String s = (String) obj; }
if (obj.getClass() == String.class) { ... }
```

```go
// Go: type assertion
var i interface{} = "hello"
s, ok := i.(string)  // s = "hello", ok = true
```

### The Walrus Operator

The walrus operator `:=` (Python 3.8+) assigns a value to a variable as part of an expression.

```python
# Without walrus — call twice
data = get_data()
if data is not None:
    process(data)

# With walrus — call once
if (data := get_data()) is not None:
    process(data)

# While loop
while (line := file.readline()) != "":
    print(line.strip())

# Comprehension with reused computation
results = [y for x in data if (y := transform(x)) is not None]
```

**Parentheses are required** when used in a larger expression:

```python
if (n := len(items)) > 10:  # correct
    print(f"Too many: {n}")

if n := len(items) > 10:    # bug: parsed as n := (len(items) > 10)
```

The walrus operator eliminates duplication. Use it when it improves clarity.

### Nullish Coalescing and the Or Pattern

**JavaScript `??` (ES2020):** returns the right operand only if the left is `null` or `undefined`.

```javascript
const value = 0;
console.log(value || 42);     // 42  (0 is falsy)
console.log(value ?? 42);     // 0   (0 is not null/undefined)

const name = "";
console.log(name || "Guest");   // "Guest"
console.log(name ?? "Guest");   // ""

console.log(null ?? "fallback");   // "fallback"
console.log(undefined ?? "fallback"); // "fallback"
console.log(false ?? true);    // false
```

**Python `or` pattern:** treats all falsy values (including `0`, `""`, `[]`) the same.

```python
value = 0
result = value or 42
print(result)  # 42 — may not be what you want

# To handle only None:
result = value if value is not None else 42
print(result)  # 0

def coalesce(*args):
    for arg in args:
        if arg is not None:
            return arg
    return None

print(coalesce(None, 0, "", 42))  # 0
```

### Optional Chaining

**JavaScript `?.` (ES2020):** accesses properties without throwing on `null`/`undefined`.

```javascript
const user = null;
// const name = user.name;  // TypeError

const name = user?.name;
console.log(name);  // undefined

const city = user?.address?.city;
console.log(city);  // undefined

const result = callback?.();  // undefined if null
const value = obj?.[key];     // bracket notation
```

**Python:** no built-in optional chaining. Use manual checks:

```python
user = None

# Manual check
if user is not None:
    name = user.get("name")
else:
    name = None

# Try/except
try:
    name = user["name"]
except (TypeError, KeyError):
    name = None
```

**Java:**

```java
String name = Optional.ofNullable(user)
    .map(u -> u.name)
    .orElse(null);
```

**Go:**

```go
var name string
if user != nil {
    name = user.Name
}
```

### Spread and Rest

The spread operator (`...`) unpacks iterable elements. The rest parameter collects elements into a single structure.

**JavaScript:**

```javascript
// Array spread
const combined = [...[1, 2], ...[3, 4]];
console.log(combined);  // [1, 2, 3, 4]

// Object spread (ES2018)
const merged = { ...{a: 1}, ...{b: 2} };
console.log(merged);  // { a: 1, b: 2 }

// Override
const defaults = { theme: "light", lang: "en" };
const config = { ...defaults, lang: "fr" };
console.log(config);  // { theme: "light", lang: "fr" }

// Function arguments
console.log(Math.max(...[1, 5, 10]));  // 10

// Rest parameters
function sum(...numbers) {
    return numbers.reduce((a, b) => a + b, 0);
}

// Destructuring with rest
const [first, second, ...rest] = [1, 2, 3, 4, 5];
console.log(first);  // 1
console.log(rest);   // [3, 4, 5]

const { name, ...other } = { name: "Alice", age: 30 };
console.log(other);  // { age: 30 }
```

**Python:**

```python
# Spread
combined = [*[1, 2], *[3, 4]]
print(combined)  # [1, 2, 3, 4]

merged = {**{"a": 1}, **{"b": 2}}
print(merged)  # {"a": 1, "b": 2}

defaults = {"theme": "light", "lang": "en"}
config = {**defaults, "lang": "fr"}

print(max(*[1, 5, 10]))  # 10

def add(a, b, c):
    return a + b + c

print(add(*[1, 2, 3]))  # 6

# Rest
def sum_all(*numbers):
    total = 0
    for n in numbers:
        total += n
    return total

def create_profile(**kwargs):
    for k, v in kwargs.items():
        print(f"{k}: {v}")

first, second, *rest = [1, 2, 3, 4, 5]
print(rest)  # [3, 4, 5]
```

**Comparison:**

| Feature | Python | JavaScript |
|---------|--------|------------|
| List/array spread | `[*a, *b]` | `[...a, ...b]` |
| Dict/object spread | `{**a, **b}` | `{...a, ...b}` |
| Rest in destructuring | `*rest` | `...rest` |
| Rest in params | `*args` `**kwargs` | `...args` |
| Spread in call | `f(*args)` | `f(...args)` |

### Bitwise Operators

Bitwise operators work on the binary representation of integers, operating on each bit independently.

| Operator | Name | Python | JavaScript |
|----------|------|--------|------------|
| `&` | AND | Yes | Yes |
| `|` | OR | Yes | Yes |
| `^` | XOR | Yes | Yes |
| `~` | NOT | Yes | Yes |
| `<<` | Left shift | Yes | Yes |
| `>>` | Right shift | Yes | Yes |
| `>>>` | Unsigned right shift | No | Yes |

```python
a = 0b1100  # 12
b = 0b1010  # 10

print(bin(a & b))   # 0b1000 (8)   — bits in both
print(bin(a | b))   # 0b1110 (14)  — bits in either
print(bin(a ^ b))   # 0b0110 (6)   — bits in exactly one
print(bin(~a))      # -0b1101 (-13) — NOT (two's complement)
print(bin(a << 2))  # 0b110000 (48) — multiply by 4
print(bin(a >> 2))  # 0b0011 (3)    — divide by 4
```

```javascript
let a = 0b1100, b = 0b1010;
console.log(a & b);   // 8
console.log(a | b);   // 14
console.log(a ^ b);   // 6
console.log(~a);      // -13
console.log(a << 2);  // 48
console.log(a >> 2);  // 3
console.log(a >>> 2); // 3 (unsigned)
```

**Truth table:**

| a | b | a & b | a \| b | a ^ b |
|---|---|-------|--------|-------|
| 0 | 0 | 0 | 0 | 0 |
| 0 | 1 | 0 | 1 | 1 |
| 1 | 0 | 0 | 1 | 1 |
| 1 | 1 | 1 | 1 | 0 |

**Use case: flags and permissions**

```python
READ = 1     # 001
WRITE = 2    # 010
EXECUTE = 4  # 100

perms = READ | WRITE  # 011
print(perms & READ)    # 1 (truthy) — has read
print(perms & EXECUTE) # 0 (falsy) — no execute

perms ^= WRITE  # toggle write
```

```javascript
const READ = 1, WRITE = 2, EXECUTE = 4;
let perms = READ | WRITE;
console.log(perms & READ);    // 1
console.log(perms & EXECUTE); // 0
```

**Use case: color components**

```python
color = 0xFFA500  # orange
red = (color >> 16) & 0xFF    # 0xFF
green = (color >> 8) & 0xFF   # 0xA5
blue = color & 0xFF           # 0x00
packed = (red << 16) | (green << 8) | blue
print(hex(packed))  # 0xffa500
```

**JavaScript note:** bitwise operators treat operands as 32-bit signed integers, even though numbers are 64-bit floats:

```javascript
console.log((1234567890123 | 0));  // -539222987 — truncation!
```

### Operator Overloading in Python

Python allows user-defined types to use standard operators by defining dunder (double underscore) methods.

```python
class Vector:
    def __init__(self, x, y):
        self.x, self.y = x, y

    def __add__(self, other):
        return Vector(self.x + other.x, self.y + other.y)

    def __sub__(self, other):
        return Vector(self.x - other.x, self.y - other.y)

    def __mul__(self, scalar):
        return Vector(self.x * scalar, self.y * scalar)

    def __eq__(self, other):
        return self.x == other.x and self.y == other.y

    def __neg__(self):
        return Vector(-self.x, -self.y)

    def __abs__(self):
        return (self.x ** 2 + self.y ** 2) ** 0.5

    def __str__(self):
        return f"({self.x}, {self.y})"

v1 = Vector(3, 4)
v2 = Vector(1, 2)
print(v1 + v2)           # (4, 6)     — __add__
print(v1 * 2)            # (6, 8)     — __mul__
print(v1 == Vector(3,4))  # True      — __eq__
print(abs(v1))           # 5.0       — __abs__
```

**Reflected operators** handle `other + self` when the left operand does not support the operation:

```python
class Vector:
    def __mul__(self, scalar):
        return Vector(self.x * scalar, self.y * scalar)
    def __rmul__(self, scalar):
        return Vector(self.x * scalar, self.y * scalar)

v = Vector(3, 4)
print(2 * v)  # Vector(6, 8) — __rmul__
```

**Common dunder methods:**

| Category | Operators | Methods |
|----------|-----------|---------|
| Arithmetic | `+ - * / // % **` | `__add__`, `__sub__`, `__mul__`, `__truediv__`, `__floordiv__`, `__mod__`, `__pow__` |
| Reflected | `other + self` | `__radd__`, `__rsub__`, `__rmul__` |
| In-place | `+= -= *=` | `__iadd__`, `__isub__`, `__imul__` |
| Unary | `- + ~ abs` | `__neg__`, `__pos__`, `__invert__`, `__abs__` |
| Comparison | `== != < <= > >=` | `__eq__`, `__ne__`, `__lt__`, `__le__`, `__gt__`, `__ge__` |
| Conversion | `str() int() bool()` | `__str__`, `__int__`, `__bool__` |

**Limitations:** cannot create new operators, cannot change precedence, cannot overload `and`/`or`/`not`. JavaScript, Java, and Go do not support operator overloading.

## Glossary

| Term | Definition |
|------|------------|
| Bitwise | Operating on individual bits of an integer |
| Walrus operator | Assignment expression `:=` in Python |
| Nullish | Specifically null or undefined (JavaScript) |
| Coalescing | Selecting the first non-nullish value |
| Optional chaining | Accessing properties safely when intermediate values may be nullish |
| Spread | Unpacking elements from an iterable into individual items |
| Rest | Collecting individual items into a single iterable |
| Operator overloading | Defining custom behavior for operators on user-defined types |
| Dunder method | A method with double underscores (Python special method) |
| Reflected operator | An operator method called when the left operand does not support the operation |
| In-place operator | An operator that modifies and assigns in one step |
| Flags | Multiple booleans packed into a single integer using bits |
| Mask | A bit pattern used to select specific bits |
| Two's complement | Binary representation of negative integers |

## Quick References

- [Python `:=` Walrus Operator PEP 572](https://peps.python.org/pep-0572/) — the original proposal with rationale and examples
- [MDN Optional Chaining](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining) — JavaScript `?.` documentation
- [MDN Nullish Coalescing](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing) — JavaScript `??` documentation
- [MDN Spread Syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) — JavaScript `...` spread and rest
- [Python Data Model](https://docs.python.org/3/reference/datamodel.html#special-method-names) — complete list of Python dunder methods for operator overloading

## Next Steps

- [Control Flow](../control-flow.md) — branching and looping with conditions built from expressions
