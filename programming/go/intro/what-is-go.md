# What Is Go

## Description

Go (or Golang) is a statically typed, compiled language created at Google in 2009. It was designed for building simple, reliable, and efficient software at scale — with first-class concurrency, fast compilation, and a minimal feature set.

## Prerequisites

- [What Is Programming](../intro/what-is-programming.md)

## Table of Contents

- [The Go Philosophy](#the-go-philosophy)
- [Where Go Excels](#where-go-excels)
- [Key Concepts](#key-concepts)
- [Why Developers Choose Go](#why-developers-choose-go)

## Content / Material

### The Go Philosophy

- **Simplicity first** — the language has very few features
- **Opinionated** — one way to format (`gofmt`), one build system, one package manager
- **Fast compilation** — compiles to a single binary in seconds
- **Explicit error handling** — errors are values, not exceptions

### Where Go Excels

| Domain | Examples |
|--------|----------|
| Cloud / backend | Docker, Kubernetes, Terraform |
| CLI tools | Hugo, Cobra-based CLIs |
| API services | Gin, Echo, Fiber, net/http |
| DevOps tooling | Prometheus, Grafana (backend) |
| Networking | etcd, Consul, Traefik |

### Key Concepts

- **Goroutines** — lightweight threads managed by the runtime
- **Channels** — communicate between goroutines
- **Interfaces** — implicit implementation (structural typing)
- **No generics** (well, now there are — added in Go 1.18)

```go
package main

import "fmt"

func greet(name string) string {
    return fmt.Sprintf("Hello, %s!", name)
}
```

### Why Developers Choose Go

- **Single binary output** — easy deployment, no runtime dependencies
- **Built-in concurrency** — goroutines + channels are part of the language
- **Static typing** — catch errors at compile time
- **Excellent standard library** — HTTP server, JSON, templating built in

## Glossary

| Term | Definition |
|------|------------|
| Goroutine | A lightweight thread managed by the Go runtime |
| Channel | A typed conduit for sending and receiving values between goroutines |
| `go fmt` | The standard tool for formatting Go source code |
| Interface | A set of method signatures — implemented implicitly in Go |

## Next Steps

- [Variables & Types](../beginner/variables-and-types.md) — var, :=, basic types, structs
- [Concurrency Basics](../beginner/concurrency-basics.md) — goroutines, channels, select
