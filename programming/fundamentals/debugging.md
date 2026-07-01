# Debugging

## Description

Systematic techniques for finding, understanding, and fixing bugs — from print statements to debuggers to the scientific method.

## Prerequisites

- [Error Handling](../error-handling.md) — understanding how errors manifest and propagate is essential before you can debug them

## Table of Contents

- [Logging Basics](#logging-basics)
- [Debugging Techniques](#debugging-techniques)
- [Common Errors Beginners Make](#common-errors-beginners-make)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Logging Basics

Logging is structured output that helps you understand what the program did.

**Print debugging** is the simplest form — inserting print calls temporarily.

```python
def process(items):
    print(f"DEBUG: got {len(items)} items")  # temporary
    for i, item in enumerate(items):
        print(f"DEBUG: item {i} = {item}")   # temporary
        # ...
```

**Structured logging** records events with metadata, making them searchable and filterable.

```python
import logging

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

logger.debug("Connecting to database")   # verbose diagnostics
logger.info("Server started on port 8080")  # normal operation
logger.warning("Disk space below 10%")     # concerning but not failing
logger.error("Failed to connect to upstream")  # failure
logger.critical("Database unreachable, shutting down")  # fatal
```

```javascript
const levels = { DEBUG: 0, INFO: 1, WARN: 2, ERROR: 3 };

function log(level, message, meta = {}) {
  if (levels[level] >= levels[LOG_LEVEL]) {
    console.log(JSON.stringify({ level, message, ...meta, timestamp: new Date() }));
  }
}

log("INFO", "Server started", { port: 8080 });
```

### Debugging Techniques

#### Print Debugging

Insert `print()` or `console.log()` calls at strategic points to inspect values. Remove them when done. Crude but effective for simple issues.

#### Using a Debugger

A debugger gives you fine-grained control over execution.

**Breakpoints** pause execution at a specific line. When paused you can inspect variables, evaluate expressions, and control execution step by step.

**Step over** executes the current line and moves to the next line in the same function. If the line calls another function, the debugger does not enter it.

**Step into** enters any function call on the current line so you can trace execution inside.

**Step out** runs until the current function returns and pauses at the caller.

**Watch variables** are expressions that the debugger evaluates and displays at every pause.

#### Reading Stack Traces

A stack trace shows the call path that led to an exception. Every line shows a frame: the file, line number, and function name.

```python
Traceback (most recent call last):
  File "app.py", line 15, in <module>
    main()
  File "app.py", line 10, in main
    process(data)
  File "app.py", line 5, in process
    return 1 / x
ZeroDivisionError: division by zero
```

Read from bottom to top. The bottom line is the exception type and message. Above it is the innermost frame where it was raised. Above that is the caller, and so on.

#### Rubber Duck Debugging

Explain your code line by line to an inanimate object (or a colleague). The act of verbalising often reveals the flaw.

#### Binary Search Through Code

If a bug manifests after many steps, comment out half the code and see if the bug disappears. Narrow down by repeatedly halving the search space.

```python
# Step 1: comment out lines 10-50 → bug still there
# Step 2: comment out lines 10-30 → bug still there
# Step 3: comment out lines 10-20 → bug is gone
# The defect is in lines 10-20
```

#### Delta Debugging

When a bug appeared after a change, systematically isolate which change caused it. Version control tools make this straightforward with `git bisect`.

```bash
git bisect start
git bisect bad           # current commit is bad
git bisect good v1.0     # known good commit
# git checks out a midpoint; test and mark good/bad
# repeat until the introducing commit is identified
```

### Common Errors Beginners Make

#### Off-by-One Errors

Loop boundaries are a frequent source of bugs.

```python
items = [1, 2, 3]
# Bug: should be len(items), not len(items) - 1
for i in range(len(items) - 1):
    print(items[i])  # prints 1, 2 — missing 3
```

```javascript
const items = [1, 2, 3];
// Bug: using <= instead of <
for (let i = 0; i <= items.length; i++) {
  console.log(items[i]);  // logs 1, 2, 3, undefined
}
```

#### Null / Undefined Access

Accessing a property on `null` or `undefined` throws immediately.

```python
user = get_user()  # returns None
print(user["name"])  # TypeError: 'NoneType' object is not subscriptable
```

```javascript
const user = getUser();  // returns null
console.log(user.name);  // TypeError: Cannot read properties of null
```

Check for null before access, or use optional chaining.

```javascript
console.log(user?.name ?? "Guest");  // JavaScript optional chaining
```

```python
if user is not None:
    print(user["name"])
```

#### Type Errors

Mixing types produces confusing errors.

```python
count = "5"
total = count + 10   # TypeError: can only concatenate str (not "int") to str
```

```javascript
let count = "5";
let total = count + 10;  // "510" — no error, but wrong! Type coercion hides the bug
```

#### Reference Before Assignment

Using a variable before it has a value.

```python
def example(flag):
    if flag:
        result = 42
    print(result)  # UnboundLocalError if flag is False
```

```javascript
function example(flag) {
  if (flag) {
    var result = 42;
  }
  console.log(result);  // undefined — no error, but likely wrong
}
```

#### Infinite Loops

A loop whose termination condition is never met.

```python
i = 0
while i < 10:
    print(i)
    # forgot to increment i
```

```javascript
let i = 0;
while (i < 10) {
  console.log(i);
  // forgot to increment i
}
```

Always verify that loop variables converge toward the exit condition.

#### Asynchronous Error Handling

Exceptions thrown in async code are easy to miss.

```python
import asyncio

async def failing():
    raise ValueError("oops")

async def main():
    task = asyncio.create_task(failing())
    await asyncio.sleep(1)
    # What happened to the error? It is silently lost unless awaited.

asyncio.run(main())
```

```javascript
async function failing() {
  throw new Error("oops");
}

const promise = failing();
// No .catch() — unhandled promise rejection
```

In Node.js, unhandled promise rejections crash the process. In the browser, they log warnings.

```javascript
async function main() {
  try {
    await failing();
  } catch (error) {
    console.error("Caught:", error);
  }
}
```

```python
async def main():
    try:
        await failing()
    except ValueError as e:
        print("Caught:", e)
```

## Glossary

| Term | Definition |
|------|------------|
| Bug | A defect in code that causes incorrect behaviour |
| Debugging | The process of identifying, isolating, and fixing bugs |
| Logging | Recording events and state during program execution for analysis |
| Breakpoint | A deliberate pause point set in a debugger to inspect program state |
| Step over | Debugger action: execute the current line and pause at the next line in the same function |
| Step into | Debugger action: enter a function call and pause at its first line |
| Step out | Debugger action: run until the current function returns and pause at the caller |
| Watch | A debugger feature that continuously evaluates and displays an expression |
| Stack trace | A report of the active stack frames at a point of execution, typically when an exception occurs |
| Call stack | The ordered list of function calls leading to the current execution point |
| Frame | A section of the call stack representing a single function invocation with its local variables |
| Rubber duck debugging | Explaining code line by line to an inanimate object to discover flaws |
| Binary search (debugging) | Systematically halving the code to narrow down the location of a bug |
| Delta debugging | Isolating which change introduced a bug by testing subsets of changes |
| git bisect | A Git command that performs binary search through commit history to find the first bad commit |
| Regression | A bug introduced by a change that previously working code |
| Race condition | A bug caused by timing-dependent interleaving of concurrent operations |
| Use-after-free | Accessing memory after it has been freed, common in C/C++ |
| Sanitizer | A compile-time instrumentation tool that detects memory errors (ASAN, UBSAN, TSAN) |
| Valgrind | A runtime analysis tool for detecting memory leaks and errors |
| N+1 query | A performance anti-pattern where one query is followed by N additional queries in a loop |
| Print debugging | Inserting temporary print statements to inspect program state |
| Structured logging | Logging with machine-parseable metadata and severity levels |

## Quick References

- [Python Logging Cookbook](https://docs.python.org/3/howto/logging-cookbook.html) — practical logging patterns for production applications
- [VS Code Debugging Guide](https://code.visualstudio.com/docs/editor/debugging) — how to set up and use the VS Code debugger
- [Chrome DevTools Debugger](https://developer.chrome.com/docs/devtools/javascript/) — debugging JavaScript in the browser
- [git bisect Documentation](https://git-scm.com/docs/git-bisect) — official reference for finding regressions with binary search
- [Valgrind Homepage](https://valgrind.org/) — memory debugging, leak detection, and profiling for C/C++
- [Address Sanitizer](https://github.com/google/sanitizers/wiki/AddressSanitizer) — fast memory error detector for C/C++

## Next Steps

- [Error Handling](../error-handling.md) — revisit with deeper insight into how errors should be caught and managed
- [Functions](../functions.md) — understanding function design helps you write debuggable code
