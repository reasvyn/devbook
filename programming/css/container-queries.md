# Container Queries

## Description

Container queries allow elements to respond to their parent container's size rather than the viewport. This enables truly reusable responsive components that adapt wherever they are placed.

## Prerequisites

- [Media Queries](media-queries.md) — viewport-based responsive design
- [CSS Grid](grid.md) — grid layout for container-based design

## Table of Contents

- [The Problem Container Queries Solve](#the-problem-container-queries-solve)
- [Defining a Container](#defining-a-container)
- [Container Query Syntax](#container-query-syntax)
- [Container Query Units](#container-query-units)
- [Container Type](#container-type)
- [Container Name](#container-name)
- [Container Shorthand](#container-shorthand)
- [Nested Containers](#nested-containers)
- [Browser Support](#browser-support)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Problem Container Queries Solve

With media queries, a component responds to the viewport size — not its container:

```css
/* Card always shows this layout when viewport < 768px */
.card {
    display: flex;
    flex-direction: column;
}

@media (min-width: 768px) {
    .card {
        flex-direction: row;
    }
}
```

This fails when the same card is placed in a narrow sidebar at 1440px viewport — the card shows a row layout even though it has no space.

Container queries fix this by letting the component respond to its parent:

```css
.card {
    display: flex;
    flex-direction: column;
}

@container (min-width: 400px) {
    .card {
        flex-direction: row;
    }
}
```

The card switches to row layout when its container is at least 400px wide, regardless of viewport size.

### Defining a Container

Elements must be declared as query containers:

```css
.sidebar {
    container-type: inline-size;     /* Enables container queries on width */
}

.main-content {
    container-type: inline-size;
}
```

Now children inside `.sidebar` and `.main-content` can use `@container` queries.

### Container Query Syntax

```css
@container (min-width: 400px) {
    .card {
        flex-direction: row;
    }
}
```

Styles apply when the nearest container with `container-type` is at least 400px wide.

Multiple conditions:

```css
@container (min-width: 400px) and (max-width: 800px) {
    .card { gap: 0.5rem; }
}

@container (orientation: landscape) {
    .card { grid-template-columns: 1fr 1fr; }
}

@container (width > 600px) {
    .card { padding: 2rem; }
}
```

Range syntax works same as media queries:

```css
@container (400px <= width <= 800px) { }
@container (width >= 600px) { }
```

### Container Query Units

Units relative to the query container:

| Unit | Relative to |
|---|---|
| `cqw` | 1% of container width |
| `cqh` | 1% of container height |
| `cqi` | 1% of container's inline size |
| `cqb` | 1% of container's block size |
| `cqmin` | Smaller of `cqi` and `cqb` |
| `cqmax` | Larger of `cqi` and `cqb` |

```css
.card {
    font-size: clamp(1rem, 3cqi, 2rem);        /* Based on container width */
    padding: 2cqi;
    margin: 1cqb;
    width: min(100%, 50cqw);                   /* Max 50% of container */
}
```

Container units make truly fluid components:

```css
.component {
    --padding: 2cqi;
    --font-size: clamp(0.875rem, 2cqi, 1.25rem);
    padding: var(--padding);
    font-size: var(--font-size);
}
```

### Container Type

```css
.container {
    container-type: normal;          /* No query containment (but named container) */
    container-type: inline-size;     /* Query on inline axis (width in horizontal writing) */
    container-type: size;            /* Query on both axes (rare — use inline-size) */
}
```

`inline-size` is the most useful — it queries the width (in horizontal writing) which is what responsive components need. `size` also tracks height but triggers layout more aggressively.

### Container Name

Name containers to disambiguate when multiple containers are nested:

```css
.sidebar {
    container-type: inline-size;
    container-name: sidebar;
}

.main {
    container-type: inline-size;
    container-name: main;
}

/* Target a specific container */
@container sidebar (min-width: 300px) {
    .card { padding: 1rem; }
}

@container main (min-width: 600px) {
    .card { padding: 2rem; }
}
```

### Container Shorthand

```css
.container {
    container: sidebar / inline-size;
    /* container-name: sidebar; container-type: inline-size; */
}

.container {
    container: inline-size;          /* Unnamed */
}
```

### Nested Containers

When containers are nested, `@container` queries the nearest container:

```css
<main class="main-container">
    <section class="section-container">
        <div class="card">...</div>
    </section>
</main>
```

```css
.main-container {
    container-type: inline-size;
}

.section-container {
    container-type: inline-size;
}

/* This queries .section-container (nearest) */
@container (min-width: 400px) {
    .card { flex-direction: row; }
}

/* To query .main-container specifically, use container-name */
.main-container {
    container-name: main;
    container-type: inline-size;
}

@container main (min-width: 800px) {
    .card { display: grid; grid-template-columns: 1fr 1fr; }
}
```

### Browser Support

Container queries are supported in all modern browsers (2024+):
- Chrome 105+
- Firefox 110+
- Safari 16+

No polyfill needed for modern development.

## Glossary

| Term | Definition |
|---|---|
| Container query | Query based on container size rather than viewport |
| container-type | Declares element as query container |
| container-name | Names a container for targeted queries |
| cqi | 1% of container's inline size |
| cqw | 1% of container width |
| cqb | 1% of container's block size |
| Inline size | Width in horizontal writing mode |

## Quick References

- [MDN: Container Queries](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_container_queries) — complete guide
- [CSS Containment Level 3](https://www.w3.org/TR/css-contain-3/) — specification
- [Caniuse: Container Queries](https://caniuse.com/css-container-queries) — browser support
- [Container Query Playground](https://container.style/) — interactive examples

## Next Steps

- [Mobile-First Design Patterns](mobile-first-patterns.md) — practical responsive patterns