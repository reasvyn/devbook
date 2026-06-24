# The Philosophy of PHP

## Description

PHP was never designed by committee — it grew organically from the needs of web developers. This bottom-up evolution gives PHP a unique philosophy: pragmatic over perfect, productive over correct, and accessible over academic. Understanding this explains both PHP's popularity and the criticism it receives.

## Prerequisites

- [What Is PHP](what-is-php.md)

## Table of Contents

- [Designed for the Web, by Web Developers](#designed-for-the-web-by-web-developers)
- [Low Barrier to Entry](#low-barrier-to-entry)
- [Progressive Evolution](#progressive-evolution)
- [Pragmatism Over Purity](#pragmatism-over-purity)

## Content / Material

### Designed for the Web, by Web Developers

PHP was created explicitly for web development — not as a general-purpose language later adapted for the web:

- HTML embeds directly in PHP (`<h1><?= $title ?></h1>`).
- Superglobals (`$_GET`, `$_POST`, `$_SESSION`) mirror HTTP directly.
- Request-response cycle is the default mental model.
- Sessions, cookies, and file uploads are built-in features.

Everything in PHP is designed around the web request lifecycle.

### Low Barrier to Entry

PHP's philosophy has always been "get it done, fast":

- No compilation step — edit, save, refresh.
- No type declarations required — strings, numbers, and arrays auto-convert.
- Forgiving error handling — warnings don't stop execution.
- Deploy by uploading files — no server configuration needed.

This made PHP the gateway language for millions of web developers.

### Progressive Evolution

PHP evolved from a simple templating language (PHP/FI) to a modern language with:

- JIT compilation (PHP 8.0) for performance-critical paths.
- Full type system — scalar types, return types, union types, generics.
- Attributes — structured metadata like annotations.
- Fibers — cooperative multitasking for async patterns.

The philosophy: add modern features without breaking the simplicity that made PHP popular.

### Pragmatism Over Purity

PHP makes design choices that language purists criticize but practitioners appreciate:

- Arrays serve as both lists and dictionaries (one data structure for everything).
- Function names are inconsistent (`strpos`, `str_replace`, `strlen`) — legacy of organic growth.
- Global functions coexist with namespaced classes.
- Loose typing "works" in most practical cases.

The philosophy: if it solves a real problem, it belongs in the language — even if it's not elegant.

## Glossary

| Term | Definition |
|------|------------|
| JIT | Just-In-Time compilation — compiles PHP bytecode to machine code at runtime |
| Superglobal | A built-in PHP variable available in all scopes (`$_GET`, `$_POST`, etc.) |
| Composer | The dependency manager for PHP |
| Packagist | The default package repository for Composer |

## Next Steps

- [Syntax & Basics](../beginner/syntax-and-basics.md) — variables, types, control flow
