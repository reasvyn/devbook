# Sass Basics

## Description

Sass (Syntactically Awesome Style Sheets) is a CSS preprocessor that adds variables, nesting, mixins, and modular file organisation to CSS. While modern CSS now supports many features Sass pioneered, legacy projects and certain workflows still rely on it.

## Prerequisites

- [Selectors](selectors.md) — CSS selector syntax
- [Cascade, Specificity & Inheritance](cascade-and-specificity.md) — how CSS resolves conflicts
- [Custom Properties](custom-properties.md) — CSS-native variable syntax
- [BEM & CSS Architecture](bem-and-architecture.md) — CSS organisation at scale

## Table of Contents

- [What Is Sass?](#what-is-sass)
- [SCSS vs. Sass Syntax](#scss-vs-sass-syntax)
- [Installation & Setup](#installation--setup)
- [Variables](#variables)
- [Nesting](#nesting)
- [Partials & Modules](#partials--modules)
- [Mixins](#mixins)
- [Functions](#functions)
- [Extend & Inheritance](#extend--inheritance)
- [Control Directives](#control-directives)
- [Operators](#operators)
- [Colour Functions](#colour-functions)
- [Project Structure](#project-structure)
- [Integration with Build Tools](#integration-with-build-tools)
- [Sass vs. Modern CSS](#sass-vs-modern-css)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What Is Sass?

Sass is a preprocessor that extends CSS with features not natively available at the time of its creation. It compiles down to plain CSS that browsers can interpret. Originally released in 2006, it became the de facto standard for writing maintainable CSS in large projects.

Sass does not change how CSS works. It provides syntactic sugar that compiles into standard CSS. Every valid CSS file is also valid SCSS.

The two syntaxes are:

- **SCSS (`.scss`)** — uses curly braces and semicolons, identical to CSS syntax. This is the more popular syntax and the one used throughout this document.
- **Sass (`.sass`)** — uses indentation, no braces or semicolons. The original syntax.

### SCSS vs. Sass Syntax

SCSS syntax:

```scss
.card {
    padding: 1rem;
    background: #fff;

    .title {
        font-size: 1.25rem;
    }
}
```

Sass (indented) syntax:

```sass
.card
    padding: 1rem
    background: #fff

    .title
        font-size: 1.25rem
```

Both compile to identical CSS. SCSS is recommended for new projects because it is a superset of CSS and has a shallower learning curve.

### Installation & Setup

Sass is installed via npm:

```bash
npm install -D sass
```

Compile a single file:

```bash
npx sass src/styles.scss dist/styles.css
```

Watch for changes:

```bash
npx sass --watch src/styles.scss dist/styles.css
```

Compile an entire directory:

```bash
npx sass src/scss/:dist/css/
```

With npm scripts in `package.json`:

```json
{
    "scripts": {
        "sass": "sass src/scss:dist/css",
        "sass:watch": "sass --watch src/scss:dist/css"
    }
}
```

Many build tools integrate Sass natively. Webpack uses `sass-loader`, Vite supports SCSS files out of the box, and Laravel Mix includes Sass compilation by default.

### Variables

Sass variables are defined with `$` and are compiled away — they do not exist in the output CSS. This contrasts with CSS custom properties which are dynamic and exist in the browser.

```scss
$primary: #0066cc;
$secondary: #6c757d;
$font-stack: "Inter", system-ui, sans-serif;
$spacing-unit: 0.25rem;
$border-radius: 6px;

.button {
    background: $primary;
    color: white;
    font-family: $font-stack;
    padding: $spacing-unit * 2 $spacing-unit * 4;
    border-radius: $border-radius;
}
```

Compiles to:

```css
.button {
    background: #0066cc;
    color: white;
    font-family: "Inter", system-ui, sans-serif;
    padding: 0.5rem 1rem;
    border-radius: 6px;
}
```

**Variable scoping:** Variables defined outside any block are global. Variables defined inside a block are local to that block and any nested blocks.

```scss
$global: #333; // global

.card {
    $local: #666; // local to .card
    color: $global;
    border-color: $local;
}

.footer {
    color: $global;
    // border-color: $local; // error — $local not defined here
}
```

**Default values** with `!default` allow overriding before the variable is used:

```scss
// In _variables.scss
$primary: #0066cc !default;

// Override before import
$primary: #004499;
@use "variables";
```

Variables can hold any CSS value: colours, strings, numbers, lists, maps, and booleans.

```scss
$breakpoints: (
    sm: 640px,
    md: 768px,
    lg: 1024px,
    xl: 1280px
);

$theme-colors: (
    primary: #0066cc,
    success: #28a745,
    danger: #dc3545
);
```

### Nesting

Nesting mirrors the HTML structure inside CSS, reducing repetition and making relationships explicit.

```scss
.card {
    padding: 1rem;
    border: 1px solid #ddd;
    border-radius: 6px;

    .title {
        font-size: 1.25rem;
        font-weight: 600;
        margin-bottom: 0.5rem;
    }

    .content {
        font-size: 0.875rem;
        color: #666;
    }

    .footer {
        margin-top: 1rem;
        padding-top: 0.5rem;
        border-top: 1px solid #eee;
    }
}
```

Compiles to:

```css
.card { padding: 1rem; border: 1px solid #ddd; border-radius: 6px; }
.card .title { font-size: 1.25rem; font-weight: 600; margin-bottom: 0.5rem; }
.card .content { font-size: 0.875rem; color: #666; }
.card .footer { margin-top: 1rem; padding-top: 0.5rem; border-top: 1px solid #eee; }
```

**The `&` parent selector** references the current parent selector.

```scss
.button {
    background: #0066cc;

    &:hover {
        background: #004499;
    }

    &--primary {
        background: #0055aa;
    }

    &__icon {
        margin-right: 0.5rem;
    }

    .dark-theme & {
        background: #3399ff;
    }
}
```

Compiles to:

```css
.button { background: #0066cc; }
.button:hover { background: #004499; }
.button--primary { background: #0055aa; }
.button__icon { margin-right: 0.5rem; }
.dark-theme .button { background: #3399ff; }
```

**Property nesting** — with the `:` prefix, you can nest properties:

```scss
.text {
    font: {
        family: "Inter", sans-serif;
        size: 1rem;
        weight: 400;
    }
    margin: {
        top: 1rem;
        bottom: 0.5rem;
    }
}
```

Compiles to:

```css
.text { font-family: "Inter", sans-serif; font-size: 1rem; font-weight: 400; margin-top: 1rem; margin-bottom: 0.5rem; }
```

**@at-root** lifts styles out of the nesting context:

```scss
.card {
    @at-root .wrapper {
        padding: 2rem;
    }
}
```

Compiles to:

```css
.wrapper { padding: 2rem; }
```

### Partials & Modules

Sass splits stylesheets into partial files that are imported into a main file. Partials are named with a leading underscore (`_variables.scss`) to indicate they should not be compiled independently.

**Modern approach — `@use`:**

```scss
// _variables.scss
$primary: #0066cc;
$secondary: #6c757d;

// _buttons.scss
@use "variables";

.button {
    background: variables.$primary;
}

// main.scss
@use "variables";
@use "buttons";
```

`@use` loads a module and creates a namespace by default. You can rename with `as`:

```scss
@use "variables" as v;

.button {
    background: v.$primary;
}
```

Or load without namespace:

```scss
@use "variables" as *;

.button {
    background: $primary;
}
```

**Legacy approach — `@import`:**

```scss
@import "variables";
@import "buttons";
```

`@import` is deprecated in modern Sass and emits warnings. It still works but `@use` is preferred because it provides encapsulation, avoids naming conflicts, and enables clearer dependency tracking.

**Built-in modules** are loaded with `@use`:

```scss
@use "sass:color";
@use "sass:math";
@use "sass:map";
@use "sass:string";
@use "sass:list";
@use "sass:selector";
@use "sass:meta";
```

**Forwarding modules** with `@forward` re-exports members from another module:

```scss
// _index.scss
@forward "variables";
@forward "buttons";
@forward "typography";

// main.scss
@use "index";

.button {
    background: index.$primary;
}
```

### Mixins

Mixins define reusable blocks of styles that accept parameters. They are one of Sass's most powerful features.

**Defining mixins:**

```scss
@mixin respond-above($bp) {
    @media (min-width: $bp) {
        @content;
    }
}

@mixin button-variant($bg, $color: white) {
    background: $bg;
    color: $color;
    border: 1px solid darken($bg, 10%);

    &:hover {
        background: darken($bg, 10%);
    }
}
```

**Using mixins:**

```scss
.card {
    padding: 1rem;

    @include respond-above(768px) {
        padding: 2rem;
    }
}

.btn-primary {
    @include button-variant(#0066cc);
}

.btn-success {
    @include button-variant(#28a745);
}
```

Compiles to:

```css
.card { padding: 1rem; }
@media (min-width: 768px) { .card { padding: 2rem; } }
.btn-primary { background: #0066cc; color: white; border: 1px solid #004499; }
.btn-primary:hover { background: #004499; }
.btn-success { background: #28a745; color: white; border: 1px solid #1e7e34; }
.btn-success:hover { background: #1e7e34; }
```

**The `@content` directive** passes content blocks into mixins:

```scss
@mixin mobile-first($bp) {
    @media (min-width: $bp) {
        @content;
    }
}

.sidebar {
    width: 100%;

    @include mobile-first(768px) {
        width: 300px;
        position: sticky;
        top: 1rem;
    }
}
```

**Argument defaults and keywords:**

```scss
@mixin size($width, $height: $width) {
    width: $width;
    height: $height;
}

.avatar {
    @include size(48px);
}

.hero {
    @include size($height: 400px, $width: 100%);
}
```

### Functions

Sass functions are similar to mixins but return a single value instead of emitting styles:

```scss
@function rem($px, $base: 16px) {
    @return math.div($px, $base) * 1rem;
}

@function spacing($multiplier: 1) {
    @return $spacing-unit * $multiplier;
}
```

Usage:

```scss
.card {
    padding: rem(16px);
    margin-bottom: spacing(4);
}
```

**Built-in functions** cover common operations:

```scss
@use "sass:math";

// Math
math.div(10, 3);        // 3.333...
math.ceil(3.14);        // 4
math.floor(3.99);       // 3
math.round(3.5);        // 4
math.min(1, 2, 3);      // 1
math.max(1, 2, 3);      // 3

@use "sass:string";

// String
string.unquote("hello"); // hello (unquoted)
string.quote(hello);     // "hello"
string.length("hello");  // 5
string.slice("hello", 1, 3); // "hel"
```

### Extend & Inheritance

`@extend` shares styles between selectors by grouping them in the compiled output:

```scss
%button-base {
    display: inline-flex;
    align-items: center;
    padding: 0.5rem 1rem;
    border-radius: 6px;
    font-family: inherit;
    font-size: 0.875rem;
    cursor: pointer;
}

.btn-primary {
    @extend %button-base;
    background: #0066cc;
    color: white;
}

.btn-secondary {
    @extend %button-base;
    background: #6c757d;
    color: white;
}
```

Compiles to:

```css
.btn-primary, .btn-secondary {
    display: inline-flex;
    align-items: center;
    padding: 0.5rem 1rem;
    border-radius: 6px;
    font-family: inherit;
    font-size: 0.875rem;
    cursor: pointer;
}

.btn-primary { background: #0066cc; color: white; }
.btn-secondary { background: #6c757d; color: white; }
```

`%placeholder` selectors (defined with `%`) are silent — they only appear when extended. This avoids generating unused classes in the output.

**Use `@extend` sparingly.** It can produce unexpected selector groupings and bloat when overused. Mixins or utility classes are often clearer alternatives.

### Control Directives

Sass supports basic control flow with `@each` for generating repetitive rules:

```scss
$breakpoints: (sm: 640px, md: 768px, lg: 1024px);

@each $name, $width in $breakpoints {
    .container-#{$name} {
        max-width: $width;
    }
}
```

`@if` / `@else` conditionals work inside mixins:

```scss
@mixin button-variant($type) {
    @if $type == primary { background: #0066cc; }
    @else if $type == success { background: #28a745; }
    @else { background: #6c757d; }
}
```

### Operators

Sass supports arithmetic on values with compatible units:

```scss
$base: 16px;

.element {
    width: $base * 2;          // 32px
    height: $base * 1.5;       // 24px
    margin: $base / 2;         // 8px
    padding: $base + 4px;      // 20px
    gap: $base - 4px;          // 12px
}
```

String concatenation:

```scss
$prefix: "btn-";

.#{$prefix}primary {
    // .btn-primary
}
```

Colour operations:

```scss
$color: #0066cc;
.lighter {
    background: $color + #222;  // lighten manually
}
```

Division is performed with `math.div()` in modern Sass; the `/` operator is deprecated for division.

### Integration with Build Tools

Most modern build tools compile Sass automatically. Vite and Parcel handle `.scss` imports with zero configuration. Webpack requires `sass-loader`. For standalone usage, npm scripts work:

```json
{
    "scripts": {
        "build:css": "sass src/scss/main.scss dist/css/main.css --style compressed",
        "watch:css": "sass --watch src/scss:dist/css"
    }
}
```

The `--style compressed` flag produces minified output for production.

### Sass vs. Modern CSS

| Feature | Sass | Modern CSS | Notes |
|---------|------|------------|-------|
| Variables | `$var` (compile-time) | `--var` (runtime) | CSS custom properties are dynamic and can be changed at runtime |
| Nesting | Full support | Native in 2024+ | CSS nesting has some limitations vs. Sass |
| Mixins | `@mixin` / `@include` | None native | Can replicate with utility classes or `@apply` (proposal) |
| Functions | `@function` | `@function` upcoming | CSS has built-in `var()`, `calc()`, etc. |
| Partials | `@use` / `@forward` | `@import` in CSS | CSS `@import` harms performance |
| Loops | `@for`, `@each`, `@while` | None native | Must generate utility classes manually or use PostCSS |
| Colour functions | Built-in | `color-mix()` in 2024+ | Sass has richer colour manipulation |
| Extend | `@extend` | None native | Grouping selectors manually |

**When to choose Sass in 2024+:**

- Maintaining or contributing to a legacy project that uses Sass
- Needing loops and control directives for generating utility classes
- Working with design tokens that require compile-time computation
- Team preference for SCSS syntax and established workflows

**When to skip Sass:**

- New greenfield projects — modern CSS covers most needs
- Projects prioritising runtime flexibility (theming, dark mode)
- Small projects or prototypes where Sass adds unnecessary complexity

## Glossary

| Term | Definition |
|------|------------|
| Compile | The process of converting Sass (.scss) into standard CSS (.css) |
| Directive | A Sass keyword prefixed with `@` that provides special behaviour (e.g., `@use`, `@mixin`, `@if`) |
| Extend | A Sass directive that shares styles between selectors by grouping them in the compiled CSS |
| Function | A Sass construct that takes arguments and returns a single computed value |
| Interpolation | Using `#{}` to inject Sass expressions into selector names or property values |
| Mixin | A reusable block of styles that can accept parameters and `@content` |
| Module | A namespace created by `@use` that encapsulates variables, mixins, and functions |
| Partial | A Sass file prefixed with `_` that is not compiled independently but imported via `@use` |
| Placeholder | A silent selector defined with `%` that only appears when extended |
| Preprocessor | A tool that extends a language's syntax and compiles down to the standard language |
| SCSS | Sassy CSS — the modern Sass syntax using braces and semicolons |
| Variable | A named value defined with `$` that holds CSS values at compile time |

## Quick References

- [Sass Documentation](https://sass-lang.com/documentation/) — official language reference
- [Sass Guide](https://sass-lang.com/guide) — getting started tutorial
- [SassMeister](https://www.sassmeister.com/) — online Sass playground
- [node-sass vs. dart-sass](https://sass-lang.com/blog/libsass-is-deprecated/) — migration guide
- [caniuse: CSS Nesting](https://caniuse.com/css-nesting) — browser support for native nesting

## Next Steps

- [BEM & CSS Architecture](bem-and-architecture.md) — organising CSS at scale
- [Custom Properties](custom-properties.md) — CSS-native variables and theming
- [CSS Tooling & Performance](tooling-and-performance.md) — build tools and optimisation
