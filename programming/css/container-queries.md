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
- [Container Queries vs Media Queries](#container-queries-vs-media-queries)
- [Real-World Patterns](#real-world-patterns)
- [Style Queries](#style-queries)
- [Browser Support](#browser-support)
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

### Container Queries vs Media Queries

Container queries and media queries serve different purposes and work best together.

| Aspect | Media Queries | Container Queries |
|---|---|---|
| Reference point | Viewport / screen size | Nearest sized container |
| Reusability | Component fixed to viewport breakpoints | Component adapts wherever placed |
| Scope | Global document styles | Scoped to container descendants |
| Use case | Page-level layout, device breakpoints | Component-level responsiveness |
| Example | `@media (width < 768px)` | `@container (width < 400px)` |
| Available units | `vw`, `vh`, `vmin`, `vmax` | `cqw`, `cqh`, `cqi`, `cqb`, `cqmin`, `cqmax` |

Use media queries for page-level layout (sidebar collapses, header shrinks) and container queries for component-level responsiveness (a card that looks good in any column).

```css
/* Page layout: use media queries */
@media (max-width: 768px) {
    .layout {
        grid-template-columns: 1fr;
    }
}

/* Component adaptation: use container queries */
@container (max-width: 300px) {
    .card {
        flex-direction: column;
        text-align: center;
    }
}

@container (min-width: 301px) and (max-width: 600px) {
    .card {
        flex-direction: row;
        gap: 1rem;
    }
}

@container (min-width: 601px) {
    .card {
        display: grid;
        grid-template-columns: 200px 1fr;
        gap: 2rem;
    }
}
```

Using both together creates responsive pages with independently adaptive components.

### Real-World Patterns

**Pattern 1: Adaptive Product Card**

A product card used in a narrow sidebar, medium content area, and full-width hero section:

```css
.product-card {
    container-type: inline-size;
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
}

@container (min-width: 350px) {
    .product-card {
        flex-direction: row;
    }
    .product-card img {
        width: 120px;
        height: 120px;
    }
}

@container (min-width: 600px) {
    .product-card {
        display: grid;
        grid-template-columns: 200px 1fr auto;
        align-items: center;
    }
    .product-card img {
        width: 180px;
        height: 180px;
    }
    .product-card .actions {
        display: flex;
        gap: 0.5rem;
    }
}
```

**Pattern 2: Dashboard Widget Grid**

Widgets that reorganize their internal layout based on available space:

```css
.widget {
    container-type: inline-size;
    container-name: widget;
    padding: 1rem;
}

@container widget (max-width: 200px) {
    .widget-header {
        flex-direction: column;
        text-align: center;
    }
    .widget-value {
        font-size: 1.25rem;
    }
    .widget-chart {
        display: none;
    }
}

@container widget (min-width: 201px) and (max-width: 500px) {
    .widget-header {
        flex-direction: row;
        justify-content: space-between;
    }
    .widget-value {
        font-size: 1.5rem;
    }
    .widget-chart {
        height: 100px;
    }
}

@container widget (min-width: 501px) {
    .widget-header {
        flex-direction: row;
    }
    .widget-value {
        font-size: 2rem;
    }
    .widget-chart {
        height: 200px;
    }
}
```

**Pattern 3: Article Body with Side Metadata**

An article component used both in a reading view and a card list:

```css
.article {
    container-type: inline-size;
}

@container (max-width: 400px) {
    .article .meta {
        display: flex;
        flex-wrap: wrap;
        gap: 0.25rem;
        font-size: 0.75rem;
    }
    .article .meta time::before {
        content: "📅 ";
    }
    .article .excerpt {
        display: -webkit-box;
        -webkit-line-clamp: 3;
        -webkit-box-orient: vertical;
        overflow: hidden;
    }
}

@container (min-width: 401px) {
    .article {
        display: grid;
        grid-template-columns: 1fr auto;
    }
    .article .meta {
        flex-direction: column;
        text-align: right;
        font-size: 0.875rem;
    }
    .article .excerpt {
        display: block;
    }
}
```

**Pattern 4: Fluid Typography Scale**

Using container units to create a type scale that adjusts with the component:

```css
.typography-system {
    container-type: inline-size;
}

.typography-system h1 {
    font-size: clamp(1.5rem, 5cqi, 3rem);
}

.typography-system h2 {
    font-size: clamp(1.25rem, 3.5cqi, 2rem);
}

.typography-system p {
    font-size: clamp(0.875rem, 2cqi, 1.125rem);
    line-height: 1.6;
}

.typography-system .small {
    font-size: clamp(0.75rem, 1.5cqi, 0.875rem);
}
```

### Style Queries

Style queries (CSS Containment Level 3) allow querying on computed style values, not just size:

```css
@container style(--theme: dark) {
    .card {
        background: #1a1a2e;
        color: #e0e0e0;
    }
}

@container style(--layout: compact) {
    .card {
        padding: 0.5rem;
        gap: 0.25rem;
    }
}
```

Style queries use custom properties as flags. They are useful for theme-aware components that adapt based on a parent's custom property:

```css
.parent-dark {
    --theme: dark;
    container-name: theme;
}

.parent-light {
    --theme: light;
    container-name: theme;
}

@container theme style(--theme: dark) {
    .component { background: #333; color: #fff; }
}

@container theme style(--theme: light) {
    .component { background: #fff; color: #333; }
}
```

Browser support for style queries is limited as of 2025 (Chrome 111+, others behind). Size queries remain the primary use case.

### Browser Support

| Browser | Supported Since | Notes |
|---|---|---|
| Chrome | 105+ (Aug 2022) | Full support |
| Edge | 105+ (Aug 2022) | Chromium-based |
| Firefox | 110+ (Feb 2023) | Full support |
| Safari | 16.0+ (Sep 2022) | Full support |
| Opera | 91+ (Aug 2022) | Chromium-based |
| Samsung Internet | 20+ (Oct 2022) | Chromium-based |

Known limitations and quirks:
- `container-type: size` (both axes) is rarely needed and triggers more aggressive layout — prefer `inline-size`
- Style queries (`@container style(...)`) have limited browser support — check Caniuse before using in production
- Container queries do not work in pseudo-elements (`::before`, `::after`)
- Elements with `display: contents` cannot be query containers
- Container queries cannot query their own size — the container is excluded from the query scope

## Learning Tips

- **Start with `inline-size`.** Only use `size` if you genuinely need height-based queries. `size` triggers layout containment on both axes, which can cause performance issues.
- **Name your containers.** When building complex layouts with nested containers, explicit `container-name` prevents confusion about which container is being queried.
- **Test in multiple contexts.** A container-queried component should be tested in narrow, medium, and wide containers. Use browser DevTools to resize containers.
- **Combine with clamp().** Container units (`cqi`, `cqw`) shine when paired with `clamp()` for fluid typography that adapts to the component's width.
- **Use progressive enhancement.** Write base styles for the smallest container first, then add `@container` rules to enhance at larger sizes.
- **Check containment in DevTools.** Chrome DevTools highlights elements that are query containers with a `container` badge in the Elements panel. Click the badge to inspect container dimensions.
- **Do not over-nest.** Deeply nested containers with many `@container` rules can impact rendering performance. Limit to 2-3 levels of containment.

## Glossary

| Term | Definition |
|---|---|
| Container query | CSS feature that applies styles based on a parent container's size or style |
| container-type | Property that declares an element as a query container (`normal`, `inline-size`, `size`) |
| container-name | Property that names a container for targeted `@container` queries |
| container | Shorthand property combining `container-name` and `container-type` |
| @container | At-rule that defines conditions for container-based style application |
| cqi | 1% of container's inline size (width in horizontal writing mode) |
| cqw | 1% of container width |
| cqh | 1% of container height |
| cqb | 1% of container's block size (height in horizontal writing mode) |
| cqmin | The smaller of `cqi` and `cqb` |
| cqmax | The larger of `cqi` and `cqb` |
| Inline size | Element's size in the inline axis (width for horizontal writing) |
| Block size | Element's size in the block axis (height for horizontal writing) |
| Query container | An element with `container-type` that can be queried by descendants |
| Containment | CSS concept that isolates an element's layout, style, or size from the rest of the document |
| Style query | `@container style(...)` that conditions on computed custom property values |
| Size query | `@container` condition based on container dimensions |
| Container containment | The `contain` value applied implicitly by `container-type` to enable querying |
| Nested container | A query container that is a descendant of another query container |

## Quick References

- [MDN: Container Queries](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_container_queries) — complete guide
- [CSS Containment Level 3](https://www.w3.org/TR/css-contain-3/) — specification
- [Caniuse: Container Queries](https://caniuse.com/css-container-queries) — browser support
- [Container Query Playground](https://container.style/) — interactive examples
- [A Complete Guide to CSS Container Queries](https://css-tricks.com/a-complete-guide-to-css-container-queries/) — CSS-Tricks comprehensive tutorial
- [Container Queries: A Practical Guide](https://www.smashingmagazine.com/2023/01/complete-guide-css-container-queries/) — Smashing Magazine in-depth patterns
- [MDN: CSS Container Queries](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_container_queries) — MDN reference with examples
- [Style Queries Explainer](https://developer.chrome.com/docs/css-ui/style-queries) — Chrome Developer Blog
- [Container Query Bits](https://container-query-bits.netlify.app/) — Collection of real-world container query patterns
- [web.dev: Container Queries](https://web.dev/articles/container-queries) — Google web fundamentals guide

## Next Steps

- [Mobile-First Design Patterns](mobile-first-patterns.md) — practical responsive patterns