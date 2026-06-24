# What Is C#

## Description

C# (pronounced "C sharp") is a modern, statically typed, multi-paradigm language built on the .NET runtime. It is the primary language for Windows development, Unity game development, and cross-platform applications via .NET Core.

## Prerequisites

- [What Is Programming](../intro/what-is-programming.md)

## Table of Contents

- [The .NET Runtime](#the-net-runtime)
- [Where C# Excels](#where-csharp-excels)
- [Key Concepts](#key-concepts)
- [Modern C#](#modern-csharp)

## Content / Material

### The .NET Runtime

.NET provides:

- **Cross-platform** — Windows, macOS, Linux
- **Garbage collection** — automatic memory management
- **Unified runtime** — one runtime for web, desktop, mobile, cloud
- **Native AOT** — ahead-of-time compilation for fast startup

### Where C# Excels

| Domain | Frameworks / Tools |
|--------|-------------------|
| Web backend | ASP.NET Core |
| Game development | Unity (primary scripting language) |
| Desktop apps | WPF, WinForms, MAUI |
| Mobile | Xamarin / MAUI |
| Cloud / microservices | Azure SDK, Orleans, Dapr |

### Key Concepts

- **Managed runtime** — CLR handles memory, security, exceptions
- **Properties** — language-level getters and setters
- **LINQ** — language-integrated queries over collections
- **Async/await** — first-class asynchronous programming

```csharp
public class Greeting
{
    public static string Greet(string name)
    {
        return $"Hello, {name}!";
    }
}
```

### Modern C#

- **Nullable reference types** — eliminate null reference exceptions
- **Records** — immutable data objects
- **Top-level statements** — minimal program entry point
- **Pattern matching** — powerful switch expressions and type matching

## Glossary

| Term | Definition |
|------|------------|
| CLR | Common Language Runtime — the virtual machine for .NET |
| LINQ | Language Integrated Query — SQL-like queries directly in C# |
| ASP.NET Core | Cross-platform framework for building web APIs and applications |

## Next Steps

- [Object-Oriented Basics](../beginner/oop-basics.md) — classes, inheritance, interfaces
- [LINQ Fundamentals](../beginner/linq-basics.md) — querying collections with LINQ
