# What Is C++

## Description

C++ is a compiled, statically typed language that gives developers fine-grained control over memory and hardware. It is the go-to choice for performance-critical systems — game engines, browsers, databases, embedded systems, and high-frequency trading.

## Prerequisites

- [What Is Programming](../intro/what-is-programming.md)

## Table of Contents

- [The C++ Philosophy](#the-cpp-philosophy)
- [Where C++ Excels](#where-cpp-excels)
- [Key Concepts](#key-concepts)
- [Modern C++](#modern-cpp)

## Content / Material

### The C++ Philosophy

- **Zero-cost abstraction** — higher-level constructs don't incur runtime overhead
- **You don't pay for what you don't use**
- **Manual memory management** — you control allocation and deallocation
- **Deterministic destruction** — destructors run at predictable times

### Where C++ Excels

| Domain | Examples |
|--------|----------|
| Game engines | Unreal Engine, Unity (backend) |
| Browsers | Chrome, Firefox core engines |
| Databases | MySQL, MongoDB, Redis (core) |
| Embedded / IoT | Firmware, real-time systems |
| Finance | High-frequency trading, risk modeling |
| Graphics | OpenGL, Vulkan, DirectX |

### Key Concepts

- **Pointers and references** — direct memory access
- **RAII** — Resource Acquisition Is Initialization (tie resource lifetime to object lifetime)
- **Templates** — compile-time polymorphism and metaprogramming
- **Move semantics** — efficient transfer of resources

```cpp
#include <string>

std::string greet(const std::string& name) {
    return "Hello, " + name + "!";
}
```

### Modern C++

Since C++11, the language has modernized significantly:

- **Smart pointers** — `unique_ptr`, `shared_ptr` (automatic memory management)
- **Lambdas** — anonymous functions
- **Concepts** (C++20) — constraints on template parameters
- **Ranges** (C++20) — composable operations on collections

## Glossary

| Term | Definition |
|------|------------|
| RAII | A C++ idiom that binds resource lifetime to object scope |
| Template | A compile-time mechanism for generic programming |
| Smart pointer | A wrapper that automatically deletes the pointed-to object |
| Move semantics | Transferring ownership of resources without copying |

## Next Steps

- [Memory Management](../beginner/memory-management.md) — stack vs heap, pointers, references
- [OOP in C++](../beginner/oop-in-cpp.md) — classes, inheritance, virtual functions
