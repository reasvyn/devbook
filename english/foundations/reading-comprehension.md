# Reading Comprehension

## Description

Reading comprehension is the ability to understand what you read — to extract meaning, identify the main idea, follow instructions, and recognize the relationships between sentences. In software development, you read constantly: documentation, error messages, specifications, code, and conversations. This document teaches you how to read technical English actively and extract meaning efficiently.

## Prerequisites

- [Reading from Scratch](reading-from-scratch.md) — ability to decode English words
- [Vocabulary for Beginners](vocabulary-for-beginners.md) — familiarity with core technical vocabulary

## Table of Contents

- [Why Comprehension Matters](#why-comprehension-matters)
- [Active Reading vs Passive Reading](#active-reading-vs-passive-reading)
- [Identifying the Main Idea](#identifying-the-main-idea)
- [Following Instructions](#following-instructions)
- [Understanding Cause and Effect](#understanding-cause-and-effect)
- [Reading Error Messages Carefully](#reading-error-messages-carefully)
- [Reading Documentation](#reading-documentation)
- [Reading Code with Comments](#reading-code-with-comments)
- [Skimming and Scanning](#skimming-and-scanning)
- [Practice Exercises](#practice-exercises)
- [Learning Tips](#learning-tips)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Why Comprehension Matters

In development, misunderstanding what you read leads to:

| Mistake | Consequence |
|---------|------------|
| Misreading an error message | Spend hours debugging the wrong problem |
| Misunderstanding documentation | Use an API incorrectly, producing bugs |
| Missing a step in instructions | Installation fails, setup is broken |
| Misreading a requirement | Build the wrong feature |
| Ignoring a warning | Security vulnerability or data loss |

Reading comprehension is not a passive skill — it is something you practice and improve. This document teaches you how.

### Active Reading vs Passive Reading

**Passive reading** is letting your eyes move over words without actively processing them. You might finish a paragraph and realize you did not absorb anything.

**Active reading** is engaging with the text as you read. You ask questions, make connections, and check your understanding.

| Passive | Active |
|---------|--------|
| Eyes move over words | Brain processes each sentence |
| Finish a paragraph, remember nothing | Can summarize the paragraph in your own words |
| Read an error message and feel confused | Read an error message and identify what, where, and why |
| Follow instructions blindly | Understand each step before doing it |

**How to read actively:**

1. **Preview.** Before reading deeply, skim the headings, bullet points, and code blocks. Get a map of what is coming.
2. **Question.** Ask: what is this paragraph telling me? What do I already know about this?
3. **Connect.** Link new information to what you already know. "This function is similar to the one I read about yesterday."
4. **Summarize.** After each section, pause and say (or write) what it said in your own words.
5. **Check.** If you cannot summarize it, re-read. Do not move on until you understand.

#### 📖 Reading Speed vs Comprehension

Not every text deserves the same reading speed. The right speed depends on your purpose and the text's difficulty:

| Purpose | Recommended speed | Technique |
|---------|-------------------|-----------|
| Learning a new concept for the first time | Slow | Active reading: question, connect, summarize |
| Reviewing familiar material | Medium | Skim for changes or new information |
| Searching for a specific fact or value | Fast | Scan with `Ctrl+F`, read only surrounding context |
| Deciding whether a document is relevant | Very fast | Skim title, headings, and first sentences only |

**The trade-off is real.** Reading faster reduces comprehension. Research consistently shows an inverse relationship between speed and retention. When you read documentation at twice your normal speed, you will catch roughly half as much detail. The practical implication: slow down when the material is unfamiliar, complex, or has consequences for getting wrong (such as security documentation or deployment instructions). Speed up when you are searching, reviewing, or reading material you already understand well.

**A useful heuristic:** if you can summarize the paragraph without re-reading it, you are reading at the right speed. If you finish a section and cannot recall its main point, you are reading too fast for the material's difficulty.

### Identifying the Main Idea

Every paragraph has a **main idea** — the one thing the author wants you to know. Everything else supports that idea.

**Technique: read the first and last sentence of the paragraph.**

```
Paragraph:
"The map() function takes a callback as its argument. It applies
that callback to every element in the array. The original array
is unchanged. A new array containing all the transformed elements
is returned. For example, mapping x => x * 2 over [1, 2, 3]
produces [2, 4, 6]."

First sentence: "The map() function takes a callback as its argument."
Last sentence: "For example, mapping x => x * 2 over [1, 2, 3] produces [2, 4, 6]."

Main idea: map() takes a function and applies it to every element,
producing a new array with the results.
```

**Practice: identify the main idea of each paragraph.**

**Paragraph 1:**
```
A variable is a named container for data. You create a variable
by giving it a name and a value. In JavaScript, you use the
`let` keyword: `let count = 0;`. The variable `count` now holds
the value `0`. You can change it later: `count = 5;`.
```

Main idea: a variable stores data, created with a name and value.

**Paragraph 2:**
```
Version control tracks every change to your code over time. If a
new change introduces a bug, you can revert to the previous
version. Git is the most widely used version control system.
Every developer should learn it.
```

Main idea: version control (especially Git) lets you track and undo code changes.

### Following Instructions

Technical instructions are step-by-step procedures. The key to following them correctly:

**Rule 1: Read all steps before starting.**

```
Steps to deploy the application:
1. Run `npm install` to install dependencies.
2. Run `npm test` to verify all tests pass.
3. Set the environment variable `NODE_ENV=production`.
4. Run `npm run build` to create the production build.
5. Run `npm start` to launch the server.
```

Before doing step 1, read steps 1–5. You will understand the sequence and catch any prerequisites (like having Node.js installed).

**Rule 2: Do exactly what each step says.**

```
Step: Run `npm install`
What you should type: npm install
What you should NOT type: npm install --save
What you should NOT type: npm i
```

Minor variations (`npm i` vs `npm install`) usually work, but when learning, follow instructions exactly. Deviations can cause unexpected results.

**Rule 3: Check the result after each step.**

```
Step 1: Run `npm install`
Expected: A `node_modules` folder appears, and the terminal shows installation progress.
If you see errors: the step failed. Do not continue until step 1 succeeds.
```

**Rule 4: If a step fails, stop and read the error message.**

```
Step 2 failed:
$ npm test
npm ERR! code ENOENT
npm ERR! syscall package.json
npm ERR! enoent No such file or directory

Analysis: ENOENT means "Error NO ENTry" — the file does not exist.
The command cannot find package.json. You are probably in the wrong directory.
Fix: navigate to the project directory and try again.
```

#### ⚖️ When Instructions Conflict

In practice, two sources often give different instructions for the same task. The official documentation says one thing; a tutorial says another; a colleague recommends a third approach. Here is how to resolve conflicts:

**Step 1: Identify which source is most authoritative.**

| Source type | Authority level | When to trust it |
|-------------|-----------------|------------------|
| Official documentation | Highest | The maintainers wrote it; it reflects the intended usage |
| Package README or CHANGELOG | High | Reflects the current version; may be more recent than docs |
| Tutorial or blog post | Medium | Often dated; may target an older version or a specific use case |
| Stack Overflow answer | Medium | Useful for specific problems, but may not generalize |
| Colleague's advice | Variable | Trustworthy if they have recent experience with the exact tool and version |

**Step 2: Check the dates and versions.** A tutorial from 2022 may describe a workflow that changed in 2024. Always verify that the instructions match the version of the software you are using.

**Step 3: Test the least destructive instruction first.** If two approaches both claim to solve the problem, try the simpler one first. If it works, stop. If it does not, try the other — but do not combine both simultaneously, as they may interfere with each other.

**Step 4: When in doubt, ask — but ask precisely.** Instead of "which way is correct?", state the specific conflict: "The official docs say to use `app.use(express.json())`, but this tutorial uses `body-parser`. Are these equivalent in Express 5?" Precise questions receive precise answers.

### Understanding Cause and Effect

Technical text often describes cause-and-effect relationships. Recognizing these helps you predict outcomes and debug problems.

**Signal words for cause and effect:**

| Cause signal | Effect signal |
|-------------|--------------|
| because | therefore |
| since | as a result |
| due to | consequently |
| caused by | thus |
| if (condition) | then (result) |

**Example:**

```
"Because the input is null, the function throws a TypeError."

Cause: the input is null
Effect: the function throws a TypeError

If you prevent null input, you prevent the error.
```

**Example:**

```
"The server runs out of memory because each request allocates a
new buffer without releasing the old one. As a result, after
approximately 10,000 requests, the process is killed by the OS."

Cause chain:
  1. Each request allocates a buffer (memory used)
  2. Old buffers are not released (memory not freed)
  3. After 10,000 requests, memory is exhausted
  4. The OS kills the process

Solution: release old buffers, or limit concurrent requests.
```

### Reading Error Messages Carefully

Error messages are short, dense, and information-packed. Read every word:

**A real error message:**

```
TypeError: Cannot read properties of undefined (reading 'name')
    at getUserProfile (src/api/users.js:45:20)
    at async GETHandler (src/routes/profile.js:12:5)
```

**Decoding it word by word:**

| Part | What it tells you |
|------|------------------|
| `TypeError` | The category of error — you used a value in a way its type does not allow |
| `Cannot read properties of undefined` | You tried to access a property on something that does not exist |
| `(reading 'name')` | Specifically, you tried to read the `.name` property |
| `at getUserProfile` | The error happened in the function `getUserProfile` |
| `src/api/users.js:45:20` | In the file `users.js`, line 45, column 20 |
| `at async GETHandler` | This function was called by `GETHandler` |
| `src/routes/profile.js:12:5` | Which is in `profile.js`, line 12, column 5 |

**The stack trace reads from bottom to top:** the last line is where the request started; the first line is where it failed. Read from top to bottom to trace the path of the error.

**A Python ImportError:**

```
ImportError: cannot import name 'DataFrame' from 'pandas'
```

| Part | What it tells you |
|------|------------------|
| `ImportError` | Python could not load a module or a name from a module |
| `cannot import name 'DataFrame'` | You asked for `DataFrame` specifically, but it is not available |
| `from 'pandas'` | The module you are importing from is `pandas` |

**Most likely causes:** (1) You misspelled the name — check the exact spelling in the library's documentation. (2) The function or class was renamed or removed in your installed version — check your version with `python -c "import pandas; print(pandas.__version__)"`. (3) The module installed incorrectly — try reinstalling with `pip install --force-reinstall pandas`.

**A JavaScript ReferenceError:**

```
ReferenceError: utils is not defined
    at processItem (src/helpers.js:12:3)
```

| Part | What it tells you |
|------|------------------|
| `ReferenceError` | You referenced a variable or function that does not exist in the current scope |
| `utils is not defined` | The identifier `utils` has not been declared anywhere accessible |
| `at processItem` | The error occurred in the function `processItem` |
| `src/helpers.js:12:3` | In file `helpers.js`, line 12, column 3 |

**Most likely causes:** (1) You forgot to import `utils` at the top of the file. (2) You misspelled the variable name. (3) The import path is wrong, so the import silently failed. (4) The variable is declared in a different scope (such as inside a function) and is not accessible where you are using it.

### Reading Documentation

Documentation is structured text that explains how to use software. Read it strategically:

**The quick-start section** — read first. It tells you how to get the software running.

```
Quick Start:
1. npm install express
2. Create a file called app.js
3. Add the code below
4. Run node app.js
5. Open http://localhost:3000
```

**The API reference** — read when you need a specific function. Do not read it top to bottom. Look up the function you need and read its entry:

```
app.use([path,] callback [, callback])

Parameters:
  path (optional): The path for which the middleware function is invoked.
  callback: The middleware function.

Example:
  app.use('/api', function(req, res, next) {
    next();
  });

Notes:
  - If path is omitted, the middleware applies to all routes.
  - Multiple callbacks can be chained.
```

**The "Notes" and "Caveats" sections** — always read these. They contain the information that prevents bugs:

```
Note: The body-parser middleware must be registered before routes
that use req.body. If registered after, req.body will be undefined.
```

#### 📄 Reading README Files

A README is typically the first file you encounter in a repository. It follows a common structure that, once you recognize it, makes navigation predictable:

| Section | What it contains | When to read it |
|---------|------------------|-----------------|
| **Title and description** | What the project is and what it does | Always — first thing |
| **Installation** | How to install the package | When you want to use it |
| **Quick start / Usage** | A minimal working example | When you want to try it immediately |
| **API reference** | All public functions, classes, and their parameters | When you need a specific feature |
| **Configuration** | Environment variables, config files, options | When the defaults do not work for your case |
| **Examples** | More complete code samples | When the quick start is too minimal |
| **Contributing** | How to contribute to the project | Only if you plan to contribute |
| **License** | Legal terms for using the software | When you need to verify compliance |

**Practical reading strategy:** Read the title and description first to confirm the project is what you think it is. Then jump to the Quick Start section and follow it. If it works, you have enough to begin. Return to the API reference and Configuration sections only when you need to customize behavior.

**A common mistake** is reading the README top to bottom like an article. READMEs are reference documents, not narratives. Skim for the section you need and read that section carefully. Ignore the rest until you need it.

### Reading Code with Comments

Code and its comments together form a single unit of meaning. Read both:

```python
def fibonacci(n):
    # Base cases: the first two Fibonacci numbers are 0 and 1
    if n <= 0:
        return 0
    if n == 1:
        return 1

    # Recursive case: F(n) = F(n-1) + F(n-2)
    # This is correct but inefficient — O(2^n) time
    return fibonacci(n - 1) + fibonacci(n - 2)
```

**How to read this:**

1. Read the function name: `fibonacci` — this calculates Fibonacci numbers
2. Read the first comment: it explains the base cases
3. Read the code: `if n <= 0 return 0` — the 0th Fibonacci number is 0
4. Read the next comment: it explains the recursive formula
5. Read the warning comment: this implementation is correct but slow

**The comment tells you what the code does AND what its weakness is.** Both are important.

### Skimming and Scanning

**Skimming** is reading quickly to get the general idea. Use it to decide if a document is worth reading carefully.

How to skim:
- Read the title and description
- Read all headings
- Read the first sentence of each paragraph
- Look at code blocks and diagrams
- Skip details

**Scanning** is searching for a specific piece of information. Use it when you know what you are looking for.

How to scan:
- Decide exactly what you need (a function name, an error code, a parameter)
- Use `Ctrl+F` to search for it
- Read the surrounding context to confirm it is what you need

**When to use each:**

| Situation | Strategy |
|-----------|----------|
| Choosing which documentation to read | Skim |
| Finding a specific function in the API docs | Scan |
| Debugging an error message | Scan for the error, then skim the explanation |
| Learning a new concept for the first time | Active reading (slow, thorough) |
| Reviewing familiar code | Skim for changes |

#### 🛑 When to Stop Reading

A common beginner mistake is reading an entire document before taking any action. In development, you rarely need to read everything. Stop reading when:

- **You have enough to act.** If you have found the function signature and understand its parameters, you do not need to read the rest of the documentation page. Start coding.
- **You have answered your specific question.** If you were scanning for how to handle a 404 error and you found the answer, close the documentation. Additional reading at this point is procrastination disguised as research.
- **You have verified the version and the example.** If the documentation matches your software version and the example is clear, proceed. Reading more examples of the same concept yields diminishing returns.
- **You feel confident enough to try.** Confidence is a signal. If you believe you understand enough to attempt a solution, attempt it. You will learn more from a five-minute coding attempt than from fifteen more minutes of reading.

**The rule of thumb:** read just enough to take the next concrete step. Then take that step. Then read again as needed. This iterative approach is how experienced developers actually work — they do not read entire manuals before writing a single line of code.

### Practice Exercises

**Exercise 1: Identify the main idea.**

```
A race condition occurs when two or more threads access shared data
concurrently and the final result depends on the timing of their
execution. The most common symptom is an intermittent bug that
appears only under load. Race conditions are notoriously difficult
to reproduce because they depend on specific timing. The standard
solution is to use locks or mutexes to ensure only one thread
accesses the shared data at a time.
```

What is the main idea? Write it in one sentence.

Sample answer: A race condition is a timing-dependent bug caused by concurrent access to shared data, and it is fixed by using locks.

**Exercise 2: Follow these instructions and identify any problems.**

```
1. Clone the repository: git clone https://github.com/example/app.git
2. cd app
3. Install dependencies: npm install
4. Start the server: npm start
5. Open http://localhost:3000 in your browser
```

Is anything missing? Yes — there is no step to set up environment variables. If the app requires a database connection, it will fail at step 4.

**Exercise 3: Read this error and explain what happened.**

```
EACCES: permission denied, open '/var/log/app.log'
```

Answer: The program tried to open (write to) the file `/var/log/app.log` but did not have permission. On Linux, `/var/log/` is owned by root. The program needs to be run with `sudo`, or the log file permissions need to be changed.

## Learning Tips

- **Read error messages word by word.** Every word in an error message is there for a reason. Do not skip any part.
- **Summarize after reading.** After reading a paragraph of documentation, close your eyes and say what it told you. If you cannot, re-read.
- **Practice with real documentation.** Go to the Express.js or Flask documentation and read one page actively. Identify the main idea, the instructions, and the caveats.
- **Read code reviews.** On GitHub, find pull request discussions. See how experienced developers explain their reasoning in English.
- **Use a highlighter (physical or digital).** Highlight the main idea in each paragraph. This forces you to identify it.
- **Keep a vocabulary journal.** When you encounter an unfamiliar technical term, write it down with its definition and the context in which you found it. Review the journal weekly. Technical vocabulary is the foundation of comprehension — if you do not understand the words, you cannot understand the text.
- **Read the same topic from two sources.** If a concept remains unclear after one explanation, find a second explanation from a different author. Different writers use different examples, analogies, and structures. The second explanation often succeeds where the first did not.
- **Practice "read-then-cover."** Read a paragraph, then cover it with your hand and try to state what it said. This technique, sometimes called the Feynman method applied to reading, forces active engagement rather than passive absorption. If you cannot restate it, you did not truly read it.

## Glossary

| Term | Definition |
|------|------------|
| Active reading | Reading with full attention, questioning and summarizing as you go |
| Cause and effect | A relationship where one event triggers another |
| Comprehension | The ability to understand the meaning of text |
| Instruction | A step-by-step procedure to follow |
| Main idea | The central point a paragraph or section communicates |
| Scan | To search quickly for a specific piece of information |
| Skim | To read quickly for general understanding |
| Stack trace | A list of function calls showing the path of an error |
| Technical documentation | Written materials explaining how to use software |

## Quick References

- [How to Read Code](https://swiftable.io/how-to-read-code) — strategies for reading and understanding source code
- [How to Read Technical Papers](https://web.stanford.edu/class/cs83s/reading-how-to-read-a-paper.pdf) — a classic guide by S. Keshav
- [Effective Error Messages](https://www.nngroup.com/articles/error-message-guidelines/) — Nielsen Norman Group guidelines on clear error communication

## Next Steps

- [Writing Basics](writing-basics.md) — learn to produce the technical text you can now understand
- [Grammar & Style](../grammar-and-style/index.md) — refine your English for professional contexts
- [Technical Writing](../technical-writing/index.md) — write documentation, READMEs, and developer guides
- Back to [English Introduction](../intro/index.md)
