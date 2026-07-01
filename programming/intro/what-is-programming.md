# What Is Programming?

## Description

An introduction to the discipline of programming — what it means to write instructions for a computer, how programs are executed, and why programming is a essential skill for developers.

## Prerequisites

- Basic computer literacy — files, folders, running applications

## Table of Contents

- [What Is a Program?](#what-is-a-program)
- [How Computers Execute Programs](#how-computers-execute-programs)
- [Programming Languages](#programming-languages)
- [Compilation vs Interpretation](#compilation-vs-interpretation)
- [The Programming Process](#the-programming-process)
- [Paradigms Overview](#paradigms-overview)
- [Programming in the Real World](#programming-in-the-real-world)
- [Common Misconceptions](#common-misconceptions)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is a Program?

A program is a sequence of instructions that tells a computer what to do. Computers are deterministic machines — they follow instructions exactly as written, without interpretation or judgment. This means a program must be precise, unambiguous, and complete.

At the lowest level, computers understand only machine code: binary instructions (ones and zeros) that directly control the processor. Writing machine code by hand is impractical for anything beyond trivial programs, which is why programming languages exist.

A program takes input data, performs operations on it, and produces output. This input-process-output model is the foundation of almost every program ever written, from a simple calculator to a distributed database.

```
Input → [Program] → Output
```

The program defines the transformation. Without a program, the computer does nothing.

### How Computers Execute Programs

A computer has four main components relevant to program execution:

**CPU (Central Processing Unit)** — the brain. It fetches instructions from memory, decodes them, and executes them. A modern CPU can execute billions of instructions per second.

**Memory (RAM)** — temporary storage that holds the program and its data while it runs. Memory is fast but volatile — when power is lost, everything in RAM disappears.

**Storage (Disk)** — permanent storage for programs and data when they are not running. Storage is slower than memory but persistent.

**I/O Devices** — keyboard, mouse, display, network card, and anything else the computer communicates with.

When you run a program, the operating system loads it from storage into memory, then tells the CPU to start executing its instructions. The CPU reads each instruction from memory, executes it, and moves to the next one. This is called the fetch-decode-execute cycle.

```
┌─────────┐     ┌─────────┐     ┌──────────┐
│  Fetch  │ →  │  Decode  │ →  │ Execute  │
│instruction│   │instruction│   │instruction│
└─────────┘     └─────────┘     └──────────┘
       ↑                             │
       └─────────────────────────────┘
```

The speed of this cycle determines how fast a program runs. Modern CPUs use pipelining (executing multiple instructions at different stages simultaneously), caching (keeping frequently used data close to the CPU), and multiple cores (executing several instruction streams in parallel) to go faster.

### Programming Languages

A programming language is a formal language with a syntax (grammar) and semantics (meaning) that allows humans to express computations. Languages exist at different levels of abstraction:

**Machine code** — raw binary that the CPU executes directly. Every CPU architecture (x86, ARM, RISC-V) has its own machine code.

**Assembly language** — a human-readable representation of machine code using mnemonics like `MOV`, `ADD`, and `JMP`. Each assembly instruction maps directly to one machine instruction. Assembly is still architecture-specific.

```
MOV AX, 5     ; Load the value 5 into register AX
ADD AX, 3     ; Add 3 to AX (result: 8)
```

**High-level languages** — languages like Python, JavaScript, Java, Go, and Rust that use human-readable syntax and abstract away the details of the CPU. A single line of a high-level language can translate to dozens or hundreds of machine instructions.

```python
result = 5 + 3  # Much easier than assembly
```

High-level languages provide:
- **Abstraction** — you describe what you want, not how the CPU should do it
- **Portability** — the same code can run on different architectures
- **Readability** — code is easier to understand, review, and maintain
- **Safety** — the language prevents common errors (type mismatches, memory access violations)

The tradeoff is performance. Well-written assembly or machine code can be faster than equivalent high-level code, but the difference shrinks as compilers improve.

### Compilation vs Interpretation

High-level languages must be translated into machine code before the computer can execute them. There are two main approaches:

**Compiled languages** (C, C++, Go, Rust):

A compiler translates the entire source code into an executable machine code file before it runs. The compiled program is standalone and runs directly on the CPU.

```
Source code → [Compiler] → Executable → [CPU] → Output
```

_Advantages:_ Fast execution, no runtime dependency needed, full optimization.

_Disadvantages:_ Slower development cycle (compile before every test), platform-specific executables.

**Interpreted languages** (Python, JavaScript, Ruby):

An interpreter reads the source code line by line and executes it directly, without producing a standalone executable. The interpreter itself is a compiled program that understands the language at runtime.

```
Source code → [Interpreter] → [CPU] → Output
          ↑ reading line by line
```

_Advantages:_ Faster development cycle, cross-platform (just need the interpreter), dynamic features.

_Disadvantages:_ Slower execution, requires the interpreter to be installed.

**Just-in-Time (JIT) compilation** (Java, C#, modern JavaScript engines):

A hybrid approach. The source code is compiled to an intermediate representation (bytecode), which is then compiled to machine code at runtime by a JIT compiler. This combines the portability of interpretation with near-native performance.

```
Source code → [Compiler] → Bytecode → [JIT Compiler] → Machine code → Output
                                       (at runtime)
```

_Advantages:_ Performance close to compiled languages, cross-platform bytecode.

_Disadvantages:_ Warm-up time while JIT compiles hot code, more complex runtime.

In practice, the line between compiled and interpreted languages has blurred. Python can be compiled to bytecode. JavaScript engines (V8, SpiderMonkey) use JIT compilation. Go compiles quickly but has a runtime. The important concept is that all code must eventually become machine code to run.

### The Programming Process

Writing a program is more than typing code. The typical workflow includes:

**1. Understanding the problem.** What are you trying to solve? What are the inputs and expected outputs? What are the edge cases? This step is often skipped by beginners and causes the most problems.

**2. Designing a solution.** Break the problem into smaller steps. Decide what data structures and algorithms to use. Sketch the architecture. For simple programs, this might be mental. For complex ones, it involves diagrams and written specifications.

**3. Writing code.** Translate the design into a programming language. This is the step most people think of as "programming," but it is only one part.

**4. Testing.** Run the program with various inputs to verify it works correctly. Testing includes unit tests (testing individual functions), integration tests (testing how components work together), and manual testing.

**5. Debugging.** When the program does not work as expected, you must find and fix the bug. Debugging involves reading error messages, inspecting variable values, and reasoning about what the program is actually doing vs what you intended.

**6. Refactoring.** Improving the code without changing its behavior. This makes the code easier to read, maintain, and extend.

**7. Maintenance.** Programs live for years. Bugs are discovered, requirements change, dependencies need updating. Most of the cost of software is in maintenance, not initial development.

### Paradigms Overview

A programming paradigm is a style or way of thinking about programming. Different paradigms emphasize different concepts and lead to different code structures.

**Imperative programming** — the oldest and most intuitive paradigm. You write step-by-step instructions that tell the computer exactly what to do, in order. Code is a sequence of statements that change the program's state.

```python
total = 0
for i in range(10):
    total = total + i
```

**Declarative programming** — you describe what you want, not how to do it. The system figures out the steps. SQL is a classic example.

```sql
SELECT SUM(price) FROM orders WHERE status = 'paid';
```

You describe the result you want. The database engine decides how to compute it.

**Functional programming** — a declarative style where computation is treated as the evaluation of mathematical functions. Functions are pure (no side effects, same input always gives same output). State mutation is avoided.

```python
numbers = [1, 2, 3, 4, 5]
squared = list(map(lambda x: x * x, numbers))
```

**Object-oriented programming** — organizes code around objects that contain data (fields) and behavior (methods). Objects encapsulate state and communicate through method calls.

```python
class Dog:
    def __init__(self, name):
        self.name = name
    
    def bark(self):
        return f"{self.name} says woof!"

rex = Dog("Rex")
print(rex.bark())
```

Most modern languages support multiple paradigms. Python, JavaScript, Kotlin, and Scala let you mix imperative, functional, and OOP styles. The best paradigm depends on the problem and the team.

### Programming in the Real World

Programming in a professional setting involves much more than writing code:

**Version control.** Every change is tracked in Git. You work on branches, open pull requests, and review teammates' code. Never commit directly to the main branch.

**Code reviews.** Every change is reviewed by at least one other developer before it is merged. Reviews catch bugs, improve code quality, and spread knowledge across the team.

**Testing.** Production code has automated tests that run on every change. A change that breaks tests is not merged until fixed.

**Documentation.** Code explains how, but comments and documentation explain why. Good documentation describes the intent, constraints, and edge cases that are not obvious from reading the code.

**Deployment.** Code must be built, tested, and deployed to production. Modern teams deploy multiple times per day using automated CI/CD pipelines.

**Incident response.** When something breaks in production, developers investigate, fix, and write postmortems to prevent recurrence.

**Collaboration.** Programming is a team sport. You work with product managers to understand requirements, with designers to build interfaces, with QA to test features, and with operations to keep systems running.

### How to Choose a Language

With thousands of programming languages in existence, choosing one to learn first can be overwhelming. Here are practical considerations:

**What do you want to build?** Web frontend? JavaScript/TypeScript is the only choice for browsers. Mobile apps? Kotlin (Android) or Swift (iOS). Data science? Python dominates. Systems programming? Rust, C, or Go. Backend services? Python, JavaScript, Go, Java, or C# all work well.

**What does your team use?** If you are joining an existing project, use whatever the project uses. Language preference matters less than team productivity and codebase consistency.

**What is the learning curve?** Python and JavaScript have gentle learning curves and large communities. Rust and C++ have steep curves but offer more control. Start with something approachable unless you have a specific reason not to.

**What is the job market?** In most markets, JavaScript/TypeScript, Python, Java, and C# have the most entry-level positions. Specialized languages (Erlang, Haskell, Prolog) have fewer roles but higher pay for experts.

**What matters most for learning?**

For a first language, prioritize:
- **Quick feedback** — being able to run code and see results immediately
- **Good error messages** — the language should help you understand what went wrong
- **Large community** — plenty of tutorials, Stack Overflow answers, and libraries
- **Gentle progression** — start simple and add complexity as you grow

Python and JavaScript satisfy all four criteria, which is why they are the most recommended first languages.

### Common Misconceptions

**"Programming is about memorizing syntax."** Syntax is the easiest part. You look it up when you need it. The hard parts are problem decomposition, algorithm design, and debugging.

**"You need to be good at math."** Basic arithmetic and logical reasoning are enough for most programming. Advanced math is only needed for specialized fields like graphics, machine learning, or cryptography.

**"Programming is solitary."** Professional programming is highly collaborative. You spend more time reading code, reviewing code, and communicating than writing code.

**"More code is better."** Every line of code is a liability. It must be tested, maintained, and understood. The best code is the code you do not write.

**"Programming is easy once you learn a language."** Learning a language is the first step. The real skill is knowing what to build, how to structure it, and how to adapt as requirements change. These skills take years to develop.

**"You can learn programming in a month."** You can learn the basics of a language in a month. Becoming a proficient programmer who can build reliable, maintainable software takes years of deliberate practice.

## Glossary

| Term | Definition |
|------|------------|
| Program | A sequence of instructions that tells a computer what to do |
| Source code | Human-readable text written in a programming language |
| Machine code | Binary instructions that a CPU executes directly |
| Compiler | A program that translates source code into machine code before execution |
| Interpreter | A program that reads and executes source code line by line |
| JIT compiler | A compiler that compiles code at runtime for better performance |
| Bytecode | An intermediate representation of code, between source and machine code |
| CPU | Central Processing Unit — the hardware that executes instructions |
| RAM | Random Access Memory — temporary, fast storage for running programs |
| Algorithm | A step-by-step procedure for solving a problem |
| Syntax | The grammar rules of a programming language |
| Semantics | The meaning of code — what it does when executed |
| Paradigm | A style or philosophy of programming (imperative, OOP, functional) |
| Bug | An error in a program that causes incorrect behavior |
| Debugging | The process of finding and fixing bugs |
| Refactoring | Improving code structure without changing its behavior |
| Abstraction | Hiding complex details behind a simple interface |
| Portability | The ability to run the same code on different systems |
| Input | Data that a program receives to process |
| Output | Data that a program produces as a result |
| Fetch-decode-execute | The cycle a CPU uses to process instructions |
| Pipeline | A CPU technique that processes multiple instructions simultaneously |
| Edge case | An input or scenario that is outside normal operation |
| Type system | Rules that define how data types can be used and combined |
| Variable | A named container that holds a value in memory |
| Expression | A combination of values and operators that produces a result |
| Statement | An instruction that performs an action |
| Boolean | A data type with two values: true and false |
| String | A sequence of characters representing text |

## Quick References

- [Structure and Interpretation of Computer Programs (SICP)](https://mitpress.mit.edu/sicp/) — classic text on programming fundamentals
- [The Pragmatic Programmer](https://pragprog.com/titles/tpp20/) — practical wisdom for software developers
- [Code: The Hidden Language of Computer Hardware and Software](https://www.charlespetzold.com/code/) — how computers work from the ground up
- [Learn to Program](https://pragprog.com/titles/ahlearnt2/) — beginner-friendly introduction
- [Programming Paradigms for Beginners](https://www.freecodecamp.org/news/an-introduction-to-programming-paradigms/) — overview of different programming styles

## Next Steps

- [Programming Fundamentals](../fundamentals/index.md) — start building practical programming skills
- [Variables & Data Types](../fundamentals/variables-and-data-types.md) — the building blocks of data
- [Control Flow](../fundamentals/control-flow.md) — making decisions and repeating actions
- [Functions](../fundamentals/functions.md) — reusable, composable units of code
