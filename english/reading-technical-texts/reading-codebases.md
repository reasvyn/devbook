# Reading Codebases Strategically

## Description

Reading code is a fundamentally different skill from writing code. Understanding how to navigate, comprehend, and extract meaning from unfamiliar codebases is essential for onboarding, code review, debugging, and learning from existing systems. This document covers strategies for approaching unfamiliar codebases, tracing execution paths, using tools for code navigation, understanding project structures, and documenting findings while reading.

## Prerequisites

- [Note-Taking & Knowledge Management](note-taking.md) — documenting codebase discoveries requires a systematic note-taking workflow

## Table of Contents

- [Reading Code vs. Writing Code](#reading-code-vs-writing-code)
- [Top-Down vs. Bottom-Up Reading](#top-down-vs-bottom-up-reading)
- [Tracing Execution Paths](#tracing-execution-paths)
- [Reading Tests as Documentation](#reading-tests-as-documentation)
- [Using Grep and IDE Tools for Navigation](#using-grep-and-ide-tools-for-navigation)
- [Understanding Project Structure Conventions](#understanding-project-structure-conventions)
- [Reading Configuration and Build Files](#reading-configuration-and-build-files)
- [Code Comprehension Techniques](#code-comprehension-techniques)
- [Documenting Findings While Reading](#documenting-findings-while-reading)
- [Building a Code Reading Practice](#building-a-code-reading-practice)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Reading Code vs. Writing Code

Reading code and writing code use different cognitive processes. Writing is generative — you start from a goal and create structure. Reading is analytical — you start from existing structure and reconstruct the goals and intentions.

**Why reading code is harder than writing it:**

| Factor | Explanation |
|---|---|
| Reverse engineering | You must infer intent from implementation, not the other way around |
| No narrative | Code is written for execution, not for sequential reading |
| Missing context | You do not know why decisions were made or what alternatives were considered |
| Noise ratio | Code contains optimizations and historical artifacts that obscure core logic |
| Incomplete understanding | You enter at an arbitrary point and must reconstruct the mental model |

Most developers spend 50-70% of their time reading code but receive almost no training in it. Writing code is taught from day one; reading code is expected to be learned by osmosis.

### Top-Down vs. Bottom-Up Reading

There are two primary strategies for approaching unfamiliar codebases, and effective readers switch between them as needed.

**Top-down reading:**

Start with the high-level architecture and drill into details only when needed.

Steps:
1. Read the project README, architecture documentation, and design docs
2. Understand the directory structure and file organization conventions
3. Identify the entry points (main functions, request handlers, event listeners)
4. Trace a single feature or request through the system at a high level
5. Only examine specific files when you need to understand details

Best for: onboarding, understanding architecture, evaluating before contributing.

**Bottom-up reading:**

Start with specific code elements and build up to understand the system.

Steps:
1. Read a specific function, class, or module in detail
2. Follow imports, calls, and references outward
3. Identify patterns and conventions from the code itself
4. Build a mental model from the ground up
5. Cross-reference with documentation to validate understanding

Best for: debugging a specific issue, verifying component behavior, learning a new paradigm.

**When to use each approach:**

| Situation | Approach |
|---|---|
| First day on a new project | Top-down (read docs, structure) |
| Fixing a bug | Bottom-up (start at the bug) |
| Adding a feature | Top-down (find extension points) |
| Reviewing a pull request | Both (PR description top-down, diff bottom-up) |

**The hybrid approach:** Start top-down for context, switch to bottom-up for details, then alternate. This prevents getting lost in details or staying too abstract.

### Tracing Execution Paths

Tracing execution paths means following the flow of control and data through a codebase.

**The execution path reading method:**

1. Identify the starting point — a main function, event handler, controller method, CLI entry point, or test case.
2. Follow the call chain — trace each function call in order.
3. Track state changes — what variables are modified, what data structures are built.
4. Note branch conditions — at each if/else, switch, or match, understand which path is taken.
5. Record external interactions — database queries, API calls, file I/O.
6. Summarize the path — write a concise summary from start to end.

**Annotated trace example:**

```text
Request: POST /api/users

app.js:21  -> router.post('/api/users', usersController.create)
                |
                v
usersController.js:45  -> validate(req.body)          // validate input
                |        if invalid -> 400 response
                v
usersController.js:52  -> hashPassword(req.body.password)
                |
                v
usersService.js:12  -> db.users.create({...})          // INSERT INTO users
                |        if duplicate email -> 409
                v
usersService.js:20  -> emailService.sendWelcome(user)  // side effect
                |
                v
usersController.js:58  -> res.status(201).json(user)   // response
```

**Tools for tracing execution:**

| Tool type | Examples | When to use |
|---|---|---|
| Debugger | gdb, lldb, Chrome DevTools, VS Code debugger | Step through code interactively |
| Print logging | console.log, printf | Quick tracing without setup |
| Stack traces | Error().stack, panic/recover | Understanding call context |
| Static analysis | CodeQL, grep, call graphs | Finding all callers of a function |
| Dynamic tracing | strace, dtrace, perf | System-level execution trace |

### Reading Tests as Documentation

Tests are the most reliable form of documentation because they cannot lie about what the code does.

**What tests document:**

| Aspect | What tests reveal |
|---|---|
| Expected behavior | What the function is supposed to do |
| Edge cases | Boundary conditions the author considered |
| Error handling | What happens when things go wrong |
| Usage patterns | How the API is intended to be used |
| Contract | Inputs, outputs, preconditions, postconditions |

**Reading tests to understand code:**

1. Find the test file for a module (`__tests__/`, `.test.js`, `*_test.go`)
2. Read the test names — good test names describe the scenario and expected behavior
3. Read the setup section — what state is created, what mocks are used
4. Read the assertion section — what does the test verify

```javascript
// Test name tells you what the code should do
test("returns 400 when email is missing from request body", () => {
  const req = { body: { name: "Alice" } };
  const res = { status: jest.fn().mockReturnThis(), json: jest.fn() };

  createUser(req, res);

  expect(res.status).toHaveBeenCalledWith(400);
  expect(res.json).toHaveBeenCalledWith({
    error: "Email is required",
  });
});
```

**Integration tests as system documentation:**

Integration tests show how components work together — the interaction between modules, data flow across boundaries, and expected sequence of operations. A full integration test for user registration documents the complete lifecycle from signup to login.

### Using Grep and IDE Tools for Navigation

Effective code reading depends on navigating quickly. Modern tools make this possible at any scale.

**Essential code navigation operations:**

| Operation | Tool | Purpose |
|---|---|---|
| Find definition | IDE (Go to Definition), ctags | Where is a function/variable defined? |
| Find references | IDE (Find All References), ripgrep | Where is a function/variable used? |
| Search by pattern | ripgrep, git grep | Find code matching a pattern |
| Search file names | find, fd | Find files by name |
| Search by commit | git log -p, git blame | When was code introduced and why? |

**Grep patterns for code reading:**

```bash
# Find function definition
rg "^func.*calculateTotal" src/

# Find all calls to a function
rg "calculateTotal\(" src/

# Find error messages (good for error handling understanding)
rg "Error:" src/ --type js

# Find TODO and FIXME comments (historical context)
rg "TODO|FIXME|HACK|XXX" src/

# Exclude test files to focus on production code
rg "database" src/ --glob '!**/*.test.*'
```

**IDE navigation shortcuts (VS Code):**

```
F12         Go to Definition
Shift+F12   Go to References
Alt+F12     Peek Definition (inline)
Ctrl+Shift+F Search in all files
Ctrl+Click  Follow link/symbol
Ctrl+-      Navigate back
```

### Understanding Project Structure Conventions

Every codebase follows conventions for organizing files. Understanding the convention helps you predict where to find things.

**MVC (Model-View-Controller):**

```
mvc-app/
├── models/       # Data models and business logic
├── views/        # Presentation layer (templates, UI)
├── controllers/  # Request handling and routing
├── routes/       # URL routing configuration
├── middleware/   # Request processing pipeline
└── config/       # Configuration files
```

Reading MVC: start with controllers (they define the API surface), trace to models (business logic), check views (output).

**Feature-based structure:**

```
feature-app/
├── users/
│   ├── usersController.js
│   ├── usersModel.js
│   ├── usersRoutes.js
│   └── users.test.js
└── products/
    ├── productsController.js
    ├── productsModel.js
    └── productsRoutes.js
```

Each directory is self-contained. Start with the feature you care about.

**Monorepo structure:**

```
monorepo/
├── packages/
│   ├── api/         # API service
│   ├── web/         # Web frontend
│   └── shared/      # Shared types and utilities
├── tools/           # Build tools and scripts
└── docs/            # Documentation
```

Reading monorepos: root config files tell you the workspace structure. Each package is independent. Start with the package entry point.

**Common structural clues:**

| File or directory | What it tells you |
|---|---|
| `index.js` or `index.ts` | Module entry point, re-exports public API |
| `main.go` or `main.py` | Application entry point |
| `internal/` (Go) | Not importable outside module |
| `src/` | Source code root |
| `vendor/` or `node_modules/` | Third-party dependencies |
| `migrations/` | Database schema changes |
| `.github/` | CI/CD workflows and templates |

### Reading Configuration and Build Files

Configuration and build files define how a project is built, tested, deployed, and run. They are often the first files to read.

**package.json (Node.js):**

```json
{
  "name": "my-service",
  "main": "dist/index.js",
  "scripts": {
    "dev": "ts-node-dev src/index.ts",
    "build": "tsc",
    "test": "jest --coverage"
  },
  "dependencies": {
    "express": "^4.18.0",
    "pg": "^8.11.0"
  }
}
```

What this tells you: entry point is `dist/index.js` (compiled from TypeScript), uses Express and PostgreSQL, tests with Jest.

**Dockerfile:**

```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

FROM node:20-alpine AS runner
WORKDIR /app
COPY --from=builder /app/dist ./dist
EXPOSE 3000
CMD ["node", "dist/index.js"]
```

What this tells you: multi-stage build, uses `npm ci`, app runs on port 3000.

**Reading build configs systematically:**

1. Identify the build tool (Make, npm, Maven, Cargo, etc.)
2. Find the entry point and output directory
3. Identify the language version and compiler options
4. Understand the dependency structure
5. Check for custom build steps (code generation, asset processing)

### Code Comprehension Techniques

Beyond navigation, code reading requires techniques to understand what code does.

**The Rubber Duck method:**

Walk through the code line by line, explaining what each line does out loud. When you cannot explain a line, you have found a gap in your understanding.

```javascript
function calculateDiscount(price, tier) {
  // "The function takes a price and a customer tier"
  let discount = 0;
  // "Initialize discount to zero"

  if (tier === "gold") {
    discount = 0.2;
    // "Gold tier gets 20% discount"
  } else if (tier === "silver") {
    discount = 0.1;
    // "Silver tier gets 10% discount"
  }

  return price * (1 - discount);
  // "Return the price after applying the discount"
}
```

**The Simplify-Rename-Restructure technique:**

When code is complex, simplify it mentally without changing behavior.

1. Simplify: replace complex expressions with their results
2. Rename: rename variables to be more descriptive
3. Restructure: flatten nested conditionals

```javascript
// Original
function process(data) {
  let x = [];
  for (let i = 0; i < data.length; i++) {
    if (data[i].active) {
      x.push({ id: data[i].id, n: data[i].name.toUpperCase() });
    }
  }
  return x.sort((a, b) => a.n.localeCompare(b.n));
}

// Simplified understanding
function getActiveUsersSortedByName(users) {
  let activeUsers = [];
  for (let i = 0; i < users.length; i++) {
    if (users[i].active) {
      activeUsers.push({
        id: users[i].id,
        name: users[i].name.toUpperCase(),
      });
    }
  }
  return activeUsers.sort((a, b) => a.name.localeCompare(b.name));
}
```

**The Ask-Why chain:**

For every line of code, ask "why is this here?" and trace the answer backward:

```
Line:   if (attempts > 3) return null;

Why?   To limit retry attempts
Why?   To prevent infinite loops during network failures
Why?   The upstream service has no timeout guarantee

-> Understanding: this is defensive code against a specific external dependency
```

**Annotated reading:**

Read with a pen (or text editor) and annotate as you go:

```javascript
async function processPayment(orderId, paymentMethod) {
  // ENTRY POINT: called from checkoutController.js:45

  const order = await db.orders.findById(orderId);
  // QUERY: fetches order with items, total, status

  if (!order) {
    // EDGE CASE: order not found — should not happen if validation works
    throw new AppError(404, "Order not found");
  }

  if (order.status !== "pending") {
    // EDGE CASE: payment already processed or cancelled
    throw new AppError(409, "Order is not in pending state");
  }

  const result = await paymentGateway.charge({
    // DEPENDENCY: Stripe SDK
    amount: order.total,
    source: paymentMethod.token,
  });
}
```

### Documenting Findings While Reading

Reading code is useless if you forget what you learned.

**Code reading notes template:**

```markdown
# Code Reading: Payment Service

Source: src/services/payment.js (v2.1.0)

## Entry Points
- `processPayment(orderId, paymentMethod)` — called from checkout

## Data Flow
1. Fetch order from database
2. Validate order status is "pending"
3. Charge via Stripe SDK
4. On success: update order status, send email

## Key Dependencies
- Stripe SDK (payment processing)
- PostgreSQL (order storage)

## Design Decisions (inferred)
- No retry logic: assumes client handles retries
- Synchronous charge: blocks until gateway responds

## Questions
- What happens if email sending fails after payment succeeds?
- How are refunds handled?
```

**The code reading log:**

Maintain a running log as you read:

```text
2026-06-29 10:00 - Started reading payment service
  - Entry point: processPayment in paymentService.js
  - Calls: db.orders.findById, paymentGateway.charge
  - Unclear: what happens if Stripe timeout?

2026-06-29 10:30 - Found the timeout handling
  - It is in the Stripe SDK config (stripe.js:15)
  - Default timeout is 30 seconds
```

**Diagrams from code:**

Creating diagrams as you read consolidates understanding:

```text
                    +-------------------+
                    |  POST /checkout   |
                    +--------+----------+
                             |
                             v
                    +-------------------+
                    | processPayment()  |
                    +--------+----------+
                             |
               +-------------+-------------+
               |             |             |
               v             v             v
        +----------+  +----------+  +------------+
        | db.orders|  |  Stripe  |  | emailSvc   |
        | findById |  |  charge  |  | sendReceipt|
        +----------+  +----------+  +------------+
```

### Building a Code Reading Practice

Code reading improves with deliberate practice.

**A code reading routine:**

```text
Weekly code reading habit (30-60 minutes):

Monday:   Pick one module from a project you use but have not read.
          Read the entry point and exports.

Tuesday:  Read the tests for that module.
          Note the edge cases and usage patterns.

Wednesday: Trace one execution path from entry to output.
           Draw a flow diagram.

Thursday: Read the configuration and build files.

Friday:   Write a summary. Identify one question and find the answer.
```

**Curated code reading list:**

| Category | Examples | What to learn |
|---|---|---|
| Well-known libraries | lodash, Redis, SQLite | Clean API design, efficient implementation |
| Standard library | Go stdlib, Python stdlib | Idiomatic code for the language |
| Tools you use daily | Git, curl | Understanding your toolchain |
| Your own past code | Projects from 1-2 years ago | Recognizing improvement opportunities |

**Reading comprehension checklist:**

- Can you explain the code to a colleague without the code in front of you?
- Can you write a test based on your understanding?
- Can you identify the design decisions and trade-offs?
- Can you spot potential bugs the author missed?
- Can you describe how this code fits into the larger system?

## Glossary

| Term | Definition |
|---|---|
| Bottom-up reading | Starting from specific code elements and building up to understand the system |
| Call graph | A diagram showing which functions call which other functions |
| Code navigation | The process of moving through a codebase to find definitions, references, and related code |
| Entry point | The first code that runs when a program or module is executed |
| Execution path | The sequence of function calls that execute for a given input |
| MVC | Model-View-Controller — a pattern separating data, presentation, and control logic |
| Monorepo | A single repository containing multiple related projects or packages |
| ripgrep | A fast command-line search tool optimized for code search |
| Top-down reading | Starting with high-level architecture and drilling into details as needed |
| Rubber duck method | Explaining code line by line to identify gaps in understanding |

## Quick References

- [How to Read Code (Wikipedia)](https://en.wikipedia.org/wiki/How_to_read_code) — overview of code reading techniques
- [The Programmer's Brain (Felienne Hermans)](https://www.manning.com/books/the-programmers-brain) — cognitive science of reading and understanding code
- [ripgrep](https://github.com/BurntSushi/ripgrep) — fast code search tool
- [Code Navigation in VS Code](https://code.visualstudio.com/docs/editor/editingevolved) — IDE features for code reading
- [Working Effectively with Legacy Code (Feathers)](https://www.oreilly.com/library/view/working-effectively-with/0131177052/) — techniques for reading and modifying legacy codebases

## Next Steps

- [How to Read a Research Paper](research-papers.md) — applying systematic reading strategies to academic papers
- [Reading Technical Specifications](specifications.md) — navigating formal specifications and design documents
