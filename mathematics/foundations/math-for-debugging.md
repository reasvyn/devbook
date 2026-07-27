# Math for Debugging

## Description

Mathematical thinking is the debugger's most powerful instrument. Bugs rarely announce themselves — they hide at the edges of inputs, in the gaps between what the programmer assumed and what the machine actually computes. Understanding boundary conditions, numeric representation, and formal reasoning transforms debugging from guesswork into a disciplined investigation. This document examines how mathematical concepts explain, predict, and resolve the most common categories of software defects.

## Prerequisites

- [Arithmetic Basics](arithmetic-basics.md) — fundamental operations, order of operations, and integer arithmetic
- [Fractions and Decimals](fractions-and-decimals.md) — rational numbers, decimal representation, and precision

## Table of Contents

- [Why Mathematical Thinking Matters for Debugging](#why-mathematical-thinking-matters-for-debugging)
- [Boundary Conditions](#-boundary-conditions)
- [Off-By-One Errors](#-off-by-one-errors)
- [Overflow and Underflow](#-overflow-and-underflow)
- [Integer Division Traps](#-integer-division-traps)
- [Float Comparison Pitfalls](#-float-comparison-pitfalls)
- [Modular Arithmetic in Debugging](#-modular-arithmetic-in-debugging)
- [Trace Tables](#-trace-tables)
- [Using Print Debugging to Verify Mathematical Assumptions](#-using-print-debugging-to-verify-mathematical-assumptions)
- [Learning Tips](#learning-tips)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Why Mathematical Thinking Matters for Debugging

Every bug is, at its root, a violation of an expectation. The programmer expected a certain relationship between inputs and outputs, and that relationship failed. Mathematics provides the language for stating those relationships precisely — and for recognizing exactly where they break down.

Consider a function that is supposed to compute the average of a list of numbers. The mathematical definition is clear:

$$\text{average} = \frac{\sum_{i=1}^{n} x_i}{n}$$

But the code must also answer: what if $n = 0$? What if the list contains negative numbers? What if the sum overflows? Each of these questions has a mathematical answer, and each leads to a potential bug if ignored.

Mathematical thinking in debugging means:

- **Reasoning about edge cases** before they cause failures
- **Tracing variable values** through each step of an algorithm
- **Understanding numeric representation** to predict how computers handle numbers
- **Applying formal logic** to construct proof-like arguments about code correctness

The discipline of thinking precisely about numbers, ranges, and operations is not merely academic — it is the foundation of every reliable program. A programmer who can reason mathematically about code can predict where defects will arise before writing a single test. This capacity for preemptive analysis is what separates reactive debugging from proactive quality engineering.

### 🔲 Boundary Conditions

Bugs hide at boundaries. A function that works perfectly for typical inputs often fails at the extremes: the smallest possible value, the largest possible value, zero, empty collections, or single-element lists. The mathematical reason is straightforward — boundaries are where the behavior of a system changes character.

**Why boundaries are dangerous:**

| Boundary | What changes | Typical bug |
|----------|-------------|-------------|
| Empty input ($n = 0$) | Division becomes undefined | ZeroDivisionError |
| Single element | Loops execute zero or one iteration | Off-by-one |
| Maximum value | Next increment wraps around | Overflow |
| Minimum value | Negation may overflow | Undefined behavior |
| Negative input | Parity, sign, and modular behavior change | Unexpected results |

The set of "typical" inputs occupies the interior of the valid range, where behavior is smooth and predictable. The boundaries are measure-zero points in the input space, yet they account for a disproportionate share of production defects. This is because the interior is tested implicitly by representative examples, while boundaries require deliberate, targeted investigation.

**Debugging Scenario 1: The Empty List**

The following function computes the median of a list. It works correctly for populated lists, but crashes on empty input:

```python
# BUGGY VERSION
def median(numbers):
    sorted_nums = sorted(numbers)
    n = len(sorted_nums)
    mid = n // 2
    if n % 2 == 0:
        return (sorted_nums[mid - 1] + sorted_nums[mid]) / 2
    else:
        return sorted_nums[mid]

# This works correctly:
print(median([3, 1, 4, 1, 5]))  # Returns 3

# This crashes:
print(median([]))  # IndexError: list index out of range
```

**The fix** adds a guard for the boundary condition:

```python
# FIXED VERSION
def median(numbers):
    if not numbers:
        raise ValueError("Cannot compute median of empty list")
    sorted_nums = sorted(numbers)
    n = len(sorted_nums)
    mid = n // 2
    if n % 2 == 0:
        return (sorted_nums[mid - 1] + sorted_nums[mid]) / 2
    else:
        return sorted_nums[mid]
```

The mathematical insight is that the definition of median requires $n \geq 1$. The function must enforce this precondition. In formal terms, the domain of the median function is $\mathbb{R}^n$ for $n \geq 1$, not $\mathbb{R}^0$.

**Boundary testing checklist:**

- [ ] Empty input (0 elements)
- [ ] Single element (1 element)
- [ ] Two elements (minimum for pairwise logic)
- [ ] Maximum representable value
- [ ] Minimum representable value (most negative)
- [ ] Zero where positive/negative behavior diverges

### ➖ Off-By-One Errors

The off-by-one error is the most common mathematical bug in programming. It arises from a confusion between inclusive and exclusive ranges — the difference between counting $n$ items and counting from index $0$ to index $n-1$.

**The mathematical root:**

A sequence of $n$ consecutive integers starting at $a$ is:

$$a, \; a+1, \; a+2, \; \ldots, \; a+n-1$$

The last element is $a + n - 1$, not $a + n$. This seems obvious in the abstract, but it becomes confusing when loops use `<` versus `<=`, when array indexing starts at 0 instead of 1, and when ranges are specified with start/end versus start/count.

**Debugging Scenario 2: Processing Every Element**

A function that is supposed to uppercase every character in a string misses the final character:

```python
# BUGGY VERSION
def uppercase_all(text):
    result = list(text)
    for i in range(len(text) - 1):  # BUG: stops one element early
        result[i] = result[i].upper()
    return "".join(result)

print(uppercase_all("hello"))  # Returns "HellO" — the final 'o' remains lowercase
```

**Trace through the loop:**

| Iteration | `i` | `len(text) - 1` | Condition `i < len(text) - 1` | Action |
|-----------|-----|-------------------|-------------------------------|--------|
| 1 | 0 | 4 | True | uppercase `h` |
| 2 | 1 | 4 | True | uppercase `e` |
| 3 | 2 | 4 | True | uppercase `l` |
| 4 | 3 | 4 | True | uppercase `l` |
| 5 | 4 | 4 | False | loop ends — `o` untouched |

**The fix** uses the correct range:

```python
# FIXED VERSION
def uppercase_all(text):
    result = list(text)
    for i in range(len(text)):  # correct: iterate over all indices
        result[i] = result[i].upper()
    return "".join(result)

print(uppercase_all("hello"))  # Returns "HELLO"
```

**The fencepost analogy:**

If a fence is 10 meters long and a post is placed every meter, 11 posts are needed — one at each end plus one at the start. The number of posts is $n + 1$ for $n$ segments. This "fencepost" confusion is the same mathematical error that produces off-by-one bugs:

- $n$ elements require indices $0$ through $n - 1$
- $n$ iterations require a loop from $0$ to $n - 1$
- Python's `range(n)` produces exactly $0, 1, \ldots, n-1$

**Common off-by-one patterns:**

| Pattern | Correct | Buggy |
|---------|---------|-------|
| Iterate all elements | `range(len(items))` | `range(len(items) - 1)` |
| Iterate including endpoint | `range(start, end + 1)` | `range(start, end)` |
| Copy array up to index $k$ | `arr[:k+1]` | `arr[:k]` |
| Halving a search range | `mid = (lo + hi) // 2` | Using `<=` instead of `<` in binary search |

### 📈 Overflow and Underflow

Every numeric type in a computer has finite bounds. **Overflow** occurs when a computation produces a value larger than the maximum representable value. **Underflow** occurs when a value is smaller in magnitude than the minimum representable positive value.

**Integer overflow in Python:**

Python integers have arbitrary precision — they grow as large as needed. This means Python does not produce integer overflow. However, many other languages do, and understanding overflow is essential when working across language boundaries or using NumPy arrays with fixed-width integer types.

```python
import sys
import numpy as np

# Python native integers: no overflow
x = 2**63
print(x)          # 9223372036854775808 — correct
print(x + 1)      # 9223372036854775809 — still correct

# NumPy fixed-width integers: overflow occurs
arr = np.array([2**63], dtype=np.int64)
print(arr[0])     # -9223372036854775808 — overflow to negative!
```

**Float overflow and underflow in Python:**

Floating-point numbers use a fixed number of bits (64 bits in IEEE 754 double precision), so they do have limits:

```python
import sys

# Float overflow
big = sys.float_info.max
print(big)         # 1.7976931348623157e+308
print(big * 2)     # inf — overflow to infinity

# Float underflow
small = sys.float_info.min
print(small)       # 2.2250738585072014e-308
print(small / 2)   # 1.1125047929256007e-308 — still representable
print(small / 1e308)  # 0.0 — underflow to zero
```

**Debugging Scenario 3: Overflow in a Counter**

A system tracks concurrent users with a 32-bit signed counter. During high traffic, the counter wraps to a negative number, corrupting downstream logic:

```python
# BUGGY VERSION (simulating 32-bit signed integer overflow)
import ctypes

def increment_counter(counter):
    counter = ctypes.c_int32(counter.value + 1)
    return counter

counter = ctypes.c_int32(2_147_483_647)  # max 32-bit signed int
print(counter.value)    # 2147483647

counter = increment_counter(counter)
print(counter.value)    # -2147483648 — overflow! Counter is now negative
```

**The fix** uses a bounds check or an appropriately large type:

```python
# FIXED VERSION
MAX_INT32 = 2_147_483_647

def increment_counter(counter):
    if counter >= MAX_INT32:
        raise OverflowError("Counter has reached maximum value")
    return counter + 1
```

**IEEE 754 float representation:**

A 64-bit float is divided into three fields:

| Field | Bits | Purpose |
|-------|------|---------|
| Sign | 1 | 0 = positive, 1 = negative |
| Exponent | 11 | Scale factor (biased by 1023) |
| Mantissa | 52 | Significant digits (significand) |

The smallest positive normal number is approximately $2.2 \times 10^{-308}$, and the largest is approximately $1.8 \times 10^{308}$. Values outside this range become `inf` or `0.0`.

**Underflow in cumulative calculations:**

```python
# BUGGY VERSION
product = 1.0
for i in range(1000):
    product *= 0.999
print(product)  # 0.0 — all precision lost to underflow

# FIXED VERSION: work in log space to avoid underflow
import math
log_sum = 0.0
for i in range(1000):
    log_sum += math.log(0.999)
result = math.exp(log_sum)
print(result)   # 0.3677 — correct value preserved
```

### ➗ Integer Division Traps

In many programming languages, dividing two integers produces an integer — the fractional part is discarded. This is mathematically surprising: $3 \div 2$ equals $1.5$, not $1$.

**Python's division operators:**

| Operator | Expression | Result | Description |
|----------|-----------|--------|-------------|
| `/` | `7 / 2` | `3.5` | True division (always returns float) |
| `//` | `7 // 2` | `3` | Floor division (truncates toward $-\infty$) |
| `%` | `7 % 2` | `1` | Modulo (remainder) |

Python's `//` floors toward negative infinity, not toward zero. This produces counterintuitive results with negative operands:

```python
print(7 // 2)     #  3  (floor of 3.5)
print(-7 // 2)    # -4  (floor of -3.5, not -3)
print(7 // -2)    # -4  (floor of -3.5)
print(-7 // -2)   #  3  (floor of 3.5)
```

**Debugging Scenario 4: Average Calculation**

A gradebook program computes a student's average score. Integer division truncates the result, producing incorrect grades:

```python
# BUGGY VERSION
def average_score(scores):
    total = sum(scores)
    count = len(scores)
    return total // count  # BUG: integer division discards fractional part

scores = [85, 92, 78, 90, 88]
print(average_score(scores))  # Returns 86 — should be 86.6
```

**The fix** uses true division:

```python
# FIXED VERSION
def average_score(scores):
    if not scores:
        raise ValueError("Cannot compute average of empty list")
    total = sum(scores)
    count = len(scores)
    return total / count  # true division returns float

scores = [85, 92, 78, 90, 88]
print(average_score(scores))  # Returns 86.6
```

**The mathematical principle:**

The set of integers is not closed under division. For $a, b \in \mathbb{Z}$ with $b \neq 0$, the quotient $a/b$ is generally not an integer. The floor operation $\lfloor a/b \rfloor$ maps the result back to an integer by discarding information. This information loss is the root cause of the bug.

**When floor division is correct:**

- Counting complete groups (how many full hours in 90 minutes? $\lfloor 90/60 \rfloor = 1$)
- Array indexing from a linear position (`row = index // width`)
- Digit extraction (hundreds digit of 4523: $\lfloor 4523/100 \rfloor \bmod 10 = 5$)

**When floor division is a bug:**

- Computing averages, rates, or ratios
- Any context where the fractional part carries semantic meaning
- Currency calculations (always use `decimal.Decimal`, never float)

### 🔬 Float Comparison Pitfalls

Floating-point numbers are approximations of real numbers, stored in binary. Two values that should be mathematically equal may differ by a tiny amount due to rounding errors. Comparing them with `==` then produces unexpected `False`.

**Why floats are imprecise:**

The decimal fraction $0.1$ cannot be represented exactly in binary:

$$0.1_{10} = 0.0001100110011\ldots_{2} \text{ (repeating)}$$

This is analogous to how $\frac{1}{3} = 0.333\ldots$ cannot be represented exactly in decimal. The computer stores a rounded approximation, and successive arithmetic operations accumulate rounding errors.

**Debugging Scenario 5: The Equality Trap**

A financial application compares two calculated totals and finds they are not equal, even though they should be:

```python
# BUGGY VERSION
price = 0.1
quantity = 3
total = price * quantity
expected = 0.3

print(total)             # 0.30000000000000004
print(total == expected) # False — unexpected inequality
```

**Trace through the binary representation:**

| Value | Decimal | Binary (52-bit mantissa, truncated) | Stored as |
|-------|---------|--------------------------------------|-----------|
| `0.1` | 0.1 | `0.0001100110011...011` | Rounded |
| `0.1 * 3` | 0.3 | `0.0100110011001...100` | Rounded (different bits) |
| `0.3` | 0.3 | `0.0100110011001...101` | Rounded (yet another pattern) |

Three different binary representations for what should be the same decimal value.

**The fix** uses tolerance-based comparison:

```python
# FIXED VERSION
import math

def float_equal(a, b, rel_tol=1e-9, abs_tol=1e-9):
    return math.isclose(a, b, rel_tol=rel_tol, abs_tol=abs_tol)

price = 0.1
quantity = 3
total = price * quantity
expected = 0.3

print(float_equal(total, expected))  # True
```

The `math.isclose` function checks whether $|a - b| \leq \max(\text{rel\_tol} \times \max(|a|, |b|), \text{abs\_tol})$. This is the mathematically correct way to compare floating-point numbers, accounting for the fact that rounding errors scale with the magnitude of the operands.

**Float comparison rules:**

| Comparison method | When to use | Example |
|------------------|-------------|---------|
| Exact equality `==` | Only for integers, known-exact values, or bit-identical floats | `3.0 == 3.0` |
| Relative tolerance | For values of similar magnitude far from zero | `math.isclose(a, b, rel_tol=1e-9)` |
| Absolute tolerance | For values near zero | `math.isclose(a, b, abs_tol=1e-12)` |
| Never compare | Subtracting nearly equal large numbers loses precision | Catastrophic cancellation |

### 🔢 Modular Arithmetic in Debugging

Modular arithmetic — arithmetic that wraps around at a fixed modulus — appears in many debugging scenarios: time calculations, circular buffers, hash tables, and scheduling algorithms.

**Definition:**

For integers $a$ and $n > 0$, the expression $a \bmod n$ yields the remainder when $a$ is divided by $n$. Formally:

$$a \bmod n = a - n \left\lfloor \frac{a}{n} \right\rfloor$$

The result is always in the range $[0, n-1]$.

**Debugging Scenario 6: The Wrapping Clock**

A scheduling system tracks which worker is on duty using a rotating assignment. Workers are numbered 0 through 4. After worker 4, the cycle returns to worker 0. A bug assigns worker $-1$:

```python
# BUGGY VERSION
def current_worker(hour, num_workers=5):
    return hour % num_workers - 1  # BUG: subtraction after modulo

for h in range(7):
    print(f"Hour {h}: Worker {current_worker(h)}")
# Hour 0: Worker -1  ← invalid index!
# Hour 1: Worker 0
# Hour 2: Worker 1
# Hour 3: Worker 2
# Hour 4: Worker 3
# Hour 5: Worker 4
# Hour 6: Worker 0  ← should be Worker 1
```

**The fix** removes the erroneous subtraction:

```python
# FIXED VERSION
def current_worker(hour, num_workers=5):
    return hour % num_workers  # modulus already produces correct wrap

for h in range(7):
    print(f"Hour {h}: Worker {current_worker(h)}")
# Hour 0: Worker 0
# Hour 1: Worker 1
# Hour 2: Worker 2
# Hour 3: Worker 3
# Hour 4: Worker 4
# Hour 5: Worker 0  ← correct wrap
# Hour 6: Worker 1
```

**Modular arithmetic patterns in debugging:**

| Pattern | Expression | Use case |
|---------|-----------|----------|
| Wrap-around index | `i % n` | Circular buffer, round-robin scheduling |
| Even/odd check | `x % 2 == 0` | Alternating behavior, parity tests |
| Divisibility test | `x % k == 0` | Batch processing, periodic triggers |
| Digit extraction | `(n // 10**k) % 10` | Parsing individual decimal digits |
| Time wrapping | `seconds % 60` | Clock displays, countdown timers |

**Negative modulo across languages:**

Python's `%` operator returns a non-negative result when the divisor is positive, even for negative dividends. This differs from C and Java:

```python
# Python
print(-1 % 5)    # 4  (correct modular result)
print(-7 % 3)    # 2  (because -7 = -3 * 3 + 2)

# C / Java
# -1 % 5  →  -1  (different convention!)
# -7 % 3  →  -1
```

When debugging cross-language code, this divergence in modular arithmetic semantics is a frequent source of confusion.

### 📋 Trace Tables

A trace table is a systematic method for tracking the values of all variables at each step of an algorithm. It is the mathematical debugger's equivalent of a ledger — every change is recorded, every intermediate value is visible.

**How to construct a trace table:**

1. List all variables as column headers.
2. Record the initial values.
3. For each line of code that modifies a variable, add a new row.
4. At each step, record the value of every variable — even those that did not change.

The discipline of trace table construction forces the programmer to reason about each step explicitly, making implicit assumptions visible and revealing exactly where the observed behavior diverges from the expected behavior.

**Trace Table Example: A Simple Loop**

```python
def sum_even(numbers):
    total = 0
    for n in numbers:
        if n % 2 == 0:
            total += n
    return total

result = sum_even([3, 4, 7, 8, 11])
```

| Step | `n` | `n % 2 == 0` | `total` |
|------|-----|---------------|---------|
| Initial | — | — | 0 |
| Iteration 1 | 3 | False | 0 |
| Iteration 2 | 4 | True | 4 |
| Iteration 3 | 7 | False | 4 |
| Iteration 4 | 8 | True | 12 |
| Iteration 5 | 11 | False | 12 |
| Return | — | — | 12 |

**Debugging Scenario 7: Recursive Fibonacci**

A recursive Fibonacci function omits a base case, causing infinite recursion:

```python
# BUGGY VERSION
def fibonacci(n):
    if n == 1:  # BUG: does not handle n == 0 or n < 1
        return 1
    return fibonacci(n - 1) + fibonacci(n - 2)

# Trace for fibonacci(3):
# fibonacci(3) = fibonacci(2) + fibonacci(1)
# fibonacci(2) = fibonacci(1) + fibonacci(0)  ← fibonacci(0) enters infinite recursion
# fibonacci(1) = 1
# fibonacci(0) = fibonacci(-1) + fibonacci(-2)  ← stack overflow
```

| Step | Call | `n` | `n == 1` | Result |
|------|------|-----|----------|--------|
| 1 | `fibonacci(3)` | 3 | False | recurse |
| 2 | `fibonacci(2)` | 2 | False | recurse |
| 3 | `fibonacci(1)` | 1 | True | 1 |
| 4 | `fibonacci(0)` | 0 | False | recurse — BUG |
| 5 | `fibonacci(-1)` | -1 | False | recurse |
| ... | ... | ... | ... | Stack overflow |

The trace table reveals that `fibonacci(0)` violates the precondition $n \geq 1$ without raising an error.

**The fix** handles all base cases:

```python
# FIXED VERSION
def fibonacci(n):
    if n <= 0:
        raise ValueError("Fibonacci is defined for positive integers")
    if n == 1 or n == 2:
        return 1
    return fibonacci(n - 1) + fibonacci(n - 2)
```

### 🔍 Using Print Debugging to Verify Mathematical Assumptions

Print debugging — inserting temporary output statements to inspect variable values — is the simplest and often most effective debugging technique. When combined with mathematical reasoning, it becomes a precise tool for testing hypotheses about program behavior.

**The print debugging workflow:**

1. State a mathematical hypothesis about what a variable should be at a given point.
2. Insert a print statement at that point.
3. Run the program.
4. Compare the actual value against the expected value.
5. If they differ, the bug is upstream of the print statement.

**Debugging Scenario 8: Floating-Point Accumulation**

A program sums a large number of small values and produces an inaccurate result:

```python
# BUGGY VERSION
def sum_large_list(n):
    total = 0.0
    for i in range(n):
        total += 0.1
    return total

# Hypothesis: sum of 1000 values of 0.1 should equal 100.0
result = sum_large_list(1000)
print(f"Expected: 100.0, Got: {result}")
# Expected: 100.0, Got: 99.99999999999062
```

The discrepancy is small ($\approx 9.4 \times 10^{-12}$), but it demonstrates how rounding errors accumulate over many operations.

**The fix** uses `math.fsum` for accurate floating-point summation:

```python
# FIXED VERSION
import math

def sum_large_list(n):
    values = [0.1] * n
    return math.fsum(values)

result = sum_large_list(1000)
print(f"Expected: 100.0, Got: {result}")
# Expected: 100.0, Got: 100.0
```

`math.fsum` tracks a running exact sum using the Shewchuk algorithm, which maintains an accurate partial sum without the rounding errors that accumulate in naive sequential addition.

**Effective print debugging patterns:**

```python
# Pattern 1: Contextual inspection
print(f"[DEBUG] line 42: total={total}, count={count}, avg={total/count}")

# Pattern 2: Before and after a critical operation
print(f"[DEBUG] before sort: {list}")
data.sort()
print(f"[DEBUG] after sort:  {list}")

# Pattern 3: Branch logging
if value > threshold:
    print(f"[DEBUG] branch: value={value} > threshold={threshold}")
else:
    print(f"[DEBUG] branch: value={value} <= threshold={threshold}")

# Pattern 4: Mathematical verification
expected = (n * (n + 1)) // 2
actual = sum(range(n + 1))
print(f"[DEBUG] sum(1..{n}): expected={expected}, actual={actual}, match={expected == actual}")
```

**When to use print debugging versus a formal debugger:**

| Situation | Recommended approach |
|-----------|---------------------|
| Quick hypothesis check | Print debugging |
| Inspecting many variables simultaneously | Debugger with watch expressions |
| Bug inside a high-iteration loop | Conditional breakpoint or logged counter |
| Bug depends on specific input values | Print debugging with input logging |
| Intermittent or timing-dependent bug | Structured logging (permanent) |
| Need to step backward through execution | Reverse debugger |

## Learning Tips

- 🧠 **Think in ranges, not values.** When analyzing a loop, do not trace only a single iteration — verify the first iteration, the last iteration, and one in the middle. If all three are correct, the pattern is likely correct across the full range.
- 🔍 **Test boundary inputs first.** Before debugging complex logic, test with empty input, single-element input, and the largest expected input. Many bugs hide at these extremes where behavior changes character.
- ✏️ **Draw trace tables on paper.** For complex algorithms, a handwritten trace table forces deliberate, step-by-step reasoning. The physical act of writing engages different cognitive processes than typing, often revealing assumptions that remain invisible on screen.
- 🧮 **Verify with a second method.** When a function produces an unexpected result, compute the expected answer by hand or with a different algorithm. If the two answers disagree, the code contains a defect; if they agree, the bug may lie elsewhere.
- 📐 **Know your data types.** Memorize the limits of your language's numeric types: `INT_MAX`, `INT_MIN`, `float` precision (approximately 15–17 significant decimal digits), and the `float` range ($\pm 10^{308}$).
- ⚖️ **Never trust `==` with floats.** Use `math.isclose()` or tolerance-based comparison for every floating-point equality check. Make this a reflexive habit, not an afterthought.

## Glossary

| Term | Definition |
|------|------------|
| Boundary condition | An input at the extreme edge of the valid range where system behavior changes character |
| Catastrophic cancellation | Severe loss of precision when subtracting two nearly equal floating-point numbers |
| Edge case | An input scenario at the boundary of expected operation that exercises unusual code paths |
| Epsilon | A small positive value used as a tolerance threshold in floating-point comparison |
| Floor division | Division that rounds the result toward negative infinity (`//` in Python) |
| IEEE 754 | The international standard for floating-point arithmetic representation and computation |
| Integer overflow | The result of a computation exceeding the maximum representable integer value for a given type |
| Mantissa | The significant digits of a floating-point number, also called the significand |
| Modulo operator | An operation returning the remainder of integer division (`%` in Python) |
| Off-by-one error | A bug caused by incorrectly counting the number of iterations, elements, or boundaries |
| Precision | The number of significant digits a numeric type can represent accurately |
| Shewchuk algorithm | An algorithm for computing floating-point sums with minimal rounding error |
| Trace table | A systematic record of all variable values at each step of an algorithm's execution |
| True division | Division that always produces a float result (`/` in Python) |
| Underflow | The result of a computation producing a value smaller than the smallest representable positive normal number |
| Wrap-around | The behavior when a value exceeds the maximum representable value and returns to the minimum (or vice versa) |

## Quick References

- [IEEE 754 Standard](https://ieeexplore.ieee.org/document/8766229) — the definitive specification for floating-point arithmetic representation
- [What Every Computer Scientist Should Know About Floating-Point Arithmetic — Goldberg](https://docs.oracle.com/cd/E19957-01/806-3568/ncg_goldberg.html) — classic paper on floating-point representation and pitfalls
- [Python `math` Module Documentation](https://docs.python.org/3/library/math.html) — `isclose`, `fsum`, and other mathematical utilities
- [Python `sys.float_info`](https://docs.python.org/3/library/sys.html#sys.float_info) — the exact bounds and precision of float representation
- [Python `decimal` Module](https://docs.python.org/3/library/decimal.html) — arbitrary-precision decimal arithmetic for financial calculations

## Next Steps

- [Logic](../logic/index.md) — propositional logic, predicate logic, and formal reasoning for program verification and proof-based debugging
- [Debugging](../../programming/foundations/debugging.md) — systematic debugging techniques, tooling, and the scientific method applied to defect resolution
