# What Is Rust

## Description

Rust is a systems programming language that guarantees memory safety, thread safety, and high performance through its ownership model — all without a garbage collector. It has been voted the most loved language on Stack Overflow for years and is adopted by Microsoft, Google, and the Linux kernel community.

## Prerequisites

- [What Is Programming](../intro/what-is-programming.md)

## Table of Contents

- [The Rust Promise](#the-rust-promise)
- [Where Rust Excels](#where-rust-excels)
- [The Ownership Model](#the-ownership-model)
- [Key Concepts](#key-concepts)

## Content / Material

### The Rust Promise

- **Memory safety** — no null pointer dereferences, no dangling pointers, no buffer overflows
- **Thread safety** — data races are compile-time errors
- **Zero-cost abstractions** — high-level ergonomics with C-level performance
- **No garbage collector** — predictable, deterministic performance

### Where Rust Excels

| Domain | Examples |
|--------|----------|
| Systems programming | OS components, device drivers |
| WebAssembly | High-performance browser code |
| CLI tools | ripgrep, fd, bat, alacritty |
| Networking | Firecracker, Linkerd (proxy) |
| Embedded | Microcontroller firmware |
| Blockchain | Solana, Polkadot |

### The Ownership Model

The core innovation of Rust:

- **Each value has exactly one owner** at any time
- **Borrowing** — references without taking ownership (`&`)
- **Lifetimes** — the compiler tracks how long references are valid

```rust
fn greet(name: &str) -> String {
    format!("Hello, {}!", name)
}
```

### Key Concepts

- **No null** — `Option<T>` instead of null pointers
- **No exceptions** — `Result<T, E>` for error handling
- **Pattern matching** — exhaustive `match` expressions
- **Traits** — Rust's version of interfaces (shared behavior)

## Glossary

| Term | Definition |
|------|------------|
| Ownership | Rust's system where each value has exactly one owner |
| Borrowing | Creating a reference to a value without taking ownership |
| Lifetime | A compile-time construct ensuring references are always valid |
| Trait | A collection of methods that types can implement |
| `Option` | An enum representing either a value (`Some(T)`) or absence (`None`) |

## Next Steps

- [Ownership & Borrowing](../beginner/ownership-borrowing.md) — the foundation of Rust
- [Structs & Enums](../beginner/structs-and-enums.md) — defining custom data types
