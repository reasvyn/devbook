# What Is Computer Science?

## Description

Computer science is the study of computation — what can be computed, how to compute it efficiently, and how to represent and manipulate information. It gives every developer a mental toolbox for breaking down complex problems, recognizing patterns, abstracting away irrelevant details, and designing precise solutions that a machine can execute.

## Prerequisites

- None

## Table of Contents

- [What Is Computation?](#what-is-computation)
- [Computer Science vs. Programming vs. Software Engineering](#computer-science-vs-programming-vs-software-engineering)
- [Major Areas of Computer Science](#major-areas-of-computer-science)
- [Computational Thinking](#computational-thinking)
- [Binary and Data Representation](#binary-and-data-representation)
- [Why CS Matters for Developers](#why-cs-matters-for-developers)

## Content / Material

### What Is Computation?

At its core, computer science asks one question: **what can be automated?** Computation is the process of transforming input into output by following a precise sequence of instructions. Every program you have ever written is a computation — it takes some input (keyboard presses, mouse clicks, network packets, sensor data) and produces some output (pixels on a screen, a saved file, an HTTP response).

CS is not about computers. Computers are just the physical devices that carry out computations. The same principles of computation apply whether you are running code on a $0.50 microcontroller, a cloud server, or a hypothetical machine built from turing-complete LEGO blocks. Computer science studies the *ideas behind* the machine — what problems can be solved, how many steps a solution requires, and how much memory it needs.

**A quick thought experiment.** Imagine you are given two jars of marbles and you need to determine whether they contain the same number of marbles without counting. You could take one marble from jar A and one from jar B, pair them, and repeat. If every marble in A finds a partner in B and nothing is left over, the jars are equal. This is a *computational procedure* — a sequence of steps that, when followed faithfully, answers a question. The procedure does not depend on the size of the jars, the color of the marbles, or whether the jars are glass or clay. The procedure is abstract; the marbles are just data.

CS generalizes procedures like this into *algorithms* — step-by-step recipes that work for any valid input. The marble-pairing procedure is an algorithm for checking set equality. It generalizes to checking whether two lists contain the same elements, whether two database tables have matching rows, or whether two network nodes have synchronized state.

```
Marble-pairing algorithm (set equality):

1. If jar A is empty AND jar B is empty → jars are equal.
2. If jar A is empty XOR jar B is empty → jars are not equal.
3. Remove one marble from A and one from B, pairing them.
4. Go to step 1.
```

This algorithm terminates (it always finishes), it is deterministic (each step is unambiguous), and it is correct for all jar sizes. Those properties — termination, determinism, correctness — are the bedrock of computer science.

### Computer Science vs. Programming vs. Software Engineering

These three terms are often used interchangeably, but they describe different activities with different goals.

| Activity | Goal | Question |
|---|---|---|
| **Computer Science** | Understand what is computable and how to compute it efficiently | "Can this problem be solved, and what is the best way?" |
| **Programming** | Express a solution in a language a computer can execute | "How do I tell the machine what to do?" |
| **Software Engineering** | Build reliable, maintainable systems that solve real-world needs | "How do I deliver value at scale, on time, and with quality?" |

**Computer science is the theory.** It provides the intellectual foundation: data structures, algorithms, computational complexity, formal languages, automata theory, and the mathematical models that describe what computation can and cannot do.

**Programming is the craft.** It takes the abstract ideas from CS and translates them into a concrete language — Python, JavaScript, Rust, C — that a computer can interpret or compile. Programming asks: "Given this algorithm, how do I write it so the machine runs it correctly?" A programmer must also deal with the quirks of real hardware, operating systems, and language runtimes.

**Software engineering is the discipline.** It adds project management, testing, version control, code review, architecture, deployment, monitoring, and team coordination on top of programming. A software engineer asks: "How do I build this so it does not break when 10,000 users hit it simultaneously, so another developer can read my code six months from now, and so I can ship updates without downtime?"

You cannot be a strong software engineer without knowing how to program. You cannot program well without understanding CS principles — if you do not understand what an array is or how big-O notation works, you will write slow, fragile code. But CS alone, without programming practice, produces theory disconnected from real machines. And programming without engineering discipline produces code that works today but collapses under the weight of its own complexity tomorrow.

**A concrete example.** Suppose you need to write a function that finds a name in a phone book.

- **CS thinking:** "A phone book is sorted alphabetically. A binary search compares the middle entry and eliminates half the remaining entries each step, giving O(log n) time. This is optimal for sorted data."
- **Programming:** You write the binary search in Python, handling edge cases like an empty list or names that do not exist.
- **Software engineering:** You write tests for the binary search, handle international name collation, document the function signature, add error handling for corrupted input, and integrate it into a larger contact-management system with a REST API.

None of these three is "better" than the others. They are complementary layers. A well-rounded developer cultivates all three.

### Major Areas of Computer Science

Computer science is broad. Here are the major subfields and what each studies.

#### Algorithms and Data Structures

The core of CS. Algorithms are step-by-step procedures for solving problems. Data structures are ways of organizing data so that algorithms can operate on them efficiently. Sorting a list, searching a tree, finding the shortest path in a graph, compressing a file, encrypting a message — every non-trivial program relies on algorithms and data structures.

**Classic examples:**

| Data Structure | Common Use |
|---|---|
| Array | Contiguous storage, fast index access |
| Linked List | Insert-heavy workloads, no fixed size |
| Hash Table | O(1) average lookups by key |
| Binary Search Tree | Sorted data with O(log n) operations |
| Stack | Undo/redo, expression parsing |
| Queue | Task scheduling, BFS traversal |
| Graph | Social networks, routing, dependency resolution |

**Why it matters:** Every time you use `Dict` in Python, `HashMap` in Rust, or `map` in Go, you rely on a hash table. Every time you sort with `.sort()`, a comparison sort runs in O(n log n) time. Understanding the trade-offs between these structures is the difference between a script that finishes in one second and one that runs for an hour.

#### Programming Languages and Compilers

This area studies how we communicate with machines. PL researchers design new languages (Rust, Go, TypeScript, Carbon) and analyze their properties — type safety, memory safety, concurrency model, expressiveness. Compiler writers turn high-level source code into machine code, tackling problems like lexical analysis, parsing, optimization, register allocation, and code generation.

**Key insight:** Every language is a trade-off. C gives you full control over memory but no safety guarantees. Java uses a garbage collector to manage memory but adds runtime overhead. Rust enforces ownership at compile time, preventing data races without a GC. Haskell uses lazy evaluation and a powerful type system to eliminate side effects. Understanding these trade-offs helps you pick the right tool for the job.

```c
// C — manual memory management, fast but error-prone
char* s = malloc(10);
strcpy(s, "hello");
free(s);
```

```rust
// Rust — ownership ensures memory safety at compile time
let s = String::from("hello");
// freed automatically when s goes out of scope
```

```haskell
-- Haskell — pure functions avoid side effects
hello :: String -> String
hello name = "hello " ++ name
```

#### Operating Systems

OS research studies how software manages hardware resources. An operating system is the layer between your programs and the physical machine. It handles:

- **Process management:** Creating, scheduling, and terminating processes and threads.
- **Memory management:** Allocating RAM to programs, enforcing isolation, handling virtual memory.
- **File systems:** Organizing data on disks so it persists across reboots.
- **Device drivers:** Abstracting hardware (keyboards, GPUs, network cards) behind a uniform API.
- **Security and permissions:** Preventing one process from reading another process's memory.

Every `open()`, `read()`, `write()`, `fork()`, or `mmap()` call you make in a systems language is an interaction with the OS kernel. Even high-level languages like Python and JavaScript call these syscalls under the hood when you read a file or spawn a thread.

#### Computer Networks

Networking is the study of how computers communicate. It covers everything from the electrical signals on a wire (Physical layer) to how a web browser fetches a page over HTTPS (Application layer). The TCP/IP stack is the most important abstraction in modern computing — it allows any two computers on earth to exchange data reliably.

**Key concepts:**

- **Packet switching:** Data is chopped into packets, sent independently, and reassembled at the destination.
- **Protocol layering:** Each layer (physical, link, network, transport, application) provides services to the layer above and hides implementation details from the layer below.
- **Reliability:** TCP ensures packets arrive in order and without loss by using acknowledgments, sequence numbers, and retransmission.

```python
# Simplified TCP handshake as a state machine
CLOSED → SYN_SENT → ESTABLISHED → FIN_WAIT_1 → CLOSED
```

Every web developer works with HTTP (an application-layer protocol), but understanding TCP, DNS, TLS, and routing helps you diagnose slow page loads, configure firewalls, and design distributed systems that tolerate network failures.

#### Artificial Intelligence and Machine Learning

AI and ML are about building systems that learn from data and make decisions. CS contributes the algorithmic foundations — search algorithms, optimization techniques, probability theory, linear algebra — that power modern AI.

**Subfields:**

- **Supervised learning:** Learn a mapping from inputs to outputs given labeled examples (classification, regression).
- **Unsupervised learning:** Find structure in unlabeled data (clustering, dimensionality reduction).
- **Reinforcement learning:** Learn a policy through trial and error with delayed rewards.
- **Deep learning:** Use multi-layer neural networks to model complex patterns in vision, language, and speech.

Every neural network you train is a large graph computation. The forward pass is a sequence of matrix multiplications and activation functions. Backpropagation is dynamic programming applied to the chain rule of calculus. The theoretical foundations of ML are firmly rooted in CS and mathematics.

#### Security and Cryptography

Security studies how to protect data and systems from unauthorized access, tampering, and denial of service. Cryptography provides the mathematical tools — encryption, hashing, digital signatures — that secure communications.

**Core problems:**

- **Confidentiality:** Only the intended recipient can read the message.
- **Integrity:** The message has not been altered in transit.
- **Authentication:** The sender is who they claim to be.
- **Non-repudiation:** The sender cannot deny sending the message.

```python
# Simple symmetric encryption with XOR (educational only)
def xor_encrypt(data: bytes, key: bytes) -> bytes:
    return bytes(d ^ k for d, k in zip(data, key * len(data)))
```

Real-world cryptography uses algorithms like AES (encryption), SHA-256 (hashing), and RSA/ECDSA (signatures). Understanding these primitives helps you avoid common mistakes like rolling your own crypto, using outdated algorithms, or mishandling keys.

#### Databases and Data Management

This area studies how to store, retrieve, and manipulate data efficiently. It covers:

- **Relational databases:** Tables, SQL, ACID transactions, B-tree indexes.
- **NoSQL systems:** Document stores, key-value stores, wide-column stores, graph databases.
- **Query optimization:** How the database chooses an execution plan for a given query.
- **Concurrency control:** Locking, MVCC, isolation levels.

Every CRUD application relies on a database. Understanding indexing, query planning, and transaction isolation helps you write applications that scale to millions of users without crumbling under load.

```sql
-- An index on email makes this lookup O(log n) instead of O(n)
CREATE INDEX idx_users_email ON users(email);
SELECT * FROM users WHERE email = 'alice@example.com';
```

#### Software Engineering and Formal Methods

This area applies CS theory to the practice of building reliable software. It includes:

- **Testing and verification:** Unit tests, integration tests, property-based testing, model checking.
- **Type systems:** Static typing catches entire classes of bugs at compile time.
- **Program analysis:** Linters, static analyzers, symbolic execution.
- **Distributed systems:** Consensus algorithms, replication, fault tolerance.

The line between CS and software engineering blurs here — techniques like formal verification (proving a program correct with mathematical logic) are pure CS, while testing methodologies lean toward engineering practice.

### Computational Thinking

Computational thinking is the mental discipline that computer science cultivates. It is a problem-solving approach that transfers far beyond programming — it is useful in biology, business, law, and everyday life. Computational thinking has four pillars:

#### 1. Decomposition

**Break a complex problem into smaller, manageable pieces.** No one writes a web browser as a single monolithic function. You decompose it into a networking engine, an HTML parser, a CSS layout engine, a JavaScript runtime, a rendering pipeline, and a UI toolkit. Each piece can be built, tested, and understood independently.

**Real-world example:** Planning a wedding. Instead of planning "wedding" as one giant task, decompose it into venue booking, catering, guest list, invitations, music, photography, seating arrangements, and transportation. Each subproblem is solvable on its own.

**In code:**

```python
# Decomposition: break "process user registration" into subtasks
def register_user(email, password):
    if not validate_email(email):
        return Error("Invalid email")
    if not validate_password_strength(password):
        return Error("Weak password")
    hashed = hash_password(password)
    user_id = insert_into_database(email, hashed)
    send_welcome_email(email)
    return User(user_id, email)
```

Each helper function (`validate_email`, `hash_password`, `insert_into_database`, `send_welcome_email`) can be written and tested separately. Decomposition makes the code readable, testable, and maintainable.

#### 2. Pattern Recognition

**Look for similarities that have been solved before.** Most programming problems are variations of a small set of classic patterns. Recognizing those patterns lets you apply known solutions instead of reinventing the wheel.

**Common patterns in CS:**

| Problem | Pattern | Solution |
|---|---|---|
| Finding an item in a collection | Search | Linear search, binary search, hash lookup |
| Processing items in order | Traversal | Iteration, recursion |
| Grouping items by a key | Hashing | Hash table |
| Processing items in FIFO order | Queue | BFS, task queue |
| Processing items in LIFO order | Stack | DFS, undo stack |
| Partitioning data by a condition | Filtering | List comprehension, filter function |

**Real-world example:** A file system directory tree and a company org chart are both trees. If you know how to traverse a tree (DFS or BFS), you can solve both problems — calculating total disk usage under a directory *and* finding all employees reporting to a manager.

```python
# Pattern: tree traversal — works for filesystems AND org charts
def size_of_directory(path: str) -> int:
    total = 0
    for entry in os.scandir(path):
        if entry.is_file():
            total += entry.stat().st_size
        elif entry.is_dir():
            total += size_of_directory(entry.path)  # recurse
    return total

def count_reports(manager_id: str, org: dict) -> int:
    total = 0
    for employee in org.get(manager_id, []):
        total += 1 + count_reports(employee["id"], org)  # recurse
    return total
```

Once you see the tree pattern, you can apply the same traversal strategy to JSON documents, HTML DOM trees, ASTs, and decision trees.

#### 3. Abstraction

**Hide irrelevant details and expose only what matters.** A web browser abstracts away the TCP connection, TLS handshake, HTTP request construction, and HTML parsing when all you want to do is call `fetch(url)`. The `fetch` API is an abstraction — it gives you a simple interface while hiding enormous complexity underneath.

**Levels of abstraction in a typical web request:**

```
Application:  fetch("https://api.example.com/users")
              ↓
HTTP library:  GET /users HTTP/1.1
               Host: api.example.com
              ↓
TLS layer:     Encrypt with session key
              ↓
TCP layer:     SYN → SYN-ACK → ACK, segment data
              ↓
IP layer:      Route packets through gateways
              ↓
Link layer:    Send Ethernet frames on the wire
              ↓
Physical:      Electrical signals on copper / light in fiber
```

Each layer is an abstraction over the one below. Without abstraction, no single developer could understand the full stack. You would need to be an expert in electromagnetism, networking, cryptography, and HTTP semantics just to load a JSON response.

**In code, abstraction means defining clean interfaces:**

```python
# Good abstraction: callers don't need to know the storage backend
class UserRepository:
    def find_by_id(self, user_id: str) -> User | None: ...
    def save(self, user: User) -> None: ...
    def delete(self, user_id: str) -> None: ...

# The implementation could use PostgreSQL, Redis, or an in-memory dict.
# Callers of UserRepository don't care — the abstraction hides it.
```

Abstraction lets you change the implementation without changing the code that uses it. This is the principle behind dependency injection, microservices, and API design.

#### 4. Algorithm Design

**Design a step-by-step solution that can be executed by a computer.** Algorithm design is where decomposition, pattern recognition, and abstraction come together. You decompose the problem, recognize subproblems that match known patterns, abstract away irrelevant details, and assemble the pieces into a precise procedure.

**Designing an algorithm: step by step.**

**Problem:** Given a list of integers, find the two numbers that sum to a target value.

**Step 1 — Decompose:** We need to check every pair of numbers. That is a nested loop. We can also preprocess the list to enable faster lookups.

**Step 2 — Recognize patterns:** This is a "pair search" problem, similar to checking if a set contains a complement. If we use a hash set, we can check membership in O(1) time.

**Step 3 — Abstract:** We do not care about the actual values except whether `target - current` exists in the set. The set does not need ordering or indexing — just fast membership tests.

**Step 4 — Design the algorithm:**

```python
def two_sum(nums: list[int], target: int) -> tuple[int, int] | None:
    seen = set()
    for n in nums:
        complement = target - n
        if complement in seen:
            return (complement, n)
        seen.add(n)
    return None
```

**Analysis:** The algorithm runs in O(n) time and O(n) space. Without the hash set, a brute-force nested loop would take O(n²) time — for an array of 1000 elements, that is 500,000 comparisons instead of 1000.

The four pillars of computational thinking are not sequential steps you follow in order. They are interrelated habits of mind that you exercise simultaneously. When you decompose, you also notice patterns. When you abstract, you design algorithms that work with the abstraction. Over time, these habits become automatic.

### Binary and Data Representation

Computers are physical devices built from billions of tiny switches called transistors. Each switch can be in one of two states: **on** (conducting electricity) or **off** (not conducting). We represent those states as the digits **1** and **0** — hence the term *binary digit* or *bit*.

Everything a computer does — every number, letter, pixel, sound sample, video frame, network packet — is ultimately represented as a sequence of bits. Understanding how data is encoded in binary is fundamental to CS.

#### Numbers: Binary Notation

In everyday life we use decimal (base-10) notation, where each digit position represents a power of 10:

```
354 = 3 × 10² + 5 × 10¹ + 4 × 10⁰
    = 300 + 50 + 4
```

Binary (base-2) works the same way but uses powers of 2 and only the digits 0 and 1:

```
1101₂ = 1 × 2³ + 1 × 2² + 0 × 2¹ + 1 × 2⁰
      = 8 + 4 + 0 + 1
      = 13₁₀
```

**Converting decimal to binary:** Repeatedly divide by 2 and collect the remainders from bottom to top.

```
13 ÷ 2 = 6 remainder 1  ↑
 6 ÷ 2 = 3 remainder 0  |
 3 ÷ 2 = 1 remainder 1  |
 1 ÷ 2 = 0 remainder 1  |
                         1101₂
```

**Why binary matters in practice:** Every integer in your program (an `int` in C, an `i32` in Rust, a `Number` in JavaScript) has a fixed number of bits — typically 32 or 64. This means there is a maximum and minimum value. Overflow happens when a computation produces a value too large for the available bits.

```c
#include <stdio.h>
#include <limits.h>

int main() {
    int x = INT_MAX;  // 2_147_483_647
    printf("%d\n", x + 1);  // -2_147_483_648  (overflow!)
    return 0;
}
```

This happens because adding 1 to the largest positive integer wraps around to the largest negative integer — the bit pattern overflows into the sign bit. Understanding binary representation helps you avoid and debug overflow bugs.

#### Text: ASCII and Unicode

How does a computer represent the letter 'A'? It assigns each character a numeric code point. The **ASCII** standard (1963) defined 128 characters — lowercase and uppercase English letters, digits, punctuation, and control characters — each represented by a 7-bit number.

```
'A' = 65₁₀  = 1000001₂
'B' = 66₁₀  = 1000010₂
'0' = 48₁₀  = 0110000₂
' ' = 32₁₀  = 0100000₂
```

ASCII worked well for English but could not represent characters from other languages. **Unicode** solves this by providing over a million code points covering every writing system in the world, plus emoji and symbols. 'A' is still U+0041, but now we also have:

```
'ñ' = U+00F1 (Spanish)
'中' = U+4E2D (Chinese)
'😀' = U+1F600 (emoji)
```

Unicode code points are typically encoded as UTF-8, UTF-16, or UTF-32. **UTF-8** is the dominant encoding on the web. It uses 1 byte for ASCII characters and up to 4 bytes for others, cleverly packing variable-length sequences so they are backward-compatible with ASCII.

```python
# UTF-8 encoding in action
text = "Hello 世界 🌍"
for char in text:
    encoded = char.encode("utf-8")
    print(f"'{char}' → {encoded.hex()} ({len(encoded)} bytes)")

# Output:
# 'H' → 48 (1 byte)
# '世' → e4b896 (3 bytes)
# '界' → e7958c (3 bytes)
# '🌍' → f09f8c8d (4 bytes)
```

Every web developer has encountered encoding bugs — mojibake (garbled text) happens when a file encoded as UTF-8 is interpreted as Latin-1, or when a database connection uses the wrong charset. Understanding Unicode and UTF-8 is essential for building internationalized applications.

#### Images: Pixels and Color

A digital image is a grid of pixels. Each pixel stores a color, typically represented as three 8-bit values for red, green, and blue (RGB). An 8-bit channel can represent 256 values (0–255), so each pixel uses 24 bits (3 bytes). A 1920×1080 image uses:

```
1920 × 1080 × 3 bytes = 6,220,800 bytes ≈ 6 MB
```

That is why images are compressed — JPEG, PNG, and WebP all use sophisticated algorithms to represent the same visual information in fewer bits by exploiting patterns in human vision (lossy) or statistical redundancy in the pixel data (lossless).

```python
# Representing a 2×2 pixel image as raw RGB bytes
pixels = bytes([
    255, 0, 0,    # top-left: red
    0, 255, 0,    # top-right: green
    0, 0, 255,    # bottom-left: blue
    255, 255, 0,  # bottom-right: yellow
])
```

#### Sound: Sampling and Quantization

Sound is a continuous vibration in air. To represent it digitally, we **sample** the amplitude at regular intervals (usually 44,100 times per second for CD-quality audio) and **quantize** each sample to a fixed number of bits (typically 16 or 24). This is called pulse-code modulation (PCM).

```
Analog sound wave:
    ~/\/\/\/\/\/\/\/\/\~

Samples (16-bit, 44.1 kHz):
    [  1245,  31982,  -8734,  -24567,   7832,    ... ]
```

Higher sample rates capture higher frequencies (Nyquist theorem: you need at least double the highest frequency you want to capture). More bits per sample give a better signal-to-noise ratio. Every MP3, AAC, or Opus file starts as raw PCM samples that are then compressed using perceptual audio coding that discards frequencies humans cannot hear well.

#### Why Data Representation Matters

Every time you:

- Choose `int` vs `float` vs `decimal` for financial calculations
- Pick `utf8mb4` over `latin1` for a database column that will store emoji
- Decide between JPEG and PNG for an application asset
- Set the audio bitrate for a video stream

...you are making a decision about data representation. A developer who understands binary, encoding, and quantization makes better decisions about correctness, performance, and compatibility.

### Why CS Matters for Developers

Every developer, regardless of their specialty, benefits from understanding computer science. Here is why.

#### 1. You Write Faster Code

Without understanding data structures, you might use a list for every collection. Lists are O(n) for membership checks — searching a list of 100,000 items takes 50,000 comparisons on average. A set or hash table does the same check in O(1) time — one operation regardless of size.

```python
# Slow: membership check on a list is O(n)
emails = ["alice@example.com", "bob@example.com", ...]  # 100k emails
if "charlie@example.com" in emails:  # O(n) — 50k comparisons avg
    ...

# Fast: membership check on a set is O(1)
emails_set = set(emails)
if "charlie@example.com" in emails_set:  # O(1) — one hash lookup
    ...
```

CS gives you the vocabulary and the knowledge to make these choices deliberately instead of by accident.

#### 2. You Debug Better

Understanding how the machine represents data helps you diagnose weird bugs. Integer overflow, floating-point precision loss, off-by-one errors in binary search, buffer overflows, race conditions — CS concepts are debugging tools.

```python
# Floating-point surprise: 0.1 + 0.2 ≠ 0.3
print(0.1 + 0.2)  # 0.30000000000000004
```

This is not a bug in Python. It is a consequence of representing decimal fractions in binary floating point (IEEE 754). Understanding floating-point representation saves hours of head-scratching.

#### 3. You Design Better Systems

CS concepts like abstraction, modularity, and layering are essential tools for system design. When you design a microservice architecture, you are decomposing a monolith (decomposition). When you use a message queue, you are applying a FIFO pattern (pattern recognition). When you define a REST API, you are creating an abstraction boundary (abstraction). When you pick a consensus algorithm for a distributed database, you are selecting an algorithm (algorithm design).

#### 4. You Communicate Better

CS gives you a shared vocabulary with other developers. When you say "this is a classic producer-consumer problem with a bounded buffer," other developers immediately understand the concurrency pattern. When you say "this algorithm has exponential time complexity in the worst case," everyone knows it will not scale. Without CS vocabulary, you end up describing everything from scratch.

#### 5. You Future-Proof Your Career

Technology changes fast. Frameworks come and go. But the fundamentals of CS — algorithms, data structures, computation theory, networking, operating systems — change slowly, if at all. A developer who knows CS fundamentals can pick up a new language or framework in weeks because they understand the underlying concepts. A developer who only knows the surface API of a specific framework is stranded when that framework becomes obsolete.

**The half-life of knowledge:**

| Type | Half-life | Examples |
|---|---|---|
| Frameworks | 2–5 years | AngularJS → React → Svelte → ? |
| Languages | 10–20 years | Java, Python, JavaScript endure |
| CS fundamentals | 50+ years | Sorting, search, hashing, TCP, relational theory |

Investing in CS fundamentals is the highest-ROI learning a developer can do. Every hour spent understanding algorithms, data structures, or operating systems pays dividends for the rest of your career.

## Glossary

| Term | Definition |
|---|---|
| Abstraction | Hiding implementation details behind a clean interface so callers only deal with what matters |
| Algorithm | A finite sequence of unambiguous steps that transforms input into output |
| ASCII | American Standard Code for Information Interchange — a 7-bit character encoding for 128 English characters and control codes |
| Big-O notation | A mathematical notation describing the upper bound of an algorithm's time or space growth relative to input size |
| Binary | A base-2 number system using only digits 0 and 1, matching the on/off states of digital circuits |
| Bit | The smallest unit of data in computing — a single binary digit (0 or 1) |
| Byte | A group of 8 bits, enough to represent 256 distinct values or one ASCII character |
| Computation | The process of transforming input into output by following a precise sequence of instructions |
| Computational thinking | A problem-solving approach using decomposition, pattern recognition, abstraction, and algorithm design |
| Data structure | A specialized format for organizing, processing, and storing data (e.g., array, hash table, tree) |
| Decomposition | Breaking a complex problem into smaller, independently solvable subproblems |
| Deterministic | An algorithm that produces the same output given the same input every time |
| Encoding | The rules for converting between abstract characters and their binary representation |
| IEEE 754 | The standard for floating-point arithmetic used by virtually all modern computers |
| O(n) | Linear time complexity — execution time grows proportionally with input size |
| O(log n) | Logarithmic time complexity — execution time grows slowly as input size increases |
| O(n²) | Quadratic time complexity — execution time grows with the square of input size |
| Overflow | When a computation produces a value too large to fit in the available number of bits |
| Pattern recognition | Identifying recurring structures or themes across different problems |
| Pixel | The smallest addressable element in a digital display — a single colored point in an image |
| Protocol | A set of rules governing how two or more entities communicate |
| Quantization | Mapping continuous values to a discrete set of representable values |
| Sample | A discrete measurement of a continuous signal, such as sound amplitude at a point in time |
| Termination | The property that an algorithm finishes after a finite number of steps |
| Transistor | A semiconductor device used as an electronic switch — the fundamental building block of modern computers |
| Unicode | A universal character encoding standard covering over a million characters from all writing systems |
| UTF-8 | A variable-length Unicode encoding that uses 1–4 bytes per character and is backward-compatible with ASCII |

## Quick References

- [Structure and Interpretation of Computer Programs](https://mitpress.mit.edu/sites/default/files/sicp/index.html) — the classic MIT CS text, available free online
- [CS50's Introduction to Computer Science](https://cs50.harvard.edu/) — Harvard's introductory course, freely available
- [Teach Yourself Computer Science](https://teachyourselfcs.com/) — a curated self-study curriculum with book and lecture recommendations
- [Computer Science from the Bottom Up](https://www.bottomupcs.com/) — a free online book covering CS from hardware upwards
- [Big-O Cheat Sheet](https://www.bigocheatsheet.com/) — a quick reference for algorithm time and space complexity

## Next Steps

- [How Computers Work](../fundamentals/how-computers-work.md) — CPU, memory, storage, binary, and the fetch-execute cycle
- [Command Line Basics](../fundamentals/command-line-basics.md) — navigating and controlling your computer through the terminal
- Back to [Computer Science Introduction](index.md)
