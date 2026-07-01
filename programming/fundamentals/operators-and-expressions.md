# Operators & Expressions

## Description

How values are combined, compared, and transformed into new values — the grammar of computation that lives at the heart of every program.

## Prerequisites

- [Variables & Data Types](../variables-and-data-types.md) — variables, data types, truthy/falsy, type coercion

## Table of Contents

- [What Is an Expression?](#what-is-an-expression)
- [What Is an Operator?](#what-is-an-operator)
- [Arithmetic Operators](#arithmetic-operators)
- [Comparison Operators](#comparison-operators)
- [Logical Operators](#logical-operators)
- [Assignment Operators](#assignment-operators)
- [Operator Precedence](#operator-precedence)
- [Operator Associativity](#operator-associativity)
- [Short-Circuit Evaluation](#short-circuit-evaluation)
- [Ternary / Conditional Operator](#ternary--conditional-operator)
- [Expressions vs Statements](#expressions-vs-statements)
- [Expression Evaluation](#expression-evaluation)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is an Expression?

An expression is any piece of code that produces (evaluates to) a single value. If you can assign it to a variable, it is an expression.

```python
42                  # literal — evaluates to 42
3 + 4              # arithmetic — evaluates to 7
len("hello")       # function call — evaluates to 5
x > 5              # comparison — True or False
```

```javascript
42                  // 42
3 + 4              // 7
"hello".length     // 5
x > 5              // true or false
```

Every expression has a type determined by its operators and operands. Expressions nest: the result of one becomes the input of another.

```python
result = (2 + 3) * (10 - 4)  # (5) * (6) = 30
```

```javascript
let result = (2 + 3) * (10 - 4);
```

### What Is an Operator?

An operator is a symbol (or keyword) that performs an operation on one or more operands and produces a value. Operators are classified by **arity**:

| Arity | Name | Example |
|-------|------|---------|
| 1 | Unary | `-x`, `!flag` |
| 2 | Binary | `a + b`, `x && y` |
| 3 | Ternary | `cond ? a : b` (JS) |

```python
x = -5             # unary negation
total = price + tax  # binary addition
result = x if cond else y  # ternary
```

```javascript
let x = -5;
let total = price + tax;
let result = cond ? x : y;
```

### Arithmetic Operators

Arithmetic operators work on numeric values and produce a numeric result.

| Operator | Meaning | Python | JavaScript | Java | Go |
|----------|---------|--------|------------|------|----|
| `+` `-` | Add / Subtract | Yes | Yes | Yes | Yes |
| `*` | Multiply | Yes | Yes | Yes | Yes |
| `/` | Division (float) | Yes | Yes | Yes | Yes |
| `//` | Floor division | Yes | No | No | No |
| `%` | Modulus | Yes | Yes | Yes | Yes |
| `**` | Exponentiation | Yes | Yes | No | No |
| `++` `--` | Increment / Decrement | No | Yes | Yes | No |

```python
a, b = 10, 3
print(a + b)   # 13
print(a - b)   # 7
print(a * b)   # 30
print(a / b)   # 3.333 (always float)
print(a // b)  # 3  (floor division — toward negative infinity)
print(a % b)   # 1  (remainder)
print(a ** b)  # 1000
print(-10 // 3)  # -4 (rounds down)
```

```javascript
let a = 10, b = 3;
console.log(a + b);   // 13
console.log(a - b);   // 7
console.log(a * b);   // 30
console.log(a / b);   // 3.333
console.log(a % b);   // 1
console.log(a ** b);  // 1000 (ES2016+)

let count = 5;
count++;   // post-increment — returns 5, then sets 6
++count;   // pre-increment — sets 7, returns 7
```

```java
int a = 10, b = 3;
System.out.println(a / b);  // 3 (integer division truncates toward zero)
System.out.println(a % b);  // 1
```

```go
a, b := 10, 3
fmt.Println(a / b)  // 3 (truncates toward zero)
fmt.Println(a % b)  // 1
```

**Integer division** differs by language: Python `//` floors toward negative infinity; Java and Go truncate toward zero; JavaScript always produces a float.

**Modulus with negatives:** In Python the result has the same sign as the divisor; in JavaScript it has the same sign as the dividend.

```python
print(-10 % 3)  # 2 (Python)
```

```javascript
console.log(-10 % 3);  // -1 (JavaScript)
```

### Comparison Operators

Comparison operators compare two values and produce a boolean result.

| Operator | Meaning | Python | JavaScript |
|----------|---------|--------|------------|
| `==` `!=` | (Loose) equality | Yes | Yes |
| `===` `!==` | Strict equality | No | Yes |
| `<` `>` `<=` `>=` | Relational | Yes | Yes |
| `is` `is not` | Identity | Yes | No |

```python
x = 5
print(x == 5)    # True
print(x != 5)    # False
print(x < 10)    # True
print(x > 10)    # False

# Identity vs equality
a = [1, 2, 3]
b = [1, 2, 3]
print(a == b)   # True  (same value)
print(a is b)   # False (different objects)

# None check
result = None
if result is None:
    print("no result")
```

```javascript
let x = 5;
let y = "5";
console.log(x == y);   // true  (loose — coerces types)
console.log(x === y);  // false (strict — no coercion)
console.log(x != y);   // false
console.log(x !== y);  // true

// Loose equality pitfalls
console.log(0 == false);    // true
console.log("" == false);   // true
console.log(null == undefined); // true

// Always prefer === over ==
```

**Chained comparisons (Python only):**

```python
x = 5
print(1 < x < 10)  # True — equivalent to 1 < x and x < 10
```

JavaScript does not support chaining — `1 < x < 10` evaluates as `(1 < x) < 10`, which coerces the boolean to a number.

**Floating-point comparison:**

```python
print(0.1 + 0.2 == 0.3)  # False
epsilon = 1e-9
print(abs((0.1 + 0.2) - 0.3) < epsilon)  # True
```

```javascript
console.log(0.1 + 0.2 === 0.3);  // false
```

### Logical Operators

Logical operators combine boolean values. They return the value of the last evaluated operand, not necessarily a boolean.

| Operation | Python | JavaScript | Java | Go |
|-----------|--------|------------|------|----|
| AND | `and` | `&&` | `&&` | `&&` |
| OR | `or` | `\|\|` | `\|\|` | `\|\|` |
| NOT | `not` | `!` | `!` | `!` |

```python
a, b = True, False
print(a and b)  # False
print(a or b)   # True
print(not a)    # False

# With truthy/falsy
print(0 and 42)  # 0   (short-circuits on falsy)
print(3 and 42)  # 42  (returns last truthy)
print(0 or 42)   # 42  (returns first truthy)
print(3 or 42)   # 3   (short-circuits on truthy)
```

```javascript
let a = true, b = false;
console.log(a && b);  // false
console.log(a || b);  // true
console.log(!a);      // false

console.log(0 && 42);    // 0
console.log(3 && 42);    // 42
console.log(0 || 42);    // 42
console.log(3 || 42);    // 3

// Default value pattern
let username = input || "Guest";
```

### Assignment Operators

Assignment operators store a value in a variable. The simple `=` is the most common; compound operators perform an operation and assignment in one step.

| Operator | Meaning | Example (x=10) | Result |
|----------|---------|----------------|--------|
| `=` | Assign | `x = 5` | 5 |
| `+=` | Add and assign | `x += 3` | 13 |
| `-=` | Subtract and assign | `x -= 3` | 7 |
| `*=` | Multiply and assign | `x *= 2` | 20 |
| `/=` | Divide and assign | `x /= 2` | 5.0 |
| `%=` | Modulus and assign | `x %= 3` | 1 |
| `**=` | Exponentiate and assign | `x **= 2` | 100 |

```python
x = 10
x += 5   # 15
x -= 3   # 12
x *= 2   # 24
x //= 5  # 4
x **= 3  # 64
```

```javascript
let x = 10;
x += 5;   // 15
x -= 3;   // 12
x *= 2;   // 24
x /= 2;   // 6
x %= 4;   // 2
x **= 2;  // 4
```

**Assignment is an expression in JavaScript** — it returns the assigned value:

```javascript
let x, y, z;
x = y = z = 5;      // all three become 5
console.log(x = 10);  // 10
```

In Python, assignment is a **statement** — it does not return a value:

```python
# x = (y = 5)  # SyntaxError
```

**Assignment vs mutation:**

```python
x = [1, 2, 3]
y = x
y.append(4)     # mutation — affects both
print(x)        # [1, 2, 3, 4]
x = [5, 6, 7]  # assignment — x points to new list
print(y)        # [1, 2, 3, 4] (unchanged)
```

### Operator Precedence

Precedence determines which operator evaluates first when multiple operators appear without explicit parentheses. Higher precedence means "binds tighter."

| Level | Operators | Associativity | Notes |
|-------|-----------|---------------|-------|
| 18 | `()` `[]` `.` | left | Call, subscript, member access |
| 16 | `+` `-` `~` `!` `not` (prefix) | right | Unary |
| 15 | `**` | right | Exponentiation |
| 14 | `*` `/` `//` `%` | left | Multiplicative |
| 13 | `+` `-` | left | Additive |
| 11 | `<` `<=` `>` `>=` `is` `in` | left | Relational |
| 10 | `==` `!=` `===` `!==` | left | Equality |
| 6 | `and` / `&&` | left | Logical AND |
| 5 | `or` / `\|\|` | left | Logical OR |
| 3 | Ternary `? :` / `if else` | right | Conditional |
| 2 | Assignment `=` `+=` etc. | right | Assignment |

```python
result = 2 + 3 * 4 ** 2
# 4**2 → 16  (exp: level 15)
# 3*16 → 48  (mul: level 14)
# 2+48 → 50  (add: level 13)
print(result)  # 50

# Parentheses override
result = (2 + 3) * 4 ** 2  # 5 * 16 = 80
```

```javascript
let result = 2 + 3 * 4 ** 2;  // 50
```

### Operator Associativity

Associativity determines evaluation order when two operators share the same precedence.

**Left-to-right:** most binary operators (arithmetic, comparison, logical):

```python
print(10 - 3 - 2)  # (10 - 3) - 2 = 5
print(100 / 10 / 2)  # (100 / 10) / 2 = 5.0
```

**Right-to-left:** assignment, exponentiation, unary:

```python
print(2 ** 3 ** 2)    # 2 ** (3 ** 2) = 2 ** 9 = 512
x = y = z = 5         # x = (y = (z = 5))
print(- - 5)          # 5 (negative of negative 5)
```

```javascript
console.log(2 ** 3 ** 2);  // 512
```

### Short-Circuit Evaluation

Logical operators `and`/`or` (Python) and `&&`/`||` (JavaScript) short-circuit: they stop evaluating as soon as the result is determined.

**AND short-circuit:** if the left operand is falsy, the right is never evaluated.

```python
def get_user():
    print("called")
    return {"name": "Alice"}

result = None and get_user()
print(result)  # None — get_user() never called

result = True and get_user()
# "called" printed — evaluates to {"name": "Alice"}
```

```javascript
function getUser() {
    console.log("called");
    return { name: "Alice" };
}
const result = null && getUser();
console.log(result);  // null — getUser() never called
```

**OR short-circuit:** if the left operand is truthy, the right is never evaluated.

```python
result = "hello" or expensive()  # "hello" — expensive() never called
result = "" or "default"         # "default"
```

```javascript
const result = "hello" || expensive();  // "hello"
const name = userInput || "Guest";
```

**Chained short-circuit:**

```python
print(0 or "" or None or 42 or "x")  # 42 (first truthy)
print(1 and "a" and 42 and [])       # [] (first falsy)
```

```javascript
console.log(0 || "" || null || 42 || "x");  // 42
console.log(1 && "a" && 42 && []);          // []
```

**Common patterns:**

```python
name = user_input or "Guest"
if user and user.get("name"):
    print(user["name"])
```

```javascript
const name = userInput || "Guest";
if (user && user.name) {
    console.log(user.name);
}
```

### Ternary / Conditional Operator

The ternary operator selects one of two values based on a condition. It is an expression — it produces a value.

**Python:** `value_if_true if condition else value_if_false`

```python
age = 20
status = "adult" if age >= 18 else "minor"
print(status)  # "adult"

# Nesting
score = 85
grade = "A" if score >= 90 else "B" if score >= 80 else "C"
print(grade)  # "B"
```

**JavaScript:** `condition ? value_if_true : value_if_false`

```javascript
let age = 20;
let status = age >= 18 ? "adult" : "minor";
console.log(status);  // "adult"

let score = 85;
let grade = score >= 90 ? "A" : score >= 80 ? "B" : "C";
console.log(grade);  // "B"
```

**Java:**

```java
String status = age >= 18 ? "adult" : "minor";
```

Go has no ternary operator — use if/else.

Ternary is best for simple binary choices. For complex logic, use if/else for readability.

### Expressions vs Statements

The distinction is fundamental. **An expression produces a value.** **A statement performs an action.**

```python
# Expressions (produce values)
42
x + 5
len(items)
x if cond else y

# Statements (perform actions)
x = 42        # assignment
if x > 10:    # if
    print(x)
for i in range(5):  # for
    pass
def f():      # definition
    pass
return x      # return
```

```javascript
// Expressions
42
x + 5
cond ? x : y

// Statements
let x = 42;
if (x > 10) { console.log(x); }
function f() {}
return x;
```

| Aspect | Python | JavaScript | Java | Go |
|--------|--------|------------|------|----|
| Assignment | Statement | Expression | Expression | Statement |
| Ternary | `x if c else y` | `c ? x : y` | `c ? x : y` | None |
| Function def | Statement | Expression | Statement | Statement |
| Function call | Expression | Expression | Expression | Expression |

**Key difference:** assignment in JavaScript is an expression, enabling chained assignment and creating the common `if (a = 5)` bug:

```javascript
if (a = 5) {  // BUG: assignment, not comparison!
    // always runs because 5 is truthy
}
```

### Expression Evaluation

Complex expressions are evaluated step by step according to precedence and associativity. Visualize as an evaluation tree.

**Example:** `(5 + 3) * 2 ** 3 // 4 - 1`

```
       (-)
       / \
     (//) 1
     /   \
   (*)    4
   / \
  (+)   2
  / \    |
 5   3  (**)
         / \
        2   3
```

| Step | Expression | Value | Reason |
|------|-----------|-------|--------|
| 1 | `2 ** 3` | 8 | Exponentiation |
| 2 | `5 + 3` | 8 | Parentheses |
| 3 | `8 * 8` | 64 | Multiplication |
| 4 | `64 // 4` | 16 | Floor division |
| 5 | `16 - 1` | 15 | Subtraction |

```python
result = (5 + 3) * 2 ** 3 // 4 - 1
print(result)  # 15
```

**Decomposition for debugging:**

```python
# Complex expression
result = (a * b + c) / (d - e % f) ** 2

# Broken down:
s1 = e % f
s2 = d - s1
s3 = s2 ** 2
s4 = a * b
s5 = s4 + c
result = s5 / s3
```

## Glossary

| Term | Definition |
|------|------------|
| Expression | Code that produces a value |
| Operator | A symbol that performs an operation on operands |
| Operand | A value that an operator acts upon |
| Unary | An operator with one operand |
| Binary | An operator with two operands |
| Ternary | An operator with three operands |
| Precedence | Rules determining which operator evaluates first |
| Associativity | Order of evaluation when operators have equal precedence |
| Short-circuit | Stopping evaluation early when the result is determined |
| Truthy | A value that evaluates to True in a boolean context |
| Falsy | A value that evaluates to False in a boolean context |
| Coercion | Implicit type conversion |
| Side effect | An operation that modifies program state beyond producing a value |
| Compound assignment | An operator that combines an operation with assignment |
| Modulus | The remainder after division |
| Integer division | Division that produces an integer result |
| Floor division | Division that rounds toward negative infinity |
| Exponentiation | Raising a number to a power |
| Identity | Whether two references point to the same object |
| Equality | Whether two values are considered equal |
| Strict equality | Equality without type coercion |
| Loose equality | Equality with type coercion |
| Overflow | When a computed value exceeds the representable range |
| Precision | The level of detail in a numeric representation |
| Rounding error | The difference between exact and approximate numeric results |

## Quick References

- [Python Operator Documentation](https://docs.python.org/3/reference/expressions.html) — full Python expression and operator reference
- [MDN Expressions and Operators](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators) — JavaScript operators and precedence table
- [Operator Precedence Table](https://en.cppreference.com/w/cpp/language/operator_precedence) — comprehensive precedence reference for C-family languages
- [IEEE 754 Floating Point](https://floating-point-gui.de/) — what every developer should know about floating-point arithmetic
- [JavaScript Equality Table](https://dorey.github.io/JavaScript-Equality-Table/) — visual guide to `==` vs `===` behavior

## Next Steps

- [Advanced Operators](advanced-operators.md) — type-specific operators, bitwise, spread/rest, operator overloading
- [Control Flow](../control-flow.md) — branching and looping with conditions built from expressions
