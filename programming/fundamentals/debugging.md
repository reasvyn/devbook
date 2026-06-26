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

## Study Cases

### Case 1: Debugging a Race Condition in Async Code

Two coroutines update the same shared counter.

```python
import asyncio

counter = 0

async def increment():
    global counter
    for _ in range(1000):
        temp = counter
        await asyncio.sleep(0)  # yield control
        counter = temp + 1

async def main():
    await asyncio.gather(increment(), increment())
    print(counter)  # expected 2000, but often smaller
```

The bug is that `counter` is read, control is yielded, and another coroutine also reads the stale value before the write completes. The fix is a lock or an atomic operation.

**Debugging approach:**

1. Add logging around reads and writes with a unique coroutine id.
2. Run with `PYTHONASYNCIODEBUG=1` to detect non-thread-safe operations.
3. Reproduce with fewer iterations and more `await asyncio.sleep(0)` points.
4. Once identified, replace with `asyncio.Lock` or use `counter = 0` with thread-safe structures.

```python
lock = asyncio.Lock()

async def increment():
    global counter
    async with lock:
        for _ in range(1000):
            counter += 1
            await asyncio.sleep(0)
```

### Case 2: Debugging a Segmentation Fault in C

A C program crashes randomly with a segmentation fault. The code manages a linked list.

```c
#include <stdio.h>
#include <stdlib.h>

typedef struct Node {
    int value;
    struct Node *next;
} Node;

void insert(Node **head, int value) {
    Node *new_node = malloc(sizeof(Node));
    new_node->value = value;
    new_node->next = *head;
    *head = new_node;
}

void free_list(Node *head) {
    Node *current = head;
    while (current != NULL) {
        free(current);
        current = current->next;  // use-after-free: current was freed
    }
}

int main() {
    Node *list = NULL;
    insert(&list, 1);
    insert(&list, 2);
    free_list(list);
    return 0;
}
```

The bug is a use-after-free. `current` is freed, then `current->next` is accessed on the freed pointer. The fix is to save the next pointer before freeing.

**Debugging approach:**

1. Compile with address sanitizer: `gcc -fsanitize=address -g program.c`.
2. Run the program — ASAN reports the exact line of the use-after-free.
3. Alternatively, run under Valgrind: `valgrind ./a.out`.
4. Fix by saving `next` before freeing.

```c
void free_list(Node *head) {
    Node *current = head;
    while (current != NULL) {
        Node *next = current->next;
        free(current);
        current = next;
    }
}
```

### Case 3: Performance Regression with git bisect

A web application became 10x slower after a week of changes. The team needs to find which commit caused it.

**Setup:**

```bash
git bisect start
git bisect bad HEAD           # current version is slow
git bisect good v2.0          # last known fast version
```

Git checks out the midpoint commit. Run the benchmark:

```bash
time curl -o /dev/null -s http://localhost:8080/api/search?q=test
```

If the response time is high, mark the commit as bad; otherwise, mark it as good.

```
git bisect bad   # this commit is slow
git bisect good  # this commit is fast
# repeat until one commit remains
```

Git identifies the first bad commit. In this case, it was a change that added an N+1 query loop.

```python
# Bad commit introduced this pattern:
def get_orders():
    orders = Order.query.all()
    for order in orders:
        order.user = User.query.get(order.user_id)  # one query per order
    return orders
```

**Fix:** Use eager loading instead.

```python
def get_orders():
    return Order.query.join(User).all()
```

Bisect turns a week of hunting into a 5-minute procedure. Always commit frequently and atomically so bisect gives precise results.

## Examples

### Stack Trace Walkthrough

Consider this program:

```python
# example.py
def parse_date(text):
    parts = text.split("-")
    return {"year": int(parts[0]), "month": int(parts[1]), "day": int(parts[2])}

def load_dates(path):
    with open(path) as f:
        return [parse_date(line.strip()) for line in f]

def main():
    dates = load_dates("dates.txt")
    print(dates)

if __name__ == "__main__":
    main()
```

If `dates.txt` contains `2024--01` (double dash) the traceback is:

```
Traceback (most recent call last):
  File "example.py", line 14, in <module>
    main()
  File "example.py", line 10, in main
    dates = load_dates("dates.txt")
  File "example.py", line 7, in load_dates
    return [parse_date(line.strip()) for line in f]
  File "example.py", line 7, in <listcomp>
    return [parse_date(line.strip()) for line in f]
  File "example.py", line 4, in parse_date
    return {"year": int(parts[0]), "month": int(parts[1]), "day": int(parts[2])}
ValueError: invalid literal for int() with base 10: ''
```

Reading bottom to top:

1. `ValueError` in `parse_date` at line 4 — `int(parts[1])` received `""` because splitting `"2024--01"` by `"-"` produces `["2024", "", "01"]`.
2. The error occurred in `parse_date`, called from the list comprehension in `load_dates` (line 7).
3. `load_dates` was called from `main` at line 10.
4. `main` was called from the module top level at line 14.

The stack trace tells you exactly which line raised the exception, in which function, and how the call chain reached it. Use this to navigate to the source of the bug rather than guessing.

### Using a Debugger in VS Code

VS Code provides a graphical debugger for most languages. Here is a typical workflow.

**Setup:**

Create a `.vscode/launch.json` configuration:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "name": "Debug Example",
            "type": "python",
            "request": "launch",
            "program": "${workspaceFolder}/example.py",
            "console": "integratedTerminal"
        }
    ]
}
```

**Workflow:**

1. Set a breakpoint by clicking the gutter next to a line number.
2. Press F5 to start debugging. Execution pauses at the breakpoint.
3. Hover over variables to see their values.
4. Use F10 (Step Over) to advance one line without entering functions.
5. Use F11 (Step Into) to enter a function call.
6. Use Shift+F11 (Step Out) to finish the current function.
7. Inspect the **Watch** pane to evaluate arbitrary expressions at each pause.

```python
def buggy_sort(items):
    for i in range(len(items)):
        for j in range(len(items) - 1):
            if items[j] > items[j + 1]:
                items[j], items[j + 1] = items[j], items[j + 1]  # no swap!
    return items

result = buggy_sort([3, 1, 4, 1, 5])
print(result)  # [3, 1, 4, 1, 5] — unchanged
```

A watch expression like `items[j]` vs `items[j+1]` would immediately reveal the typo: they are assigned to themselves instead of swapped.

### Comparing Print Debugging vs Logging

Print debugging works for small scripts but fails in production. Structured logging scales.

**Print debugging (ad hoc):**

```python
def process_file(path):
    print(f"DEBUG: opening {path}")
    f = open(path)
    print(f"DEBUG: file opened, reading...")
    data = f.read()
    print(f"DEBUG: read {len(data)} bytes")
    result = transform(data)
    print(f"DEBUG: transform done, result={result}")
    return result
```

Problems: output is mixed with normal stdout; no timestamps; cannot filter by severity; easily left in committed code.

**Structured logging (production):**

```python
import logging

logger = logging.getLogger(__name__)

def process_file(path):
    logger.info("Processing file", extra={"path": path})
    try:
        f = open(path)
        data = f.read()
        logger.debug("File read complete", extra={"size": len(data)})
        result = transform(data)
        logger.info("Transform complete")
        return result
    except Exception as e:
        logger.exception("Failed to process file", extra={"path": path})
        raise
```

Benefits: severity levels allow filtering; log aggregators parse JSON structure; timestamps and metadata are automatic; can be toggled at runtime.

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
