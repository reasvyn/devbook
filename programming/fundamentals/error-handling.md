# Error Handling

## Description

Errors happen in every program. This document teaches you how to anticipate, catch, and recover from errors gracefully. Mastering error handling separates fragile code from production-ready software.

## Prerequisites

- [Collections](../collections.md) — familiarity with arrays, lists, and dictionaries helps with understanding iteration errors and type safety

## Table of Contents

- [What Are Errors?](#what-are-errors)
- [Error Handling Philosophy](#error-handling-philosophy)
- [Try / Catch / Finally](#try--catch--finally)
- [Throwing Errors](#throwing-errors)
- [Error Types](#error-types)
- [Error Propagation](#error-propagation)
- [Catching Selectively](#catching-selectively)
- [The Finally Block](#the-finally-block)
- [Try / Except / Else (Python)](#try--except--else-python)
- [Error Handling Patterns](#error-handling-patterns)
- [Assertions](#assertions)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Are Errors?

Errors are defects in a program that prevent it from doing what it should. They fall into three categories.

**Syntax errors** happen when the code violates the language grammar. The interpreter or compiler cannot parse the file.

```python
# Python — missing colon
def greet(name)
    print("Hello", name)
```

```javascript
// JavaScript — unmatched brace
function greet(name) {
  console.log("Hello", name);
// missing closing brace
```

Syntax errors are caught before any code runs. Your editor or linter should flag them immediately.

**Runtime errors** (exceptions) occur during execution. The syntax is valid, but something goes wrong while running.

```python
x = 10 / 0  # ZeroDivisionError
```

```javascript
let data = JSON.parse("not-json");  // SyntaxError at runtime
```

**Logic errors** are the trickiest. The program runs without crashing but produces wrong results. No exception is raised.

```python
def average(nums):
    return sum(nums) / len(nums)  # correct
    # but if called with an empty list — ZeroDivisionError is a runtime error

# logic error example:
def discount(price, percent):
    return price + price * percent / 100  # added instead of subtracted
```

### Error Handling Philosophy

Three guiding principles inform how experienced developers approach errors.

**Defensive programming** assumes that inputs, dependencies, and external systems will misbehave. Validate everything at boundaries.

```python
def divide(a, b):
    if b == 0:
        return None  # handle the edge case
    return a / b
```

**Fail fast** means crashing immediately when something unexpected happens, rather than silently continuing with corrupted state. A loud crash is easier to debug than a subtle misbehaviour later.

```python
def connect(timeout):
    if timeout < 0:
        raise ValueError("timeout must be non-negative")
    # proceed with connection
```

**Graceful degradation** means the application continues operating in a reduced capacity when a non-critical component fails. Show the user a friendly message instead of a blank crash page.

```python
try:
    data = fetch_user_preferences()
except ConnectionError:
    data = get_default_preferences()  # fall back to defaults
```

### Try / Catch / Finally

The try/catch/finally construct is the primary tool for handling exceptions at runtime.

**Python syntax:**

```python
try:
    risky_operation()
except SomeError:
    handle_error()
finally:
    cleanup()
```

**JavaScript syntax:**

```javascript
try {
  riskyOperation();
} catch (error) {
  handleError(error);
} finally {
  cleanup();
}
```

**Flow rules:**

1. If the `try` body completes without throwing, `catch` is skipped and `finally` runs afterwards.
2. If an exception is thrown inside `try`, the remaining `try` code is skipped, `catch` runs, then `finally` runs.
3. If `catch` itself throws, `finally` still runs before the new exception propagates.
4. If `finally` contains a `return` or throws, it overrides any exception from `try` or `catch`.

```python
def demo():
    try:
        return "from try"
    finally:
        return "from finally"  # overrides the try return

print(demo())  # "from finally"
```

### Throwing Errors

You raise (Python) or throw (JavaScript) an error yourself when your code detects a condition it cannot handle.

```python
def withdraw(balance, amount):
    if amount < 0:
        raise ValueError("amount must be positive")
    if amount > balance:
        raise InsufficientFundsError(balance, amount)
    return balance - amount
```

```javascript
function withdraw(balance, amount) {
  if (amount < 0) {
    throw new Error("amount must be positive");
  }
  if (amount > balance) {
    throw new InsufficientFundsError(balance, amount);
  }
  return balance - amount;
}
```

**Custom error types** make errors more descriptive and allow selective catching.

```python
class InsufficientFundsError(Exception):
    def __init__(self, balance, amount):
        self.balance = balance
        self.amount = amount
        super().__init__(f"need {amount}, have {balance}")
```

```javascript
class InsufficientFundsError extends Error {
  constructor(balance, amount) {
    super(`need ${amount}, have ${balance}`);
    this.balance = balance;
    this.amount = amount;
    this.name = "InsufficientFundsError";
  }
}
```

### Error Types

Languages ship with built-in error types for common situations.

| Category | Python | JavaScript |
|----------|--------|------------|
| Type mismatch | `TypeError` | `TypeError` |
| Invalid value | `ValueError` | `RangeError` |
| Reference to undefined | `NameError` | `ReferenceError` |
| Index out of bounds | `IndexError` | `RangeError` |
| Key not found | `KeyError` | — |
| Attribute missing | `AttributeError` | `TypeError` (for null/undefined access) |
| I/O | `OSError` / `FileNotFoundError` | `Error` |
| Assertion | `AssertionError` | `AssertionError` (via assert module) |

### Error Propagation

When an exception is not caught where it occurs, it propagates up the call stack until a matching handler is found or the program terminates.

```python
def inner():
    raise ValueError("inner problem")

def middle():
    inner()  # no try here — propagates up

def outer():
    try:
        middle()
    except ValueError as e:
        print("Caught in outer:", e)

outer()
```

The call stack is the ordered list of function calls that led to the current point. When an exception propagates, the runtime unwinds the stack, discarding local frames.

### Catching Selectively

Never catch all exceptions with a bare `except:` unless you have a very specific reason. It hides bugs.

```python
# Bad
try:
    result = do_something()
except:           # catches everything — including KeyboardInterrupt, MemoryError
    pass

# Good
try:
    result = do_something()
except ValueError:
    handle_bad_input()
except ConnectionError:
    retry()
```

**Multiple except blocks** let you handle each error type differently.

```python
try:
    data = fetch_from_network()
except TimeoutError:
    retry()
except ConnectionError:
    use_cache()
except ValueError as e:
    log_and_report(e)
    raise  # re-raise if you cannot handle it
```

```javascript
try {
  await fetchFromNetwork();
} catch (error) {
  if (error instanceof TimeoutError) {
    retry();
  } else if (error instanceof ConnectionError) {
    useCache();
  } else {
    logAndReport(error);
    throw;  // re-throw
  }
}
```

### The Finally Block

`finally` exists for one reason: cleanup. Code that must run regardless of success or failure belongs there — closing files, releasing locks, shutting down connections.

```python
file = None
try:
    file = open("data.txt")
    process(file.read())
except ProcessingError:
    log("processing failed")
finally:
    if file is not None:
        file.close()  # always runs
```

```javascript
let conn = null;
try {
  conn = openConnection();
  sendData(conn);
} catch (error) {
  log("send failed");
} finally {
  if (conn) conn.close();
}
```

In modern Python, resource cleanup is often handled by context managers (`with` statement), which internally use `try/finally`.

### Try / Except / Else (Python)

Python's `try` block accepts an optional `else` clause that runs only if no exception was raised. Use it to separate code that might throw from code that should only execute on success.

```python
try:
    config = load_config()
except FileNotFoundError:
    config = DEFAULT_CONFIG
else:
    print("Config loaded from file")  # only runs if no exception
```

This avoids accidentally catching an error from code that belongs in the success path.

### Error Handling Patterns

#### Look Before You Leap (LBYL)

Check conditions before attempting an operation. Common in languages without exceptions or where performance matters and the common case is failure.

```python
def divide_lbyl(a, b):
    if b == 0:
        return None
    if not isinstance(a, (int, float)) or not isinstance(b, (int, float)):
        return None
    return a / b
```

```javascript
function divideLbyl(a, b) {
  if (b === 0) return null;
  if (typeof a !== "number" || typeof b !== "number") return null;
  return a / b;
}
```

#### Easier to Ask Forgiveness Than Permission (EAFP)

Attempt the operation and handle the failure. Idiomatic in Python and used widely in dynamic languages.

```python
def divide_eafp(a, b):
    try:
        return a / b
    except (ZeroDivisionError, TypeError):
        return None
```

```javascript
function divideEafp(a, b) {
  try {
    return a / b;
  } catch (error) {
    return null;
  }
}
```

EAFP often produces cleaner code when the common case is success. LBYL can lead to duplicate checks or race conditions (the state checked may change before the operation).

#### Result Type Pattern

Instead of throwing, return an object that encodes success or failure. Common in functional-influenced code.

```python
from dataclasses import dataclass
from typing import Generic, TypeVar, Union

T = TypeVar("T")
E = TypeVar("E")

@dataclass
class Ok(Generic[T]):
    value: T

@dataclass
class Err(Generic[E]):
    error: E

Result = Union[Ok[T], Err[E]]

def divide_result(a, b):
    if b == 0:
        return Err("division by zero")
    return Ok(a / b)

result = divide_result(10, 0)
if isinstance(result, Ok):
    print(result.value)
else:
    print("Error:", result.error)
```

```javascript
class Ok {
  constructor(value) { this.value = value; }
}

class Err {
  constructor(error) { this.error = error; }
}

function divideResult(a, b) {
  if (b === 0) return new Err("division by zero");
  return new Ok(a / b);
}

const result = divideResult(10, 0);
if (result instanceof Ok) {
  console.log(result.value);
} else {
  console.log("Error:", result.error);
}
```

This pattern forces callers to handle errors explicitly — no silent exceptions.

#### Error Monad / Either Type

A more formal version of the result type used in functional programming. Libraries like `returns` (Python) or `fp-ts` (TypeScript) provide implementations.

```python
from returns.result import Result, Success, Failure

def divide_result(a, b):
    if b == 0:
        return Failure(ZeroDivisionError("division by zero"))
    return Success(a / b)

result = divide_result(10, 0)
result.map(lambda v: print(v))  # no-op on Failure
```

#### Callback Error Handling (Node.js Style)

Older Node.js APIs follow the convention of passing an error as the first argument to a callback.

```javascript
const fs = require("fs");

fs.readFile("/etc/passwd", (err, data) => {
  if (err) {
    console.error("Failed to read file:", err.message);
    return;
  }
  console.log(data.toString());
});
```

Modern code uses Promises and async/await instead, but you may encounter this pattern in legacy codebases.

### Assertions

Assertions are runtime checks that verify your assumptions. They are a debugging aid, not a replacement for error handling.

```python
def calculate(rate, time):
    assert rate > 0, "rate must be positive"
    assert time >= 0, "time must be non-negative"
    return rate * time
```

```javascript
function calculate(rate, time) {
  console.assert(rate > 0, "rate must be positive");
  console.assert(time >= 0, "time must be non-negative");
  return rate * time;
}
```

Python assertions can be disabled with the `-O` flag. Never use assertions for validation that must always run (input validation, security checks).

## Glossary

| Term | Definition |
|------|------------|
| Error | A deviation from correct behaviour in a program |
| Exception | An event that occurs during program execution that disrupts the normal flow of instructions |
| Syntax error | An error in the grammatical structure of code that prevents parsing |
| Runtime error | An error that occurs during program execution (also called an exception) |
| Logic error | A bug where the program produces wrong results without crashing |
| Try | A block that wraps code which might throw an exception |
| Catch | A block that handles an exception if one occurs (JavaScript) |
| Except | A block that handles an exception if one occurs (Python) |
| Finally | A block that always executes after try/catch, used for cleanup |
| Throw | The action of raising an exception explicitly (JavaScript) |
| Raise | The action of raising an exception explicitly (Python) |
| Error propagation | The process of an exception moving up the call stack until caught |
| Call stack | The ordered list of function calls leading to the current execution point |
| Stack trace | A printed report of the call stack at the point of an exception |
| Assertion | A boolean check that crashes the program if false, used as a debugging aid |
| LBYL | Look Before You Leap — checking conditions before performing an operation |
| EAFP | Easier to Ask Forgiveness Than Permission — attempting an operation and handling failures |
| Fail fast | A philosophy of crashing immediately on unexpected errors rather than continuing with corrupted state |
| Graceful degradation | Continuing operation in a reduced capacity when non-critical components fail |
| Defensive programming | Writing code that anticipates and guards against invalid inputs and unexpected states |
| Error handling | The practice of anticipating, detecting, and recovering from errors |
| Exception safety | A guarantee about how code behaves when exceptions are thrown |
| Resource leak | A failure to release a system resource (file handle, connection, lock) after use |
| Cleanup | Code that releases resources or restores state, typically placed in a finally block |
| Re-raise | Throwing a caught exception again to let it propagate after logging or partial handling |
| Context manager | A Python object that defines `__enter__` and `__exit__` for resource management |
| Exponential backoff | A retry strategy that increases the delay between attempts by a multiplicative factor |
| Result type | A type that encodes success or failure as a value rather than using exceptions |
| Thundering herd | A problem where many clients retry simultaneously and overwhelm a recovering service |

## Quick References

- [Python Errors and Exceptions (Official Docs)](https://docs.python.org/3/tutorial/errors.html) — Python's official tutorial on errors, exceptions, and handling
- [MDN: Control flow and error handling](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Control_flow_and_error_handling) — JavaScript guide to try/catch/throw and error types
- [Python Logging Cookbook](https://docs.python.org/3/howto/logging-cookbook.html) — practical logging patterns for production applications
- [Exception Handling Anti-Patterns (Python docs)](https://docs.python.org/3/howto/doanddont.html) — best practices and what to avoid
- [Rust Error Handling](https://doc.rust-lang.org/book/ch09-00-error-handling.html) — the Result and Option types for comparison with exception-based languages

## Next Steps

- [Debugging](../debugging.md) — systematic techniques for finding and fixing bugs
- [Functions](../functions.md) — revisit how function design affects error propagation
