# The Philosophy of Go

## Description

Go was designed by three engineers at Google (Ken Thompson, Rob Pike, Robert Griesemer) who were tired of slow builds, complex type systems, and concurrency bugs. Go's philosophy is radical simplicity: few features, opinionated tooling, and a focus on readability over cleverness.

## Prerequisites

- [What Is Go](what-is-go.md)

## Table of Contents

- [Simplicity by Design](#simplicity-by-design)
- [Opinionated Tooling](#opinionated-tooling)
- [Concurrency as a First-Class Citizen](#concurrency-as-a-first-class-citizen)
- [Readable Over Clever](#readable-over-clever)

## Content / Material

### Simplicity by Design

Go has fewer keywords than almost any mainstream language (25). It deliberately omits:

- No classes — use structs and interfaces.
- No inheritance — use composition.
- No generics (until Go 1.18) — use interfaces and code generation.
- No exceptions — use explicit error returns.
- No method overloading — each name has one meaning.
- No implicit conversions — type safety is explicit.

The philosophy: a simpler language produces simpler, more maintainable code.

### Opinionated Tooling

Go ships with everything you need — and enforces how you use it:

- `go fmt` — one true formatting, no debates.
- `go mod` — built-in dependency management, no npm/pip/maven.
- `go test` — testing built into the toolchain.
- `go vet` — static analysis as part of the workflow.
- `go build` — compile to a single static binary.

No configuration files. No build systems. No debates about style. This eliminates entire categories of team friction.

### Concurrency as a First-Class Citizen

Concurrency is built into the language, not bolted on:

- **Goroutines** — lightweight threads (cheap, thousands per process).
- **Channels** — typed communication between goroutines.
- **Select** — wait on multiple channel operations.
- **Sync primitives** — WaitGroup, Mutex, RWMutex, Once.

```go
func main() {
    ch := make(chan string)
    go func() {
        ch <- "Hello from goroutine!"
    }()
    fmt.Println(<-ch)
}
```

The philosophy: "Don't communicate by sharing memory; share memory by communicating."

### Readable Over Clever

Go code tends to look similar across projects:

- No operator overloading — `a + b` always means addition.
- No metaprogramming — `go generate` is the closest thing.
- Explicit error handling — every error path is visible.
- Short variable names — `i`, `r`, `w` are conventional for loops, readers, writers.

Go prioritizes code that is easy for the next person to understand over code that is clever or concise.

## Glossary

| Term | Definition |
|------|------------|
| Goroutine | A lightweight thread managed by the Go runtime |
| Channel | A typed conduit for communication between goroutines |
| `go fmt` | The standard, enforced code formatter for Go |
| Interface | A set of method signatures — implemented implicitly |

## Next Steps

- [Variables & Types](../beginner/variables-and-types.md) — var, :=, structs, interfaces
