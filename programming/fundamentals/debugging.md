# Debugging

## Description

Systematic techniques for finding, understanding, and fixing bugs — from print statements to debuggers to the scientific method. Debugging is a skill that improves with structured practice and knowledge of available tooling.

## Prerequisites

- [Error Handling](../error-handling.md) — understanding how errors manifest and propagate is essential before you can debug them

## Table of Contents

- [Logging Basics](#logging-basics)
- [Debugging Techniques](#debugging-techniques)
- [Advanced Breakpoints](#advanced-breakpoints)
- [Tool-Specific Debugging](#tool-specific-debugging)
- [Tracing and Profiling](#tracing-and-profiling)
- [Common Errors Beginners Make](#common-errors-beginners-make)
- [Learning Tips](#learning-tips)
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

#### The Scientific Method

Treat debugging as hypothesis testing:

1. Observe the failure and gather data.
2. Formulate a hypothesis about the root cause.
3. Design an experiment to test the hypothesis.
4. Run the experiment and analyze results.
5. If the hypothesis is confirmed, fix the bug. Otherwise, refine the hypothesis.

This prevents random trial-and-error and keeps debugging structured. Document each hypothesis and experiment result to avoid retesting the same ideas.

```python
# Hypothesis: The cache key does not include the user ID.
# Experiment: Log the cache key and user ID together.
cached = cache.get(key)
logger.debug(f"Cache lookup: key={key}, user={user_id}, found={cached is not None}")
```

#### Logging as Instrumentation

Add permanent logging at module boundaries — function entry/exit, API calls, and database queries. This creates an audit trail for future debugging without needing to reproduce the bug.

```python
import functools
import logging

logger = logging.getLogger(__name__)

def trace(func):
    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        logger.debug(f"Entering {func.__name__}(args={args}, kwargs={kwargs})")
        try:
            result = func(*args, **kwargs)
            logger.debug(f"Exiting {func.__name__} -> {result}")
            return result
        except Exception as e:
            logger.exception(f"Exception in {func.__name__}: {e}")
            raise
    return wrapper
```

#### Reverse Debugging (Time Travel Debugging)

Modern debuggers (rr, UndoDB, GDB reverse) record program execution and allow stepping backward. When a bug manifests, you can run backward from the crash to find exactly where state became invalid.

```bash
# Record execution with rr
rr record ./my_program

# Replay and debug
rr replay
(gdb) continue    # run to crash
(gdb) reverse-step # step backward from crash
```

### Advanced Breakpoints

Beyond simple line breakpoints, modern debuggers support sophisticated conditional and data-dependent breakpoints.

**Conditional breakpoints** pause only when an expression evaluates to true. They are essential for bugs that only manifest under specific conditions.

```
// VS Code: right-click breakpoint → "Edit Breakpoint" → "Expression"
// Condition: i > 1000 && items[i] == null
```

```gdb
# GDB: condition breakpoint
break process_item if error_count > 5 && item_id == 42
```

**Data breakpoints (watchpoints)** pause when a memory location changes. These are invaluable for tracking down unexpected mutations.

```gdb
# GDB: watch a variable's address
watch my_var
# Hardware watchpoint: pause when my_var changes
```

**Tracepoints** (GDB) log data without pausing execution, useful in production-like environments where stopping is unacceptable.

```gdb
# GDB: tracepoint that logs and continues
trace calculate_total
actions
    collect total
    collect $pc
    printf "total = %d\n", total
end
```

**Function breakpoints** pause whenever a specific function is entered, even without knowing where it is called from. Useful for third-party library debugging.

```gdb
break malloc
# pause every time malloc is called
```

**Exception breakpoints** pause when an exception is thrown or caught, before any crash. In Chrome DevTools and VS Code, enable "Pause on caught exceptions" in the debugger panel.

### Tool-Specific Debugging

#### GDB (GNU Debugger)

GDB is the standard debugger for C, C++, Rust, and Go programs on Unix systems.

**Basic GDB workflow:**

```bash
# Compile with debug symbols
gcc -g -o my_program my_program.c
# Start debugging
gdb ./my_program
```

**Essential GDB commands:**

```gdb
(gdb) break main         # break at function entry
(gdb) run arg1 arg2      # start program with arguments
(gdb) next               # step over (like "step over")
(gdb) step               # step into function
(gdb) continue           # resume until next breakpoint
(gdb) print var          # inspect variable
(gdb) display var        # auto-print variable every pause
(gdb) backtrace          # show stack trace
(gdb) frame 2            # switch to frame 2
(gdb) info locals        # show all local variables
(gdb) list               # show source code around current line
(gdb) quit               # exit
```

**Debugging core dumps:**

```bash
# Enable core dumps
ulimit -c unlimited
# After crash, analyze the core dump
gdb ./my_program core
(gdb) backtrace full     # show stack with local variables at crash
```

#### Chrome DevTools

Chrome DevTools provides a full-featured debugger for JavaScript running in the browser.

**Debugging workflow:**

1. Open DevTools (F12 or Ctrl+Shift+I), go to Sources panel.
2. Set breakpoints by clicking line numbers in the source viewer.
3. Reload or trigger the bug. Execution pauses at breakpoints.
4. Use the Scope panel to inspect variables.
5. Use the Call Stack panel to trace the call path.
6. Use Watch to evaluate expressions live.

**Event listener breakpoints:** Right-click in the Sources panel and select "Event Listener Breakpoints". Pause on click, keydown, fetch, timer, or any DOM event without adding breakpoint lines.

**XHR/Fetch breakpoints:** In the Sources panel, open "XHR/Fetch Breakpoints". Add a URL pattern. DevTools pauses before any matching network request, showing the call stack that initiated it.

**Performance analysis:**

```javascript
// Use console.assert for invariant checking
function processOrder(order) {
  console.assert(order.items.length > 0, "Order must have items");
  console.assert(order.total > 0, "Order total must be positive");
  // ...
}
```

**Live expressions:** Add expressions (like `document.querySelectorAll('.error')`) to the console that update in real-time as you step through code.

#### Python Debugger (pdb)

```python
import pdb

def buggy_function(x, y):
    result = x / y
    pdb.set_trace()  # execution pauses here
    return result * 2
```

**pdb commands:** `n` (next), `s` (step into), `c` (continue), `p` (print), `l` (list), `q` (quit).

#### Rust/Cargo with LLDB

Rust programs can be debugged with lldb (the LLVM debugger) or by enabling debugger integration in VS Code with the rust-analyzer extension.

```bash
rustup add component rust-src
cargo build
lldb target/debug/my_program
```

### Tracing and Profiling

Tracing records the execution path of a program at function or instruction granularity, while profiling measures performance.

**System call tracing with strace:**

```bash
# Trace all system calls made by a program
strace -o trace.log ./my_program
# Filter specific syscalls
strace -e openat,read,write ./my_program
```

Use strace to debug permission issues, missing files, and unexpected network connections. The trace shows every file opened, every network socket created, and every error code returned.

**Function call tracing:**

```bash
# C/C++ with gcc -finstrument-functions
# Every function entry/exit triggers instrument functions
# Or use ltrace for library calls
ltrace ./my_program
```

**Python tracing:**

```python
import sys

def trace_calls(frame, event, arg):
    if event == 'call':
        print(f"→ {frame.f_code.co_name} at {frame.f_lineno}")
    elif event == 'return':
        print(f"← {frame.f_code.co_name} → {arg}")
    return trace_calls

sys.settrace(trace_calls)
```

**Performance profiling:**

```python
import cProfile
import pstats

def run():
    # your code here
    pass

cProfile.run('run()', 'profile_stats')
p = pstats.Stats('profile_stats')
p.sort_stats('cumtime').print_stats(20)  # top 20 by cumulative time
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

#### Mutation Aliasing

Two references pointing to the same mutable object cause unexpected side effects.

```python
def add_item(item, items=[]):  # Bug: mutable default argument
    items.append(item)
    return items

print(add_item(1))  # [1]
print(add_item(2))  # [1, 2] — bug!
```

```javascript
const config = { timeout: 1000 };
function setConfig(overrides) {
  Object.assign(config, overrides);  // mutates the original
}
```

#### Off-By-N Errors in Recursion

Recursive base cases that never execute.

```python
def factorial(n):
    # Bug: should be n <= 1, not n == 1
    if n == 1:
        return 1
    return n * factorial(n - 1)  # StackOverflow for n <= 0
```

Always check edge cases: empty input, single element, negative values, and zero.

### Learning Tips

**Reproduce before diagnosing:** Never start debugging without a reliable reproduction. A bug you can reproduce is a bug you can fix. Write a minimal test case before touching production code.

**One change at a time:** When testing hypotheses, change exactly one thing between runs. Changing multiple things simultaneously makes it impossible to know which change affected the result.

**Check the obvious first:** Wrong data type, wrong environment variable, wrong branch, stale build. These account for a surprising number of "mysterious" bugs. Always confirm assumptions.

**Write tests for bugs:** Before fixing a bug, write a test that reproduces it. This ensures you understand the bug, your fix actually resolves it, and the bug never regresses.

**Use `git stash` and `git blame`:** Stash your current changes to verify the bug exists in the clean codebase. Use `git blame` to see when and why a line was last changed — the commit message may explain the intent.

**Practice with deliberately broken code:** Create small programs with hidden bugs and practice finding them with different techniques (print, debugger, binary search). Speed improves with deliberate practice.

**Learn your debugger deeply:** Most developers use 10% of their debugger's features. Spend an hour learning conditional breakpoints, watchpoints, and call stack navigation.

**Take breaks:** Debugging fatigue leads to sloppy thinking. If you have been stuck for 30 minutes, step away. A fresh perspective often finds the bug in minutes.

## Glossary

| Term | Definition |
|------|------------|
| Bug | A defect in code that causes incorrect behaviour |
| Debugging | The process of identifying, isolating, and fixing bugs |
| Logging | Recording events and state during program execution for analysis |
| Breakpoint | A deliberate pause point set in a debugger to inspect program state |
| Conditional breakpoint | A breakpoint that pauses only when a specified expression is true |
| Data breakpoint (watchpoint) | A breakpoint that pauses when a memory location is written to |
| Tracepoint | A debugger feature that logs data without pausing execution |
| Step over | Debugger action: execute the current line and pause at the next line in the same function |
| Step into | Debugger action: enter a function call and pause at its first line |
| Step out | Debugger action: run until the current function returns and pause at the caller |
| Watch | A debugger feature that continuously evaluates and displays an expression |
| Stack trace | A report of the active stack frames at a point of execution, typically when an exception occurs |
| Call stack | The ordered list of function calls leading to the current execution point |
| Frame | A section of the call stack representing a single function invocation with its local variables |
| Core dump | A file containing the memory state of a crashed process for post-mortem analysis |
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
| strace | Linux tool for tracing system calls made by a process |
| Reverse debugging | Debugging technique that allows stepping backward through execution (time travel) |
| Profiler | A tool that measures where a program spends its time or memory |
| Crash reproduction | The ability to reliably trigger a bug on demand |
| Hypothesis-driven debugging | Treating each bug as a scientific inquiry with testable hypotheses |
| Tracing | Recording the sequence of function calls or events during execution |
| Mutation aliasing | A bug caused by unintentionally sharing mutable references |

## Quick References

- [Python Logging Cookbook](https://docs.python.org/3/howto/logging-cookbook.html) — practical logging patterns for production applications
- [VS Code Debugging Guide](https://code.visualstudio.com/docs/editor/debugging) — how to set up and use the VS Code debugger
- [Chrome DevTools Debugger](https://developer.chrome.com/docs/devtools/javascript/) — debugging JavaScript in the browser
- [GDB Documentation](https://sourceware.org/gdb/current/onlinedocs/gdb/) — official GDB manual
- [git bisect Documentation](https://git-scm.com/docs/git-bisect) — official reference for finding regressions with binary search
- [Valgrind Homepage](https://valgrind.org/) — memory debugging, leak detection, and profiling for C/C++
- [Address Sanitizer](https://github.com/google/sanitizers/wiki/AddressSanitizer) — fast memory error detector for C/C++
- [rr — Record and Replay Debugging](https://rr-project.org/) — reversible debugging for Linux
- [strace Tutorial](https://blogs.oracle.com/linux/post/strace-the-syscall-tracer) — using strace for system call tracing
- [cProfile Documentation](https://docs.python.org/3/library/profile.html) — Python performance profiler
- [The Debugging Mindset — Julia Evans](https://jvns.ca/debugging/) — practical debugging techniques and mental models
- [Why Programs Fail — Zeller](https://www.whyprogramsfail.com/) — systematic approach to debugging

## Next Steps

- [Error Handling](../error-handling.md) — revisit with deeper insight into how errors should be caught and managed
- [Functions](../functions.md) — understanding function design helps you write debuggable code
