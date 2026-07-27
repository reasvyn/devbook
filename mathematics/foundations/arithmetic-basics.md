# Arithmetic Basics

## Description

Arithmetic is the foundation upon which every algorithm, data structure, and computation rests. Addition, subtraction, multiplication, and division are not merely childhood exercises — they are the operations that processors execute billions of times per second, that modulo wraps array indices, and that integer division silently truncates results in ways that cause entire systems to fail. This document covers each operation in depth, connects every concept to Python, and equips the reader with the mental models necessary to avoid the most common arithmetic pitfalls in software development.

## Prerequisites

- [Number Sense](number-sense.md) — understanding of integers, the number line, place value, and basic number properties

## Table of Contents

- [Addition](#addition)
- [Subtraction](#subtraction)
- [Multiplication](#multiplication)
- [Division](#division)
- [Order of Operations](#order-of-operations)
- [The Modulo Operation](#the-modulo-operation)
- [Arithmetic in Programming](#arithmetic-in-programming)
- [Common Mistakes](#common-mistakes)
- [Mental Math Strategies](#mental-math-strategies)
- [Learning Tips](#learning-tips)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Addition 🔢

Addition is the combination of two or more quantities into a single sum. It is the most fundamental operation — every other arithmetic operation can be expressed in terms of repeated addition.

$$a + b = c$$

where $a$ and $b$ are **addends** and $c$ is the **sum**.

#### Properties of Addition

| Property | Statement | Example |
|----------|-----------|---------|
| **Commutative** | $a + b = b + a$ | $3 + 7 = 7 + 3 = 10$ |
| **Associative** | $(a + b) + c = a + (b + c)$ | $(2 + 3) + 4 = 2 + (3 + 4) = 9$ |
| **Identity** | $a + 0 = a$ | $15 + 0 = 15$ |
| **Closure** | Sum of two integers is an integer | $7 + (-3) = 4$ |

These properties are not abstract curiosities — they guarantee that the order in which a program sums a list of numbers does not change the result (within floating-point precision limits).

#### Addition in Python

Python uses the `+` operator for addition. The operator works with integers, floats, and also concatenates strings and lists — a form of conceptual addition.

```python
# Integer addition
a = 17
b = 25
result = a + b
print(result)  # 42

# Float addition
x = 3.14
y = 2.86
print(x + y)  # 6.0

# Cumulative addition with +=
total = 0
for value in [10, 20, 30, 40]:
    total += value
print(total)  # 100
```

The `+=` operator is an **augmented assignment**. It adds the right operand to the left operand and assigns the result back to the left operand. In Python, `total += value` is equivalent to `total = total + value` for immutable types. For mutable types like lists, `+=` calls `__iadd__`, which modifies the list in place — a subtle distinction with practical consequences.

```python
# += with lists modifies in place
list_a = [1, 2]
list_b = list_a
list_a += [3]
print(list_a)  # [1, 2, 3]
print(list_b)  # [1, 2, 3] — list_b is affected because it references the same object

# + with lists creates a new list
list_c = [1, 2]
list_d = list_c
list_c = list_c + [3]
print(list_c)  # [1, 2, 3]
print(list_d)  # [1, 2] — list_d is unaffected
```

#### Overflow and Large Numbers

In Python, integers have arbitrary precision — they grow as large as memory allows. This is unlike languages such as C or Java, where `int` types have fixed sizes and can overflow.

```python
# Python handles arbitrarily large integers
large = 10 ** 100  # 1 followed by 100 zeros
print(large + 1)   # 10000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000001
```

This freedom is a luxury not shared by every language. When writing performance-critical code or interacting with lower-level systems, awareness of integer overflow remains essential.

---

### Subtraction ➖

Subtraction finds the difference between two quantities — how much one value exceeds or falls short of another.

$$a - b = c$$

where $a$ is the **minuend**, $b$ is the **subtrahend**, and $c$ is the **difference**.

#### Properties of Subtraction

Unlike addition, subtraction is **not commutative** and **not associative**:

$$a - b \neq b - a \quad \text{(in general)}$$
$$(a - b) - c \neq a - (b - c) \quad \text{(in general)}$$

Subtraction can be understood as the addition of a negative: $a - b = a + (-b)$. This equivalence is why most processors implement subtraction using addition circuits with two's complement representation.

#### Countdown and Ranges

Subtraction powers the concept of counting down — essential for loops that iterate in reverse, stack operations, and timer countdowns.

```python
# Countdown loop
for i in range(10, 0, -1):
    print(i)
print("Liftoff!")

# Subtraction in range: step parameter
# range(start, stop, step) — step can be negative
squares_desc = [x ** 2 for x in range(10, 0, -1)]
print(squares_desc)  # [100, 81, 64, 49, 36, 25, 16, 9, 4, 1]
```

#### Subtraction with Augmented Assignment

The `-=` operator subtracts the right operand from the left and reassigns:

```python
balance = 1000
withdrawal = 250
balance -= withdrawal
print(balance)  # 750

# Chained subtraction
remaining = 100
for cost in [12, 35, 8, 47]:
    remaining -= cost
print(remaining)  # -2 — overspending detected
```

#### Subtraction and Negative Results

When the subtrahend exceeds the minuend, the result is negative. This is not an error — it carries meaningful information (a deficit, a debt, a position below a reference point).

```python
temperature_now = 5
temperature_tomorrow = -3
change = temperature_tomorrow - temperature_now
print(change)  # -8 — a drop of 8 degrees
```

---

### Multiplication ✖️

Multiplication is repeated addition. The expression $a \times b$ means "add $a$ to itself $b$ times" (or vice versa, by the commutative property).

$$a \times b = \underbrace{a + a + \cdots + a}_{b \text{ times}}$$

#### Properties of Multiplication

| Property | Statement | Example |
|----------|-----------|---------|
| **Commutative** | $a \times b = b \times a$ | $4 \times 5 = 5 \times 4 = 20$ |
| **Associative** | $(a \times b) \times c = a \times (b \times c)$ | $(2 \times 3) \times 4 = 2 \times (3 \times 4) = 24$ |
| **Identity** | $a \times 1 = a$ | $17 \times 1 = 17$ |
| **Zero property** | $a \times 0 = 0$ | $999 \times 0 = 0$ |
| **Distributive** | $a \times (b + c) = a \times b + a \times c$ | $3 \times (4 + 5) = 3 \times 4 + 3 \times 5 = 27$ |

The **distributive property** is the bridge between multiplication and addition, and it underpins how compilers optimize expressions and how hardware computes products.

#### Geometric Interpretation: Area

Multiplication models area. A rectangle with width $a$ and height $b$ has area $a \times b$. This geometric intuition extends into computing: a 2D array with 4 rows and 6 columns has $4 \times 6 = 24$ elements.

```python
rows = 4
cols = 6
total_elements = rows * cols
print(total_elements)  # 24
```

#### Multiplication in Python

```python
# Integer multiplication
print(6 * 7)  # 42

# Float multiplication
print(2.5 * 4.0)  # 10.0

# String repetition (conceptual multiplication)
print("=" * 40)  # ========================================

# List repetition
print([0] * 5)  # [0, 0, 0, 0, 0]

# Augmented assignment
count = 10
factor = 3
count *= factor
print(count)  # 30
```

#### Multiplication Tables and Pattern Recognition

Memorizing multiplication tables is not rote drudgery — it is pattern recognition. The patterns in a multiplication table reveal structural regularities useful for mental arithmetic and algorithm design.

```python
# Generate a multiplication table
for i in range(1, 10):
    row = [f"{i * j:3d}" for j in range(1, 10)]
    print(" ".join(row))
```

Notable patterns:
- Multiples of 9 have digits that sum to 9 (e.g., $9 \times 7 = 63$, $6 + 3 = 9$).
- Squares of numbers ending in 5 always end in 25, with the leading digits being $n \times (n+1)$ (e.g., $35^2 = 1225$ because $3 \times 4 = 12$).

---

### Division ➗

Division splits a quantity into equal parts. It is the inverse of multiplication: if $a \times b = c$, then $c \div a = b$ (assuming $a \neq 0$).

$$\frac{a}{b} = c \quad \text{where } a = b \times c$$

where $a$ is the **dividend**, $b$ is the **divisor**, and $c$ is the **quotient**.

#### Division by Zero 🔥

Division by zero is **undefined** in mathematics. In Python, it raises a `ZeroDivisionError` — a runtime exception that halts execution unless caught.

```python
# This will crash
result = 10 / 0  # ZeroDivisionError: division by zero

# Defensive programming: check before dividing
def safe_divide(a, b):
    if b == 0:
        return None  # or raise a custom exception
    return a / b

print(safe_divide(10, 0))   # None
print(safe_divide(10, 3))   # 3.3333333333333335
```

Division by zero is not merely a theoretical concern. It is one of the most common causes of runtime crashes in production software. Defensive checks and input validation are non-negotiable practices.

#### Float Division vs. Integer Division

Python provides two division operators:

| Operator | Name | Behavior | Example |
|----------|------|----------|---------|
| `/` | True division | Always returns a float | `7 / 2 = 3.5` |
| `//` | Floor division | Returns the floor of the quotient | `7 // 2 = 3` |

```python
# True division (/) — always float
print(10 / 3)    # 3.3333333333333335
print(10 / 2)    # 5.0 — still a float
print(10 / -3)   # -3.3333333333333335

# Floor division (//) — rounds toward negative infinity
print(7 // 2)    # 3
print(-7 // 2)   # -4 — NOT -3 (this surprises many developers)
print(10 // 3)   # 3
```

The critical distinction is the behavior with negative numbers. Floor division rounds toward **negative infinity**, not toward zero. This means `-7 // 2` yields `-4`, not `-3`. This behavior follows the mathematical definition of floor but contradicts the truncation behavior of many other languages.

#### Rounding

When a result has more decimal places than needed, rounding selects the nearest approximation.

```python
# Python's round() uses banker's rounding (round half to even)
print(round(2.5))    # 2 — rounds to even
print(round(3.5))    # 4 — rounds to even
print(round(2.4))    # 2
print(round(2.6))    # 3

# Rounding to specific decimal places
pi = 3.141592653589793
print(round(pi, 2))  # 3.14
print(round(pi, 4))  # 3.1416
```

Banker's rounding (round half to even) reduces cumulative rounding bias in large datasets — a property important in financial and statistical computing.

#### The Divmod Function

Python provides `divmod()`, which returns both the quotient and remainder in a single call — useful when both values are needed.

```python
q, r = divmod(17, 5)
print(q)  # 3
print(r)  # 2

# Practical use: converting seconds to hours, minutes, seconds
total_seconds = 3661
hours, remainder = divmod(total_seconds, 3600)
minutes, seconds = divmod(remainder, 60)
print(f"{hours}h {minutes}m {seconds}s")  # 1h 1m 1s
```

---

### Order of Operations 🧮

When an expression contains multiple operations, a convention determines which operation is performed first. Without this convention, $2 + 3 \times 4$ could equal either $20$ or $14$.

#### PEMDAS / BODMAS

The standard convention is encoded in the acronyms **PEMDAS** (US) and **BODMAS** (UK/Commonwealth):

| Priority | PEMDAS | BODMAS | Description |
|----------|--------|--------|-------------|
| 1 (highest) | **P**arentheses | **B**rackets | Expressions inside grouping symbols first |
| 2 | **E**xponents | **O**rders | Powers, roots, exponents |
| 3 | **M**ultiplication | **D**ivision | Left to right |
| 3 | **D**ivision | **M**ultiplication | Left to right |
| 4 | **A**ddition | **A**ddition | Left to right |
| 4 | **S**ubtraction | **S**ubtraction | Left to right |

Multiplication and division share equal priority (evaluated left to right), as do addition and subtraction.

#### Examples

```python
# Without parentheses: left-to-right evaluation for same-priority operators
print(2 + 3 * 4)     # 14 — multiplication first
print((2 + 3) * 4)   # 20 — parentheses force addition first
print(12 / 4 + 2)    # 5.0 — division first: 3.0 + 2
print(12 / (4 + 2))  # 2.0 — parentheses force addition first
print(2 ** 3 ** 2)    # 512 — exponentiation is right-associative: 2 ** (3 ** 2) = 2 ** 9
print((2 ** 3) ** 2)  # 64 — explicit left-associative
```

#### Why Order of Operations Matters in Code

Ambiguous expressions are a source of subtle bugs. The safest practice is to use parentheses liberally — not because the compiler needs them, but because future readers (including yourself) do.

```python
# Ambiguous — works correctly but reads poorly
result = a + b * c - d / e

# Clear — intent is explicit
result = a + (b * c) - (d / e)
```

PEMDAS is a convention, not a law of nature. Misremembering it or assuming it applies differently than it does is a frequent source of errors in both manual calculations and code review.

---

### The Modulo Operation 🔄

The modulo operation computes the **remainder** after integer division.

$$a \mod b = a - b \times \lfloor a / b \rfloor$$

In Python, the modulo operator is `%`.

```python
print(17 % 5)    # 2 — 17 = 5*3 + 2
print(10 % 2)    # 0 — 10 is even
print(7 % 3)     # 1
print(-7 % 3)    # 2 — Python's modulo is always non-negative when the divisor is positive
print(7 % -3)    # -2 — sign follows the divisor
```

Python's modulo follows the divisor's sign, which differs from the behavior in languages like C and Java. This distinction is critical when porting code between languages.

#### Even and Odd Checks

The most common use of modulo is testing divisibility:

```python
def is_even(n):
    return n % 2 == 0

print(is_even(4))   # True
print(is_even(7))   # False
print(is_even(0))   # True
print(is_even(-4))  # True
```

#### Wrapping and Circular Indices

Modulo enables **circular** or **wraparound** behavior — when a value exceeds a boundary, it loops back to the beginning.

```python
# Days of the week
days = ["Mon", "Tue", "Wed", "Thu", "Fri", "Sat", "Sun"]
today_index = 2  # Wednesday
days_ahead = 10
future_index = (today_index + days_ahead) % len(days)
print(days[future_index])  # "Sat"

# Circular buffer
buffer_size = 4
for i in range(10):
    position = i % buffer_size
    print(f"Write to position {position}")
# Positions cycle: 0, 1, 2, 3, 0, 1, 2, 3, 0, 1
```

#### Modulo in Hashing

Hash functions use modulo to map arbitrary keys into a fixed range of bucket indices:

```python
def simple_hash(key, table_size):
    return hash(key) % table_size

# Two keys that collide
print(simple_hash("apple", 10))
print(simple_hash("elppa", 10))
# May produce the same index — this is a hash collision
```

---

### Arithmetic in Programming 🖥️

This section consolidates the operations discussed above into practical programming patterns.

#### Summing a List

```python
numbers = [4, 8, 15, 16, 23, 42]

# Manual accumulation
total = 0
for n in numbers:
    total += n
print(total)  # 108

# Built-in sum()
print(sum(numbers))  # 108

# sum() with a starting value (useful for offsets)
running_balance = 100
transactions = [-20, -15, +50, -10]
final = sum(transactions, running_balance)
print(final)  # 105
```

#### Computing Averages

```python
def compute_average(values):
    if not values:
        return 0
    return sum(values) / len(values)

temperatures = [22.5, 24.0, 19.5, 21.0, 23.5]
avg = compute_average(temperatures)
print(f"Average temperature: {avg:.1f}°C")  # 22.1°C
```

#### Factorial Calculation

Factorial demonstrates multiplication in a loop — foundational for combinatorics, probability, and algorithm analysis.

```python
def factorial(n):
    if n < 0:
        raise ValueError("Factorial undefined for negative numbers")
    result = 1
    for i in range(2, n + 1):
        result *= i
    return result

print(factorial(5))  # 120 — 5 * 4 * 3 * 2 * 1
print(factorial(0))  # 1 — the empty product
```

#### Tip Calculation

A practical example combining division, multiplication, and rounding:

```python
def calculate_tip(bill, tip_percent, num_people):
    tip = bill * (tip_percent / 100)
    total = bill + tip
    per_person = total / num_people
    return round(per_person, 2)

each = calculate_tip(85.50, 18, 4)
print(f"Each person pays: ${each:.2f}")  # Each person pays: $25.22
```

#### Pixel Coordinate Arithmetic

Game development and graphics programming rely heavily on integer arithmetic:

```python
# Grid-based movement
player_x, player_y = 3, 5
direction = "right"
step = 1

if direction == "right":
    player_x += step
elif direction == "left":
    player_x -= step

# Wrapping around a grid (toroidal)
grid_width, grid_height = 10, 10
player_x = (player_x + step) % grid_width
player_y = (player_y + step) % grid_height
```

#### Bit-Level Arithmetic

Though Python abstracts away most bit-level concerns, understanding how arithmetic maps to binary is essential for systems programming and optimization.

```python
# Powers of 2
for i in range(8):
    print(f"2^{i} = {2 ** i}")

# Bit shifting as multiplication/division by 2
n = 5
print(n << 1)  # 10 — equivalent to n * 2
print(n << 3)  # 40 — equivalent to n * 2^3 = n * 8
print(n >> 1)  # 2  — equivalent to n // 2 (floor)
```

---

### Common Mistakes ⚠️

Understanding where arithmetic goes wrong is as important as understanding how it works. The following pitfalls are among the most frequently encountered in software development.

#### Off-by-One Errors

Off-by-one errors occur when a loop iterates one too many or one too few times, typically due to misunderstanding the boundary conditions of a range.

```python
# Bug: processes 9 items instead of 10
items = list(range(10))
for i in range(1, 10):  # Should be range(10) or range(1, 11)
    print(items[i])

# Correct: use len() and range()
for i in range(len(items)):
    print(items[i])
```

The root cause is a mismatch between the mathematical interval (which is continuous) and the programming range (which is discrete). Always verify whether your range is inclusive or exclusive at each end.

#### Integer Division Truncation

In Python 3, the `//` operator floors the result rather than truncating toward zero. In Python 2, `/` performed integer division when both operands were integers. Code ported between versions may silently change behavior.

```python
# Python 3
print(7 // 2)     # 3
print(-7 // 2)    # -4 (floors toward negative infinity)

# In C, Java, JavaScript, truncation toward zero:
# -7 / 2 == -3 (not -4)
```

If truncation toward zero is desired in Python:

```python
import math
print(math.trunc(-7 / 2))  # -3
print(int(-7 / 2))         # -3
```

#### Float Precision

Floating-point numbers cannot represent all decimal values exactly. This leads to surprising results:

```python
print(0.1 + 0.2)           # 0.30000000000000004
print(0.1 + 0.2 == 0.3)    # False

# Solution: compare with a tolerance
def approx_equal(a, b, tol=1e-9):
    return abs(a - b) < tol

print(approx_equal(0.1 + 0.2, 0.3))  # True
```

For precise decimal arithmetic (financial calculations, currency), use Python's `decimal` module:

```python
from decimal import Decimal, getcontext
getcontext().prec = 10

a = Decimal("0.1")
b = Decimal("0.2")
print(a + b)        # 0.3
print(a + b == Decimal("0.3"))  # True
```

#### Sign Errors in Modulo

Python's `%` operator follows the divisor's sign, which differs from C, Java, and JavaScript:

```python
# Python
print(-7 % 3)   # 2 (non-negative result because divisor is positive)
print(7 % -3)   # -2 (sign follows divisor)

# C / Java / JavaScript
# -7 % 3 == -1 (sign follows the dividend)
```

When porting algorithms that rely on modulo (hashing, circular buffers, calendar calculations), verify the sign behavior of the target language.

#### Chained Comparisons Misinterpreted

Python supports chained comparisons (`a < b < c`), but developers accustomed to other languages may write incorrect expressions:

```python
# Correct Python chained comparison
x = 5
print(0 < x < 10)  # True — equivalent to (0 < x) and (x < 10)

# Incorrect assumption from C-like languages
# In C: 0 < x < 10 evaluates as (0 < x) < 10, which is always True/False < 10
```

#### Mixed-Type Arithmetic

Mixing integers and floats can produce unexpected results:

```python
# Integer division vs float division
print(10 / 3)      # 3.3333333333333335
print(10 // 3)     # 3
print(10.0 // 3)   # 3.0

# Large integers lose precision when converted to float
big = 10 ** 20
print(float(big))  # 1e+20 — exact value lost
print(float(big) == big + 1)  # True — precision loss
```

---

### Mental Math Strategies 🧠

Mental arithmetic is not a parlor trick — it is a debugging skill. When a developer can estimate results without a calculator, they can instantly detect when a computation has gone wrong.

#### Decomposition

Break complex problems into simpler parts:

```python
# 47 + 68 = (47 + 70) - 2 = 117 - 2 = 115
# 15% of 80 = 10% + 5% = 8 + 4 = 12
```

#### Rounding and Adjusting

Round to a nearby "nice" number, then correct the difference:

```python
# 198 * 5 = 200 * 5 - 2 * 5 = 1000 - 10 = 990
# 997 + 346 = 1000 + 346 - 3 = 1343
```

#### Recognizing Multiples

Knowing common multiples accelerates mental calculation:

| Number | Recognizable Pattern |
|--------|---------------------|
| 2 | Last digit is even |
| 3 | Digit sum is divisible by 3 |
| 4 | Last two digits divisible by 4 |
| 5 | Last digit is 0 or 5 |
| 9 | Digit sum is divisible by 9 |
| 10 | Last digit is 0 |
| 11 | Alternating digit sum is 0 or divisible by 11 |

```python
def is_divisible_by_3(n):
    return sum(int(d) for d in str(abs(n))) % 3 == 0

print(is_divisible_by_3(123))  # True — 1+2+3 = 6, divisible by 3
print(is_divisible_by_3(124))  # False — 1+2+4 = 7, not divisible by 3
```

#### Estimation

Rough estimation catches major errors before they propagate:

```python
# Before running a precise calculation, estimate:
# "I have 1,024 items, each 3.99 each. That's roughly 4,000."
precise = 1024 * 3.99
print(precise)  # 4085.76 — close to the estimate of 4,000

# If the precise answer were 40,000, something would be wrong.
```

#### Percentage Mental Shortcuts

```python
# 15% tip: calculate 10%, then add half
bill = 60.00
tip_10 = bill / 10     # 6.00
tip_15 = tip_10 + tip_10 / 2  # 9.00

# To find what percent A is of B:
# (A / B) * 100
# "What percent of 240 is 48?" → 48/240 = 1/5 = 20%
```

---

## Learning Tips

- **🔧 Practice with code, not paper.** Every arithmetic concept in this document can be verified with a three-line Python script. Write the script. Confirm the result. The feedback loop builds intuition faster than mental calculation alone.
- **🔍 Trace edge cases.** What happens when you add the largest possible integer to itself? When you divide the smallest positive float by 2? When you modulo by 1? These edge cases reveal how the operations actually behave under stress.
- **📐 Use the number line.** Subtraction and negative results become intuitive when visualized on a number line. Draw one when debugging sign-related logic.
- **⏱️ Estimate before computing.** Before running a calculation, predict the order of magnitude. If the result deviates wildly from the prediction, investigate immediately.
- **🧩 Memorize powers of 2.** The sequence 1, 2, 4, 8, 16, 32, 64, 128, 256, 512, 1024, 2048, 4096 appears everywhere in computing — array sizes, memory allocation, hash tables, subnet masks.
- **⚠️ Respect floating-point.** Never use `==` to compare floats. Always compare with a tolerance. This single habit eliminates an entire category of bugs.

## Glossary

| Term | Definition |
|------|------------|
| Addend | A number that is added to another in addition |
| Augmented assignment | An operator that combines an arithmetic operation with assignment (e.g., `+=`, `-=`) |
| Commutative property | The property that $a + b = b + a$ or $a \times b = b \times a$ |
| Distributive property | The property that $a \times (b + c) = a \times b + a \times c$ |
| Dividend | The number being divided in a division operation |
| Divisor | The number by which another is divided |
| Floor division | Division that rounds the quotient toward negative infinity (`//` in Python) |
| Identity element | A value that does not change another when combined with it (0 for addition, 1 for multiplication) |
| Integer | A whole number — positive, negative, or zero, with no fractional component |
| Modulo | The remainder after dividing one integer by another (`%` in Python) |
| Multiplicand | A number that is multiplied by another |
| Off-by-one error | A bug caused by iterating one too many or one too few times |
| Operator precedence | The set of rules determining the order in which operations are evaluated |
| Overflow | A condition where a number exceeds the maximum representable value for its data type |
| PEMDAS/BODMAS | Acronyms encoding the order of operations convention |
| Product | The result of multiplication |
| Quotient | The result of division |
| Remainder | The integer left over after floor division |
| Subtrahend | The number being subtracted |
| Sum | The result of addition |
| True division | Division that always returns a float (`/` in Python) |

## Quick References

- [Khan Academy — Arithmetic](https://www.khanacademy.org/math/arithmetic) — comprehensive lessons on all four operations and order of operations
- [Khan Academy — Order of Operations](https://www.khanacademy.org/math/cc-sixth-grade-math/cc-6th-arithmetic-operations) — PEMDAS explained with practice exercises
- [Python Documentation — Expressions](https://docs.python.org/3/reference/expressions.html#operator-precedence) — official operator precedence table for Python
- [Floating-Point Arithmetic (What Every Programmer Should Know)](https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html) — the definitive guide to IEEE 754 floating-point arithmetic
- [Python `decimal` Module](https://docs.python.org/3/library/decimal.html) — precise decimal arithmetic for financial calculations

## Next Steps

- [Fractions and Decimals](fractions-and-decimals.md) — work with parts of wholes, rational numbers, and decimal representations
- [Math for Debugging](math-for-debugging.md) — apply arithmetic reasoning to diagnose and resolve software defects
