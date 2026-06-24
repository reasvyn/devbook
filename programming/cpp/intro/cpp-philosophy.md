# The Philosophy of C++

## Description

C++ is a language of trade-offs. It gives you power — direct memory control, zero-cost abstractions, compile-time computation — in exchange for complexity. Its philosophy is that you should never pay for what you don't use, and you should be able to do anything the hardware can do.

## Prerequisites

- [What Is C++](what-is-cpp.md)

## Table of Contents

- [You Don't Pay for What You Don't Use](#you-dont-pay-for-what-you-dont-use)
- [Multiple Paradigms, One Language](#multiple-paradigms-one-language)
- [Trust the Programmer](#trust-the-programmer)
- [Compile-Time Computation](#compile-time-computation)

## Content / Material

### You Don't Pay for What You Don't Use

C++'s core principle: if you don't use a feature, you shouldn't pay for it — in performance, memory, or startup time.

- Virtual functions incur a vtable lookup — but only if you use `virtual`.
- Exceptions add runtime overhead — but you can disable them.
- RTTI (Runtime Type Information) costs memory — but you opt in.

This is why C++ is used in game engines, browsers, and trading systems: you get high-level abstractions with C-level performance.

### Multiple Paradigms, One Language

C++ supports multiple paradigms without forcing any:

- **Procedural** — write C-style code with functions and structs.
- **Object-oriented** — classes, inheritance, virtual functions.
- **Generic** — templates for type-parameterized code.
- **Functional** — lambdas, `std::function`, `std::transform`.
- **Compile-time** — `constexpr`, `consteval`, template metaprogramming.

The philosophy: let the developer choose the right tool for the problem, not the language.

### Trust the Programmer

C++ trusts you to know what you're doing:

- Raw pointers and manual memory management are available.
- Undefined behavior is the programmer's responsibility.
- No garbage collector — you control object lifetimes.
- No mandatory bounds checking — you control the safety-performance trade-off.

This trust enables maximum performance but requires deep understanding.

### Compile-Time Computation

C++ pushes as much work as possible to compile time:

- `constexpr` — evaluate functions at compile time.
- Templates — generate specialized code for each type.
- `consteval` (C++20) — functions that *must* run at compile time.
- Template metaprogramming — Turing-complete computation during compilation.

The philosophy: if it can be computed at compile time, it should be.

## Glossary

| Term | Definition |
|------|------------|
| Zero-cost abstraction | A language feature that adds no runtime overhead if unused |
| RAII | Resource Acquisition Is Initialization — tying resource lifetime to object scope |
| Undefined behavior | Program behavior that the language standard does not define |
| Template | A compile-time mechanism for type-parameterized code |

## Next Steps

- [Memory Management](../beginner/memory-management.md) — stack vs heap, pointers, smart pointers
