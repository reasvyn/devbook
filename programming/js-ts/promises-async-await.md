# Promises & Async/Await

## Description

JavaScript is single-threaded but non-blocking. Promises and async/await are the modern way to handle asynchronous operations — from API calls to file reads to timers — without callback nesting.

## Prerequisites

- [Functions in Depth](functions-in-depth.md) — callbacks, closures, arrow functions
- [Events](events.md) — event loop basics

## Table of Contents

- [Promise Basics](#promise-basics)
- [Creating Promises](#creating-promises)
- [Promise Chaining](#promise-chaining)
- [Error Handling](#error-handling)
- [Promise Static Methods](#promise-static-methods)
- [Async/Await](#asyncawait)
- [Concurrency Patterns](#concurrency-patterns)
- [The Event Loop](#the-event-loop)
- [Microtasks vs. Macrotasks](#microtasks-vs-macrotasks)
- [Promise Anti-Patterns](#promise-anti-patterns)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Problem with Callbacks

Before Promises, asynchronous code used callbacks. Nesting callbacks creates "callback hell" — deeply indented, error-prone code:

```javascript
getUser(42, (err, user) => {
    if (err) return handleError(err);
    getPosts(user.id, (err, posts) => {
        if (err) return handleError(err);
        getComments(posts[0].id, (err, comments) => {
            if (err) return handleError(err);
            renderComments(comments);
        });
    });
});
```

**Problems with this pattern:**

- Deep nesting makes code hard to read
- Error handling is repetitive
- Each function must handle both result and error parameters
- Inversion of control — the callback is given to another function

Promises solve these problems with a flat chain and unified error handling:

```javascript
getUser(42)
    .then(user => getPosts(user.id))
    .then(posts => getComments(posts[0].id))
    .then(comments => renderComments(comments))
    .catch(handleError);
```

### Promise Basics

A Promise represents a value that may be available now, later, or never.

**Three states:**

```
┌──────────┐
│ Pending  │──► Resolved (fulfilled)
│          │──► Rejected
└──────────┘
```

- **Pending** — initial state, neither fulfilled nor rejected
- **Fulfilled** — operation completed successfully
- **Rejected** — operation failed

```javascript
const promise = new Promise((resolve, reject) => {
    // async work here
});

console.log(promise);                    // Promise { <pending> }
```

**Consuming a Promise:**

```javascript
fetch("/api/users")
    .then(response => response.json())   // resolved
    .then(data => console.log(data))     // resolved
    .catch(error => console.error(error)); // rejected
```

- `.then(onFulfilled, onRejected)` — handles fulfillment (and optionally rejection)
- `.catch(onRejected)` — handles rejection
- `.finally(onFinally)` — runs regardless of outcome

### Creating Promises

Wrap callback-based APIs:

```javascript
function readFilePromise(path) {
    return new Promise((resolve, reject) => {
        fs.readFile(path, "utf8", (err, data) => {
            if (err) reject(err);
            else resolve(data);
        });
    });
}
```

**Wrap setTimeout as a delay utility:**

```javascript
function delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}

// Usage
await delay(1000);
console.log("One second later");
```

**Important rules:**

- `resolve` and `reject` are one-shot — calling either a second time does nothing
- Once a Promise is settled (fulfilled or rejected), it stays settled
- The executor function runs synchronously

```javascript
const p = new Promise((resolve) => {
    console.log("executor runs immediately");   // logged synchronously
    resolve("done");
    resolve("ignored");                          // silently ignored
});

console.log("after creation");                   // logged second
p.then(console.log);                              // "done" (async)
```

### Promise Chaining

`.then()` always returns a new Promise, making chaining possible:

```javascript
fetch("/api/users/42")
    .then(response => response.json())
    .then(user => fetch(`/api/posts?userId=${user.id}`))
    .then(response => response.json())
    .then(posts => {
        console.log(`User has ${posts.length} posts`);
    })
    .catch(error => {
        console.error("Something failed:", error);
    });
```

**What `.then()` returns:**

| If handler returns | The chain receives |
|---|---|
| A value (non-Promise) | A Promise resolved to that value |
| A Promise | That Promise (chain waits for it) |
| Nothing (`undefined`) | A Promise resolved to `undefined` |
| Throws an error | A rejected Promise |

```javascript
Promise.resolve(1)
    .then(v => v * 2)                          // returns 2
    .then(v => Promise.resolve(v * 3))         // returns 6 (waits for Promise)
    .then(v => { console.log(v); })            // logs 6, returns undefined
    .then(v => { throw new Error("fail"); })   // rejects
    .catch(e => console.log(e.message));       // "fail"
```

**Passing results forward:**

```javascript
getUser(42)
    .then(user => {
        return getPosts(user.id)
            .then(posts => ({ user, posts })); // combine results
    })
    .then(({ user, posts }) => {
        renderProfile(user, posts);
    });
```

### Error Handling

Errors propagate down the chain until caught by a `.catch()`:

```javascript
doSomething()
    .then(() => doSomethingElse())
    .then(() => {
        throw new Error("Something went wrong");
    })
    .then(() => doThirdThing())                 // skipped
    .catch(error => {
        console.error(error.message);           // handles the error
    });
```

**Recovering from errors:**

```javascript
fetch("/api/users")
    .then(response => {
        if (!response.ok) {
            throw new Error(`HTTP ${response.status}`);
        }
        return response.json();
    })
    .catch(error => {
        console.warn("Using cached data:", error);
        return getCachedUsers();                // fallback
    })
    .then(users => {
        renderUsers(users);                     // runs with real or cached data
    });
```

**The two forms of .catch:**

```javascript
// Form 1 — chained catch
promise.then(onFulfilled).catch(onRejected);

// Form 2 — second argument to then
promise.then(onFulfilled, onRejected);
```

Form 1 catches errors in `onFulfilled` as well. Form 2 does not:

```javascript
Promise.resolve("ok")
    .then(
        v => { throw new Error("in handler"); },   // error thrown
        e => console.log("This won't run")
    )
    .catch(e => console.log("Caught in catch"));   // "Caught in catch"

// With catch:
Promise.resolve("ok")
    .then(v => { throw new Error("in handler"); })
    .catch(e => console.log("Caught"));            // "Caught"
```

**Finally — cleanup that runs regardless:**

```javascript
showLoadingSpinner();

fetch("/api/data")
    .then(response => response.json())
    .then(data => renderData(data))
    .catch(error => showError(error))
    .finally(() => {
        hideLoadingSpinner();                       // always runs
    });
```

`.finally()` passes through the result or error unchanged:

```javascript
Promise.resolve(42)
    .finally(() => console.log("cleanup"))
    .then(v => console.log(v));                    // 42

Promise.reject("err")
    .finally(() => console.log("cleanup"))
    .catch(e => console.log(e));                   // "err"
```

### Promise Static Methods

**Promise.resolve** — create an already-fulfilled Promise:

```javascript
const cached = cache.get(key);
return cached
    ? Promise.resolve(cached)
    : fetchFromServer(key);
```

**Promise.reject** — create an already-rejected Promise:

```javascript
function validateUser(user) {
    if (!user.email) {
        return Promise.reject(new Error("Email required"));
    }
    return saveUser(user);
}
```

**Promise.all** — wait for all Promises, reject if any rejects:

```javascript
const [users, posts, comments] = await Promise.all([
    fetch("/api/users").then(r => r.json()),
    fetch("/api/posts").then(r => r.json()),
    fetch("/api/comments").then(r => r.json())
]);
```

If any Promise rejects, `Promise.all` rejects immediately (fail-fast).

**Promise.allSettled** — wait for all, never reject:

```javascript
const results = await Promise.allSettled([
    fetch("/api/users").then(r => r.json()),
    fetch("/api/failing-endpoint").then(r => r.json()),
    fetch("/api/posts").then(r => r.json())
]);

for (const result of results) {
    if (result.status === "fulfilled") {
        console.log("Data:", result.value);
    } else {
        console.warn("Failed:", result.reason);
    }
}
```

**Promise.race** — first settled Promise wins:

```javascript
const result = await Promise.race([
    fetch("/api/data"),
    delay(5000).then(() => { throw new Error("Timeout"); })
]);
```

**Promise.any** — first fulfilled Promise wins, reject only if all reject:

```javascript
const fastest = await Promise.any([
    fetch("https://mirror1.example.com/data"),
    fetch("https://mirror2.example.com/data"),
    fetch("https://mirror3.example.com/data")
]);
```

### Async/Await

Async/await is syntactic sugar over Promises — it makes asynchronous code read like synchronous code.

**`async` function:**

```javascript
async function getData() {
    return 42;                              // automatically wrapped in Promise
}

getData().then(v => console.log(v));        // 42
```

An `async` function always returns a Promise. If the return value is not a Promise, it is wrapped in `Promise.resolve()`.

**`await` expression:**

```javascript
async function loadUser(id) {
    const response = await fetch(`/api/users/${id}`);
    const user = await response.json();
    return user;
}

const user = await loadUser(42);
console.log(user.name);
```

`await` pauses the execution of the async function until the Promise settles. It does not block the main thread.

**Converting Promise chains to async/await:**

```javascript
// Promise chain
fetch("/api/users/42")
    .then(r => r.json())
    .then(user => fetch(`/api/posts?userId=${user.id}`))
    .then(r => r.json())
    .then(posts => renderPosts(posts))
    .catch(handleError);

// Async/await
async function loadUserPosts(userId) {
    try {
        const userResponse = await fetch(`/api/users/${userId}`);
        const user = await userResponse.json();

        const postsResponse = await fetch(`/api/posts?userId=${user.id}`);
        const posts = await postsResponse.json();

        renderPosts(posts);
    } catch (error) {
        handleError(error);
    }
}
```

**Await at top level (modules):**

In modules, top-level await is supported:

```javascript
// In a module
const config = await fetch("/config.json").then(r => r.json());
export default config;
```

Outside modules, wrap in an async IIFE:

```javascript
(async () => {
    const data = await fetch("/api/data").then(r => r.json());
    console.log(data);
})();
```

### Error Handling with Async/Await

**Try/catch:**

```javascript
async function loadData() {
    try {
        const response = await fetch("/api/data");
        if (!response.ok) {
            throw new Error(`HTTP ${response.status}`);
        }
        return await response.json();
    } catch (error) {
        console.error("Failed to load data:", error);
        return null;
    }
}
```

**Multiple awaits — one try/catch covers all:**

```javascript
async function loadDashboard() {
    try {
        const user = await fetchUser();
        const posts = await fetchPosts(user.id);
        const notifications = await fetchNotifications(user.id);
        return { user, posts, notifications };
    } catch (error) {
        console.error("Dashboard load failed:", error);
        showErrorPage();
    }
}
```

**Catching specific errors:**

```javascript
class NetworkError extends Error { name = "NetworkError"; }
class AuthError extends Error { name = "AuthError"; }

async function loadSecureData() {
    try {
        return await fetch("/api/secure-data").then(r => r.json());
    } catch (error) {
        if (error instanceof AuthError) {
            redirectToLogin();
        } else if (error instanceof NetworkError) {
            showOfflineMessage();
        } else {
            throw error; // rethrow unexpected errors
        }
    }
}
```

### Concurrency Patterns

**Sequential — wait for one, then next:**

```javascript
// Total time: sum of all request times
async function sequential() {
    const a = await fetchA();
    const b = await fetchB();          // waits for fetchA
    const c = await fetchC();          // waits for fetchB
    return { a, b, c };
}
```

**Parallel — start all at once, wait for all:**

```javascript
// Total time: max of all request times
async function parallel() {
    const [a, b, c] = await Promise.all([
        fetchA(),
        fetchB(),
        fetchC()
    ]);
    return { a, b, c };
}
```

**Mixed — parallel groups with sequential dependencies:**

```javascript
async function mixed() {
    // Step 1: parallel fetch of independent data
    const [user, config] = await Promise.all([
        fetchUser(),
        fetchConfig()
    ]);

    // Step 2: depends on Step 1
    const dashboard = await fetchDashboard(user.id, config.locale);

    // Step 3: parallel, depends on Step 2
    const [notifications, suggestions] = await Promise.all([
        fetchNotifications(user.id, dashboard.lastSeen),
        fetchSuggestions(dashboard.categories)
    ]);

    return { user, config, dashboard, notifications, suggestions };
}
```

**Limited concurrency — run N at a time:**

```javascript
async function runWithConcurrency(tasks, limit) {
    const results = [];
    const executing = new Set();

    for (const task of tasks) {
        const promise = task().then(result => {
            executing.delete(promise);
            return result;
        });

        executing.add(promise);
        results.push(promise);

        if (executing.size >= limit) {
            await Promise.race(executing);
        }
    }

    return Promise.all(results);
}

const urls = [...]; // 100 URLs
const fetchTasks = urls.map(url => () => fetch(url));
const responses = await runWithConcurrency(fetchTasks, 5); // 5 at a time
```

### The Event Loop

The event loop is the mechanism that allows JavaScript to be non-blocking despite being single-threaded.

```
┌─────────────────────────┐
│ while (queue.waitFor()) {│
│   queue.processTask();   │ ← processes one macrotask
│   queue.processAll       │
│     .microtasks();       │ ← processes all microtasks
│ }                        │
└─────────────────────────┘
```

**Timeline of an async operation:**

```javascript
console.log("1: sync");

setTimeout(() => console.log("2: macrotask"), 0);

Promise.resolve().then(() => console.log("3: microtask"));

console.log("4: sync");

// Output:
// 1: sync
// 4: sync
// 3: microtask
// 2: macrotask
```

### Microtasks vs. Macrotasks

**Microtasks** (processed immediately after each macrotask): Promise callbacks, `queueMicrotask`, `MutationObserver`.
**Macrotasks** (one per event loop iteration): `setTimeout`, `setInterval`, I/O callbacks, UI rendering.

```javascript
console.log("A");

setTimeout(() => console.log("B"), 0);

Promise.resolve()
    .then(() => console.log("C"))
    .then(() => console.log("D"));

console.log("E");

// Output: A, E, C, D, B
```

**Understanding the order:**

1. All synchronous code runs (macrotask)
2. All microtasks are drained (Promises, etc.)
3. Next macrotask is picked from the queue (setTimeout, etc.)

This means Promise callbacks always run before `setTimeout(0)` callbacks.

### Promise Anti-Patterns

**The explicit Promise constructor (when not wrapping callbacks):**

```javascript
// ❌ Bad — unnecessary wrapper
const promise = new Promise((resolve) => {
    resolve(fetch("/api/data"));
});

// ✅ Good — fetch already returns a Promise
const promise = fetch("/api/data");
```

**Promise nesting instead of chaining:**

```javascript
// ❌ Bad — nesting creates Pyramid of Doom
loadUser().then(user => {
    loadPosts(user.id).then(posts => {
        render(user, posts);
    });
});

// ✅ Good — flat chain
loadUser()
    .then(user => loadPosts(user.id))
    .then(posts => render(user, posts));
```

**Swallowing errors:**

```javascript
// ❌ Bad — catch does nothing
fetch("/api/data")
    .catch(() => {});                         // error silently ignored

// ✅ Good — at least log
fetch("/api/data")
    .catch(error => {
        console.error("Request failed:", error);
    });
```

**Forgetting to return in a chain:**

```javascript
// ❌ Bad — missing return
fetch("/api/users")
    .then(response => {
        response.json();                       // missing return — chain gets undefined
    })
    .then(data => {
        console.log(data);                     // undefined
    });

// ✅ Good — return the Promise
fetch("/api/users")
    .then(response => response.json())
    .then(data => console.log(data));
```

**Creating dangling Promises:**

```javascript
// ❌ Bad — error not caught
function dangerous() {
    return fetch("/api/data")
        .then(r => r.json())
        .then(processData);
    // If processData throws, caller handles it
}

// ✅ Good — consistent error handling
function dangerous() {
    return fetch("/api/data")
        .then(r => r.json())
        .then(processData)
        .catch(error => {
            console.error("Process failed:", error);
            throw error;                       // rethrow for caller
        });
}
```

## Glossary

| Term | Definition |
|------|------------|
| Promise | An object representing the eventual completion or failure of an async operation |
| Pending | Initial state of a Promise — not yet settled |
| Fulfilled | Promise resolved successfully |
| Rejected | Promise failed with an error |
| Settled | Either fulfilled or rejected |
| Resolve | Callback that fulfills a Promise |
| Reject | Callback that rejects a Promise with an error |
| then | Method to handle Promise fulfillment |
| catch | Method to handle Promise rejection |
| finally | Method that runs regardless of settlement |
| async | Keyword that marks a function as asynchronous |
| await | Keyword that pauses execution until a Promise settles |
| Event loop | Mechanism that processes tasks and microtasks |
| Microtask | A task queued by Promise handlers, processed before next macrotask |
| Macrotask | A task like setTimeout, I/O, UI rendering |
| Callback hell | Deeply nested callbacks making code unreadable |
| Race condition | Behavior depending on unpredictable timing of operations |
| Exponential backoff | Delaying retries with exponentially increasing intervals |

## Quick References

- [MDN: Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
- [MDN: Async function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)

## Next Steps

- [Fetch API & HTTP](fetch-api.md)
