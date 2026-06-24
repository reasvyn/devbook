# What Is Kotlin

## Description

Kotlin is a statically typed language that runs on the JVM, developed by JetBrains. It is fully interoperable with Java but eliminates much of Java's verbosity. Google officially supports Kotlin for Android development, and it is growing in backend and multiplatform use.

## Prerequisites

- [What Is Programming](../intro/what-is-programming.md)

## Table of Contents

- [Why Kotlin](#why-kotlin)
- [Where Kotlin Excels](#where-kotlin-excels)
- [Key Concepts](#key-concepts)
- [Kotlin vs. Java](#kotlin-vs-java)

## Content / Material

### Why Kotlin

- **Concise** — eliminate boilerplate (getters, setters, constructors)
- **Null safety** — null pointer exceptions are compile-time errors
- **Interoperable** — call Java code from Kotlin and vice versa
- **Modern features** — coroutines, data classes, extension functions

### Where Kotlin Excels

| Domain | Frameworks / Tools |
|--------|-------------------|
| Android | Official first-class language |
| Backend | Ktor, Spring Boot (with Kotlin support) |
| Multiplatform | Kotlin Multiplatform — share code across platforms |
| Data science | Kotlin for Data Science (Jupyter, lets-plot) |

### Key Concepts

- **Null safety** — `String?` vs `String` (nullable vs non-nullable types)
- **Data classes** — `data class` generates equals, hashCode, toString, copy automatically
- **Coroutines** — lightweight concurrency (like goroutines but on JVM)
- **Extension functions** — add methods to existing classes without inheritance

```kotlin
fun greet(name: String): String {
    return "Hello, $name!"
}
```

### Kotlin vs. Java

| Aspect | Java | Kotlin |
|--------|------|--------|
| Null safety | `Optional` or runtime checks | Built-in with `?` syntax |
| Data classes | Manual boilerplate | `data class` keyword |
| Concurrency | Threads, `CompletableFuture` | Coroutines (structured concurrency) |
| Extension methods | Not built-in | Extension functions |
| Type inference | Limited (`var` since Java 10) | Full type inference |

## Glossary

| Term | Definition |
|------|------------|
| Coroutine | A suspendable computation — lightweight concurrency primitive |
| Data class | A class automatically providing equals, hashCode, toString, and copy |
| Null safety | A type system feature that distinguishes nullable and non-nullable types |
| Extension function | A function that adds behavior to an existing class without inheritance |

## Next Steps

- [Kotlin Syntax](../beginner/kotlin-syntax.md) — variables, functions, null safety basics
- [Coroutines](../beginner/coroutines.md) — structured concurrency in Kotlin
