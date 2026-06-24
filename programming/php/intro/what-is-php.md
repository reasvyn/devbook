# What Is PHP

## Description

PHP is a server-side scripting language designed for web development. It powers over 75% of websites (including WordPress, Facebook's original backend) and has evolved significantly with modern frameworks, JIT compilation, and strong typing since PHP 7+.

## Prerequisites

- [What Is Programming](../intro/what-is-programming.md)

## Table of Contents

- [Why PHP Endures](#why-php-endures)
- [Where PHP Excels](#where-php-excels)
- [Key Concepts](#key-concepts)
- [Modern PHP](#modern-php)

## Content / Material

### Why PHP Endures

- **Purpose-built for the web** — HTML embedding, superglobals, session handling
- **Zero friction deployment** — upload files, they run
- **Massive ecosystem** — Composer, Packagist, WordPress plugins
- **Incredible hosting availability** — every host supports PHP

### Where PHP Excels

| Domain | Frameworks / Tools |
|--------|-------------------|
| Web applications | Laravel, Symfony, CakePHP |
| CMS | WordPress, Drupal, Joomla |
| E-commerce | Magento, WooCommerce |
| APIs | Laravel Sanctum, API Platform |

### Key Concepts

- **Request-response cycle** — PHP runs per-request, everything is fresh
- **Superglobals** — `$_GET`, `$_POST`, `$_SESSION`, `$_SERVER`
- **SQL injection prevention** — PDO prepared statements are essential
- **Composer** — dependency manager (equivalent to npm or pip)

```php
<?php

function greet(string $name): string
{
    return "Hello, {$name}!";
}
```

### Modern PHP

Since PHP 7.0, the language has transformed:

- **Type declarations** — scalar types, return types, union types
- **JIT compiler** (PHP 8.0) — significant performance gains
- **Named arguments** — skip optional parameters
- **Attributes** — structured metadata (like annotations in Java)
- **Match expression** — safer alternative to switch

## Glossary

| Term | Definition |
|------|------------|
| Composer | The dependency manager for PHP |
| PDO | PHP Data Objects — a database access abstraction layer |
| JIT | Just-In-Time compiler — compiles PHP to machine code at runtime |
| Laravel | The most popular PHP web framework |

## Next Steps

- [Syntax & Basics](../beginner/syntax-and-basics.md) — variables, types, operators, control flow
- [Laravel Essentials](../beginner/laravel-essentials.md) — routing, Eloquent, Blade
