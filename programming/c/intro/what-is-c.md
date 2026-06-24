# What Is C

## Description

C is a minimal, compiled systems programming language developed at Bell Labs in 1972. It provides direct memory access, minimal runtime overhead, and maximum control. C is the foundation upon which most modern operating systems, compilers, and language runtimes are built.

## Prerequisites

- [What Is Programming](../intro/what-is-programming.md)

## Table of Contents

- [Why C Matters](#why-c-matters)
- [Where C Excels](#where-c-excels)
- [Key Concepts](#key-concepts)
- [C and the Modern Developer](#c-and-the-modern-developer)

## Content / Material

### Why C Matters

- **The mother of all languages** — C syntax influenced Java, C#, C++, JavaScript, Go, Rust
- **Portable assembler** — write once, compile anywhere
- **Minimal runtime** — no garbage collector, no exceptions, no overhead
- **Understanding C == understanding the computer**

### Where C Excels

| Domain | Examples |
|--------|----------|
| Operating systems | Linux, Windows kernel |
| Embedded systems | Microcontrollers, firmware, IoT |
| Compilers & interpreters | LLVM, CPython, Ruby MRI |
| Databases | SQLite core |
| Networking | nginx, Redis core |

### Key Concepts

- **Pointers** — variables that hold memory addresses
- **Manual memory management** — `malloc` and `free`
- **Structured but not OOP** — structs without methods
- **Preprocessor** — `#include`, `#define`, macros

```c
#include <stdio.h>
#include <string.h>

void greet(const char* name) {
    printf("Hello, %s!\n", name);
}
```

### C and the Modern Developer

Even if you never write C professionally, learning C teaches you:

- How memory works (stack vs heap, pointers, arrays)
- What the OS does for you (syscalls, process model)
- Why high-level languages cost performance
- How to debug at the lowest level

## Glossary

| Term | Definition |
|------|------------|
| Pointer | A variable that stores a memory address |
| `malloc` | A standard library function for dynamic memory allocation |
| Preprocessor | A text-processing step that runs before compilation |
| Syscall | A request from user-space to the operating system kernel |

## Next Steps

- [Memory & Pointers](../beginner/memory-and-pointers.md) — the core of C programming
- [Structures & File I/O](../beginner/structures-and-files.md) — organizing data and persistence
