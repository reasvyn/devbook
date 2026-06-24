# What Is Java

## Description

Java is a statically typed, object-oriented language that runs on the Java Virtual Machine (JVM). It has been a dominant force in enterprise software, Android development, and large-scale distributed systems since its release in 1995.

## Prerequisites

- [What Is Programming](../intro/what-is-programming.md)

## Table of Contents

- [The JVM Ecosystem](#the-jvm-ecosystem)
- [Where Java Excels](#where-java-excels)
- [Key Concepts](#key-concepts)
- [Modern Java](#modern-java)

## Content / Material

### The JVM Ecosystem

Java's superpower is the JVM — a runtime that provides:

- **Platform independence** — write once, run anywhere
- **Garbage collection** — automatic memory management
- **Mature tooling** — profiling, monitoring, debugging
- **Rich ecosystem** — Spring, Hibernate, Maven, Gradle

The JVM also hosts Kotlin, Scala, Groovy, and Clojure.

### Where Java Excels

| Domain | Frameworks / Tools |
|--------|-------------------|
| Enterprise backend | Spring Boot, Jakarta EE |
| Android | Native Android SDK |
| Big data | Apache Hadoop, Spark, Flink |
| Financial systems | High-frequency trading, banking |
| Cloud-native | Quarkus, Micronaut, Spring Cloud |

### Key Concepts

- **Object-oriented** — everything (except primitives) is an object
- **Static typing** — types checked at compile time
- **Verbose** — explicit over concise
- **Thread-based** — traditional multi-threading with `synchronized`, `volatile`, `Lock`

```java
public class Greeting {
    public static String greet(String name) {
        return "Hello, " + name + "!";
    }
}
```

### Modern Java

Java has evolved rapidly since version 8 (2014):

- **Lambdas & streams** — functional-style operations on collections
- **Records** — concise data carriers (Java 14+)
- **Pattern matching** — cleaner type-based logic
- **Virtual threads** — lightweight concurrency (Java 21+)

## Glossary

| Term | Definition |
|------|------------|
| JVM | Java Virtual Machine — the runtime that executes Java bytecode |
| Garbage collection | Automatic memory reclamation of unused objects |
| Spring Boot | A popular Java framework for building production-grade applications |
| Record | A concise way to define immutable data classes (Java 14+) |

## Next Steps

- [Object-Oriented Basics](../beginner/oop-basics.md) — classes, inheritance, polymorphism
- [Collections Framework](../beginner/collections.md) — List, Set, Map, and Stream API
