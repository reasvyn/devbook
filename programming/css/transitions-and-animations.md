# CSS Transitions & Animations

## Description

CSS brings interfaces to life with transitions, transforms, and keyframe animations. These tools allow smooth state changes and complex motion sequences without JavaScript.

## Prerequisites

- [Pseudo-Classes](pseudo-classes.md) — :hover, :focus state triggers
- [CSS Functions](css-functions.md) — cubic-bezier, transform functions

## Table of Contents

- [Transitions](#transitions)
- [Transition Properties](#transition-properties)
- [Transition Shorthand](#transition-shorthand)
- [Transition Timing Functions](#transition-timing-functions)
- [Transforms](#transforms)
- [2D Transforms](#2d-transforms)
- [3D Transforms](#3d-transforms)
- [Individual Transform Properties](#individual-transform-properties)
- [Animations and @keyframes](#animations-and-keyframes)
- [Animation Properties](#animation-properties)
- [Animation Shorthand](#animation-shorthand)
- [Animation Timing Functions](#animation-timing-functions)
- [Animation Events](#animation-events)
- [Will-Change](#will-change)
- [Performance Best Practices](#performance-best-practices)
- [Reduced Motion](#reduced-motion)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Transitions

Transitions animate property changes smoothly:

```css
.button {
    background: #0066cc;
    color: white;
    transition: background 0.3s ease;
}

.button:hover {
    background: #0052a3;
}
```

When hovered, the background fades from blue to dark blue over 0.3 seconds.

### Transition Properties

```css
.element {
    transition-property: all;            /* Which properties to animate */
    transition-duration: 0.3s;           /* How long the animation lasts */
    transition-timing-function: ease;    /* Speed curve */
    transition-delay: 0s;               /* Delay before start */
}
```

**Transition-property** — limit to what changes:

```css
.element {
    transition-property: opacity, transform;  /* Only these animate */
    transition-duration: 0.3s;
}

/* Or animate everything (less performant) */
.element {
    transition: all 0.3s;
}
```

**Transition-duration**:

```css
transition-duration: 0.2s;          /* 200ms */
transition-duration: 1s;            /* 1 second */
transition-duration: 300ms;         /* Milliseconds */
```

**Transition-delay**:

```css
transition-delay: 0s;               /* No delay (default) */
transition-delay: 0.2s;             /* Start after 200ms */
transition-delay: -0.1s;            /* Start as if partway through */
```

### Transition Shorthand

```css
.element {
    transition: property duration timing-function delay;
    transition: opacity 0.3s ease 0.1s;
}
```

Multiple transitions:

```css
.element {
    transition:
        opacity 0.3s ease,
        transform 0.2s ease-out,
        background 0.15s;
}
```

Properties with different durations for forward and reverse:

```css
/* Forward: 0.2s, reverse: 0.5s (delayed) */
.element {
    transition:
        transform 0.2s ease-out,
        background 0.15s;
}

/* Reverse: hover off triggers this */
.element {
    transition:
        transform 0.5s ease 0.2s,     /* 200ms delay before reversing */
        background 0.3s ease;
}
```

### Transition Timing Functions

```css
transition-timing-function: ease;            /* Default — slow start/end, fast middle */
transition-timing-function: linear;          /* Constant speed */
transition-timing-function: ease-in;         /* Slow start, fast end */
transition-timing-function: ease-out;        /* Fast start, slow end */
transition-timing-function: ease-in-out;     /* Slow both ends */

transition-timing-function: cubic-bezier(0.4, 0, 0.2, 1);   /* Custom */
transition-timing-function: steps(4, end);                  /* Discrete steps */
transition-timing-function: step-start;                     /* Jump to end immediately */
transition-timing-function: step-end;                       /* Stay at start, jump at end */
```

Custom cubic-bezier:

- `cubic-bezier(0.4, 0, 0.2, 1)` — standard easing (Material Design)
- `cubic-bezier(0.68, -0.55, 0.27, 1.55)` — bounce effect
- `cubic-bezier(0.4, 0, 1, 1)` — accelerate (like ease-in with sharper curve)

Visualize with browser DevTools (the cubic-bezier editor in the transition panel).

### Transforms

Transforms change an element's appearance and position without affecting layout:

```css
.element {
    transform: scale(1.5) rotate(45deg) translate(10px, 20px);
}
```

Multiple transforms are applied right to left (last listed = applied first).

### 2D Transforms

```css
.element {
    transform: translate(20px, 10px);     /* Move from current position */
    transform: translateX(50%);           /* 50% of element's own width */
    transform: translateY(1rem);

    transform: scale(1.5);                /* Enlarge 1.5x */
    transform: scaleX(0.5);               /* Squish horizontally */
    transform: scaleY(2);                 /* Stretch vertically */

    transform: rotate(45deg);             /* Clockwise rotation */
    transform: rotate(-0.25turn);         /* Counter-clockwise quarter turn */

    transform: skew(10deg, 5deg);         /* Tilt along X and Y axes */
    transform: skewX(10deg);

    transform: matrix(a, b, c, d, tx, ty); /* Combined — complex */
}
```

### 3D Transforms

```css
.container {
    perspective: 800px;             /* Depth of field on parent */
    perspective-origin: center;     /* Vanishing point */
}

.element {
    transform: translateZ(50px);              /* Move toward viewer */
    transform: translate3d(10px, 20px, 30px);
    transform: rotateX(45deg);                 /* Flip around X axis */
    transform: rotateY(180deg);                /* Flip around Y axis */
    transform: rotateZ(45deg);                 /* Same as rotate(45deg) */
    transform: scaleZ(2);                      /* Stretch in Z depth */
    transform: scale3d(1.5, 0.5, 2);

    /* Combined */
    transform: perspective(800px) rotateY(15deg) translateZ(20px);

    backface-visibility: hidden;               /* Hide rear face when rotated */
    transform-style: preserve-3d;              /* Children maintain 3D space */
}
```

### Individual Transform Properties

Individual properties (2024+) make transforms cleaner:

```css
.element {
    translate: 10px 20px;          /* Instead of transform: translate() */
    rotate: 45deg;                 /* Instead of transform: rotate() */
    scale: 1.5;                    /* Instead of transform: scale() */
}

/* Separate properties avoid ordering bugs */
/* transform: translate(10px) rotate(45deg) is different from rotate then translate */
/* Individual properties always apply in a fixed order: translate → rotate → scale */
```

### Animations and @keyframes

Keyframe animations define multi-step sequences:

```css
@keyframes slide-in {
    from {
        transform: translateX(-100%);
        opacity: 0;
    }

    to {
        transform: translateX(0);
        opacity: 1;
    }
}

.element {
    animation: slide-in 0.5s ease-out;
}
```

Percentage-based keyframes for more than two steps:

```css
@keyframes pulse {
    0% {
        transform: scale(1);
        opacity: 1;
    }
    50% {
        transform: scale(1.2);
        opacity: 0.7;
    }
    100% {
        transform: scale(1);
        opacity: 1;
    }
}

.element {
    animation: pulse 2s ease-in-out infinite;
}
```

### Animation Properties

```css
.element {
    animation-name: slide-in;             /* @keyframes name */
    animation-duration: 0.5s;             /* Time for one iteration */
    animation-timing-function: ease-out;  /* Speed curve */
    animation-delay: 0.2s;                /* Delay before start */
    animation-iteration-count: 1;         /* Number of repeats */
    animation-direction: normal;          /* Direction of play */
    animation-fill-mode: none;            /* Styles before/after animation */
    animation-play-state: running;        /* Pause/resume */
}
```

**animation-iteration-count:**

```css
animation-iteration-count: 1;             /* Play once (default) */
animation-iteration-count: 3;             /* Play 3 times */
animation-iteration-count: infinite;      /* Loop forever */
```

**animation-direction:**

```css
animation-direction: normal;              /* 0% → 100% (default) */
animation-direction: reverse;             /* 100% → 0% */
animation-direction: alternate;           /* 0% → 100%, then 100% → 0% */
animation-direction: alternate-reverse;   /* 100% → 0%, then 0% → 100% */
```

**animation-fill-mode:**

```css
animation-fill-mode: none;        /* No styles applied when animation is not playing */
animation-fill-mode: forwards;    /* Keeps the final keyframe styles after animation ends */
animation-fill-mode: backwards;   /* Applies first keyframe styles during delay */
animation-fill-mode: both;        /* Forwards + backwards */
```

**animation-play-state:**

```css
.element {
    animation: pulse 2s infinite;
}

.paused .element {
    animation-play-state: paused;          /* Pause on hover/class toggle */
}

.resumed .element {
    animation-play-state: running;
}
```

### Animation Shorthand

```css
.element {
    animation: name duration timing-function delay iteration-count direction fill-mode;
    animation: slide-in 0.5s ease-out 0.2s 1 normal forwards;
}
```

Only `name` and `duration` are required:

```css
.element {
    animation: fade-in 1s;
}
```

Multiple animations:

```css
.element {
    animation:
        fade-in 0.5s ease-out,
        slide-up 0.5s 0.2s ease-out both;
}
```

### Animation Timing Functions

Per-keyframe timing functions:

```css
@keyframes bounce {
    0% {
        transform: translateY(0);
        animation-timing-function: ease-out;   /* Descend accelerates */
    }
    50% {
        transform: translateY(-30px);
        animation-timing-function: ease-in;    /* Ascend decelerates */
    }
    100% {
        transform: translateY(0);
    }
}
```

Linear for continuous motion, ease-out for entrances, ease-in for exits.

### Animation Events

JavaScript animation events:

```javascript
element.addEventListener('animationstart', () => {});
element.addEventListener('animationend', () => {});
element.addEventListener('animationiteration', () => {});

// Transition events
element.addEventListener('transitionstart', () => {});
element.addEventListener('transitionend', () => {});
element.addEventListener('transitioncancel', () => {});
```

These are useful for sequencing UI changes:

```javascript
const modal = document.querySelector('.modal');
modal.addEventListener('animationend', () => {
    if (modal.classList.contains('closing')) {
        modal.style.display = 'none';
    }
});
```

### Will-Change

Hint to the browser about upcoming changes:

```css
.element {
    will-change: transform, opacity;
    /* Tells browser to prepare GPU layer for transform/opacity animations */
}
```

Use `will-change` sparingly:
- Only on elements that will animate
- Remove it after the animation (via JS)
- Do not apply to too many elements (memory cost)
- Apply it shortly before the animation starts, not permanently

```javascript
// Better: apply just before animation
element.addEventListener('mouseenter', () => {
    element.style.willChange = 'transform';
});

element.addEventListener('animationend', () => {
    element.style.willChange = 'auto';
});
```

### Performance Best Practices

**Animatable properties by performance tier:**

| Tier | Properties | Cost |
|---|---|---|
| Best | `transform`, `opacity` | Compositor-only, no layout/paint |
| Good | `color`, `background-color`, `border-color` | Paint only, no layout |
| Poor | `width`, `height`, `top`, `left`, `margin`, `padding` | Layout + paint + composite |

**Always prefer `transform` and `opacity` for animations:**

```css
/* Good — animates transform and opacity */
.element {
    transition: transform 0.3s, opacity 0.3s;
}

.element.hidden {
    transform: scale(0.8);
    opacity: 0;
}

/* Bad — animates layout properties */
.element {
    transition: width 0.3s, height 0.3s;  /* Triggers layout */
}

.element.hidden {
    width: 0;
    height: 0;
}
```

**Anti-pattern: animating `display`** — not animatable:

```css
/* Does not work */
.element {
    transition: display 0.3s;
    display: none;          /* No transition — it's binary */
}
```

Instead, animate opacity/transform, then set display via animationend.

### Reduced Motion

Respect user preferences:

```css
@media (prefers-reduced-motion: reduce) {
    *, *::before, *::after {
        animation-duration: 0.01ms !important;
        animation-iteration-count: 1 !important;
        transition-duration: 0.01ms !important;
    }
}
```

Or design motion-inclusive alternatives:

```css
@media (prefers-reduced-motion: reduce) {
    .card:hover {
        transform: none;               /* Remove lift animation */
        filter: brightness(0.98);      /* Use subtle static change instead */
    }
}
```

## Study Cases

### Smooth Page Entrance

```css
.page {
    animation: page-enter 0.4s ease-out;
}

@keyframes page-enter {
    from {
        opacity: 0;
        transform: translateY(1rem);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}
```

### Accordion with Max-Height

```css
.accordion-content {
    max-height: 0;
    overflow: hidden;
    transition: max-height 0.3s ease-out;
}

.accordion.is-open .accordion-content {
    max-height: 500px;               /* Large enough for content */
    transition: max-height 0.5s ease-in;
}
```

### Button Ripple Effect

```css
.button {
    position: relative;
    overflow: hidden;
}

.button::after {
    content: '';
    position: absolute;
    inset: 0;
    background: radial-gradient(circle, rgba(255,255,255,0.3) 0%, transparent 70%);
    opacity: 0;
    transition: opacity 0.3s;
}

.button:active::after {
    opacity: 1;
    transform: scale(2);
    transition: 0s;                  /* Instant on click */
}
```

## Examples

### Example 1: Hover lift effect

```css
.card {
    transition: transform 0.2s ease, box-shadow 0.2s ease;
}

.card:hover {
    transform: translateY(-4px);
    box-shadow: 0 8px 24px rgba(0, 0, 0, 0.12);
}
```

### Example 2: Loading spinner

```css
@keyframes spin {
    to { transform: rotate(360deg); }
}

.spinner {
    width: 24px;
    height: 24px;
    border: 3px solid #ddd;
    border-top-color: #0066cc;
    border-radius: 50%;
    animation: spin 0.8s linear infinite;
}
```

### Example 3: Staggered list entrance

```css
.list-item {
    opacity: 0;
    transform: translateX(-1rem);
    animation: list-enter 0.3s ease-out forwards;
}

.list-item:nth-child(1) { animation-delay: 0s; }
.list-item:nth-child(2) { animation-delay: 0.05s; }
.list-item:nth-child(3) { animation-delay: 0.1s; }
.list-item:nth-child(4) { animation-delay: 0.15s; }
.list-item:nth-child(5) { animation-delay: 0.2s; }

@keyframes list-enter {
    to {
        opacity: 1;
        transform: translateX(0);
    }
}
```

## Glossary

| Term | Definition |
|---|---|
| Transition | Smooth change between two states |
| Transform | Visual manipulation (scale, rotate, translate) |
| @keyframes | Definition of multi-step animation sequence |
| Timing function | Speed curve control (ease, linear, cubic-bezier) |
| Fill mode | Pre/post animation style behavior |
| Compositor | GPU layer responsible for rendering |
| Will-change | Performance hint for upcoming changes |
| Reduced motion | User preference to minimize animation |

## Quick References

- [MDN: CSS Transitions](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_transitions) — transition reference
- [MDN: CSS Transforms](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_transforms) — transform reference
- [MDN: CSS Animations](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_animations) — animation reference
- [Easing Functions Cheat Sheet](https://easings.net/) — visual timing functions
- [CSS Transform Spec Level 1](https://www.w3.org/TR/css-transforms-1/) — specification
- [CSS Animations Level 1](https://www.w3.org/TR/css-animations-1/) — specification

## Next Steps

- [BEM & CSS Architecture](bem-and-architecture.md) — organizing CSS at scale