# Values & Types

## Description

JavaScript has seven primitive types and one object type. Understanding how types work — coercion, equality, truthiness, and type checking — is essential to writing correct, bug-free code.

## Prerequisites

- [What Is JavaScript?](intro/what-is-javascript.md) — language overview

## Table of Contents

- [The Type System](#the-type-system)
- [Primitive Types](#primitive-types)
- [Objects & References](#objects--references)
- [Type Coercion](#type-coercion)
- [Equality Operators](#equality-operators)
- [Truthy & Falsy](#truthy--falsy)
- [Type Checking](#type-checking)
- [Converting Between Types](#converting-between-types)
- [Null vs. Undefined](#null-vs-undefined)
- [Symbol: The Hidden Primitive](#symbol-the-hidden-primitive)
- [BigInt: Arbitrary Precision](#bigint-arbitrary-precision)
- [Common Type Traps](#common-type-traps)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### The Type System

JavaScript is dynamically typed. Variables can hold any type, and the type can change at runtime. This flexibility is powerful but requires discipline.

```javascript
let value = 42;           // number
value = "hello";          // string — same variable, different type
value = { id: 1 };        // object
value = true;             // boolean
```

Despite dynamic typing, every value has a well-defined type at runtime. The `typeof` operator reveals it:

```javascript
typeof 42;                // "number"
typeof "hello";           // "string"
typeof true;              // "boolean"
typeof undefined;         // "undefined"
typeof null;              // "object" — this is a known bug
typeof Symbol();          // "symbol"
typeof 42n;               // "bigint"
typeof {};                // "object"
typeof [];                // "object"
typeof function() {};     // "function"
```

### Primitive Types

JavaScript has seven primitive types. Primitives are immutable and compared by value.

**String** represents textual data:

```javascript
const single = 'hello';
const double = "hello";
const backtick = `hello ${name}`; // template literal with interpolation

// Strings are immutable
let str = "hello";
str[0] = "j";              // no error, but str is still "hello"
str = "jello";             // reassignment, not mutation

// Common string methods
"hello".length;            // 5
"hello".toUpperCase();     // "HELLO"
"hello".indexOf("l");      // 2
"hello".slice(1, 3);       // "el"
"a,b,c".split(",");        // ["a", "b", "c"]
```

**Number** represents both integers and floating-point:

```javascript
typeof 42;                 // "number"
typeof 3.14;               // "number"
typeof NaN;                // "number" — NaN is a number type!
typeof Infinity;           // "number"

// Special values
NaN === NaN;               // false — NaN is never equal to itself
Number.isNaN("abc");       // false — safe check
isNaN("abc");              // true — coerces to number first

// Floating point precision
0.1 + 0.2;                // 0.30000000000000004
```

**Boolean** represents logical values:

```javascript
const isActive = true;
const isComplete = false;

// Boolean coercion
Boolean(1);                // true
Boolean(0);                // false
Boolean("");               // false
Boolean("hello");          // true
```

**Undefined** represents a value that has not been assigned:

```javascript
let x;
typeof x;                  // "undefined"

function noReturn() {}
noReturn();                // undefined

const obj = {};
obj.nonexistent;           // undefined
```

**Null** represents the intentional absence of a value:

```javascript
const empty = null;
typeof null;               // "object" — this is a well-known JavaScript bug

// Historical note: typeof null returns "object" because of a bug
// in the original JavaScript implementation. It cannot be fixed
// because fixing it would break existing code.
```

**Symbol** creates unique, immutable identifiers:

```javascript
const sym1 = Symbol("id");
const sym2 = Symbol("id");
sym1 === sym2;             // false — symbols are always unique

const obj = { [sym1]: "secret" };
obj[sym1];                 // "secret"
Object.keys(obj);          // [] — symbols are hidden from enumeration
```

**BigInt** represents integers of arbitrary size:

```javascript
const big = 9007199254740991n;
const big2 = BigInt("9007199254740991");
big + 1n;                  // 9007199254740992n
big + 1;                   // TypeError: cannot mix BigInt and number
```

### Objects & References

Objects are the only non-primitive type. They are compared by reference, not value:

```javascript
const a = { id: 1 };
const b = { id: 1 };
const c = a;

a === b;                   // false — different references
a === c;                   // true — same reference

// Objects include:
// - Plain objects: {}
// - Arrays: []
// - Functions: function() {}
// - Dates: new Date()
// - RegExps: /pattern/
// - Map, Set, WeakMap, WeakSet
```

Objects are mutable by default:

```javascript
const obj = { name: "Alice" };
obj.name = "Bob";           // mutation is allowed
obj.age = 30;               // adding properties is allowed

// Even with const, the object can be mutated
// const only prevents reassignment of the variable
```

### Type Coercion

JavaScript automatically converts types in certain contexts. This is convenient but can produce surprising results.

**String coercion** happens with the `+` operator when one operand is a string:

```javascript
"5" + 3;                   // "53" — number coerced to string
"5" + "3";                 // "53"
"Hello" + 42;              // "Hello42"
5 + "5";                   // "55"
```

**Numeric coercion** happens with all other operators:

```javascript
"5" - 3;                   // 2 — string coerced to number
"5" * "3";                 // 15
"10" / 2;                  // 5
"5" - "3";                 // 2

+"42";                     // 42 — unary plus coerces to number
+"hello";                  // NaN

// Subtraction, multiplication, division always coerce to numbers
```

**Boolean coercion** happens in conditional contexts:

```javascript
if ("hello") { /* runs — string is truthy */ }
if (0) { /* doesn't run — zero is falsy */ }
if ([]) { /* runs — empty array is truthy */ }

// Logical operators return the actual value, not boolean
"hello" || "world";        // "hello" — short-circuit
"" || "world";             // "world"
"hello" && "world";        // "world"
"" && "world";             // ""
```

### Equality Operators

JavaScript has four equality behaviours:

**Strict equality (`===`)** — no coercion. Only true if same type and same value:

```javascript
42 === 42;                 // true
"42" === 42;               // false — different types
null === null;             // true
undefined === undefined;   // true
NaN === NaN;               // false — NaN is never equal to itself
```

**Loose equality (`==`)** — coerces types before comparing:

```javascript
"42" == 42;                // true — string coerced to number
1 == true;                 // true — boolean coerced to number
"" == false;               // true — both coerce to 0
null == undefined;         // true — special case
[] == false;               // true — complex coercion chain
```

**`Object.is`** — same-value equality, handles NaN correctly:

```javascript
Object.is(NaN, NaN);       // true
Object.is(-0, 0);          // false
Object.is(42, 42);         // true
```

**Same-value-zero** — used by `Set`, `Map`, and `Array.prototype.includes`:

```javascript
// Same as Object.is but considers -0 and 0 equal
[NaN].includes(NaN);       // true
```

### Truthy & Falsy

Every value in JavaScript has an inherent truthiness. Falsy values coerce to `false` in boolean contexts. There are exactly seven falsy values:

```javascript
false;                     // the boolean false
0;                         // the number zero
-0;                        // negative zero
0n;                        // BigInt zero
"";                        // empty string
null;                      // null
undefined;                 // undefined
NaN;                       // Not-a-Number
```

Everything else is truthy, including:

```javascript
"false";                   // non-empty string — truthy!
"0";                       // non-empty string — truthy!
[];                        // empty array — truthy!
{};                        // empty object — truthy!
Infinity;                  // truthy
```

### Type Checking

Reliable type checking requires knowing which method works for each type:

```javascript
// Primitive types
typeof "hello" === "string";
typeof 42 === "number";
typeof true === "boolean";
typeof undefined === "undefined";
typeof Symbol() === "symbol";
typeof 42n === "bigint";

// Special case: null
value === null;

// Array
Array.isArray([]);         // true
Array.isArray({});         // false

// NaN
Number.isNaN(NaN);         // true
Number.isNaN("abc");       // false — no coercion

// Integer
Number.isInteger(42);      // true
Number.isInteger(3.14);    // false

// Finite number
Number.isFinite(Infinity); // false
Number.isFinite(42);       // true

// Object (excluding null)
value !== null && typeof value === "object";

// Plain object (not array, not null)
Object.prototype.toString.call(value) === "[object Object]";
```

### Converting Between Types

Explicit conversion makes code more predictable:

```javascript
// To string
String(42);                // "42"
String(true);              // "true"
(42).toString();           // "42"
String(null);              // "null"
String(undefined);         // "undefined"

// To number
Number("42");              // 42
Number("42.5");            // 42.5
Number("hello");           // NaN
Number(true);              // 1
Number(false);             // 0
Number(null);              // 0
Number(undefined);         // NaN
Number("");                // 0

// parseInt and parseFloat — parse until invalid character
parseInt("42px");          // 42
parseInt("1010", 2);       // 10 — binary
parseFloat("3.14em");      // 3.14

// To boolean
Boolean(1);                // true
Boolean(0);                // false
Boolean("hello");          // true
Boolean("");               // false
Boolean({});               // true
Boolean([]);               // true
Boolean(null);             // false

// Using !! as shorthand
!!1;                       // true
!!0;                       // false
```

### Null vs. Undefined

The distinction between `null` and `undefined` is subtle but important:

```javascript
// undefined — the language sets this automatically
let x;
x;                         // undefined

const obj = {};
obj.key;                   // undefined

function f() {}
f();                       // undefined

// null — the developer sets this intentionally
const user = null;         // "no user yet, but explicitly set"
```

**Recommended convention:**
- Let `undefined` represent missing or uninitialised values (the language sets this)
- Use `null` when you want to explicitly signal "no value" in your code

```javascript
function findUser(id) {
    const user = database.get(id);
    return user ?? null;  // explicitly null when not found
}
```

### Symbol: The Hidden Primitive

Symbols create guaranteed-unique property keys:

```javascript
const id = Symbol("id");
const user = {
    name: "Alice",
    [id]: 12345
};

user[id];                  // 12345
user.id;                   // undefined — not the same

// Symbols are not enumerated
Object.keys(user);         // ["name"]
JSON.stringify(user);      // '{"name":"Alice"}'

// Global symbol registry
Symbol.for("app.role");    // shared symbol across realms
Symbol.keyFor(sym);        // "app.role"
```

Use cases for symbols:
- Private-like properties (hiding metadata)
- Protocol methods (Symbol.iterator, Symbol.toPrimitive)
- Preventing property name collisions

### BigInt: Arbitrary Precision

BigInt handles integers beyond the safe range of `Number`:

```javascript
const maxSafe = Number.MAX_SAFE_INTEGER; // 9007199254740991
const bigger = 9007199254740992n;

bigger + 1n;               // 9007199254740993n
bigger ** 2n;              // huge — exact precision

// Cannot mix with regular numbers
bigger + 1;                // TypeError

// Must convert explicitly
Number(bigger) + 1;        // 9007199254740993 — may lose precision
BigInt(42) + bigger;       // works
```

BigInt is useful for:
- Cryptography and hashing
- Financial calculations with large amounts
- Working with API IDs that exceed Number.MAX_SAFE_INTEGER

### Common Type Traps

**Trap 1: Adding arrays**

```javascript
[] + [];                   // "" — empty arrays become empty strings
[] + {};                   // "[object Object]"
{} + [];                   // 0 — {} is interpreted as an empty block
```

**Trap 2: Comparing NaN**

```javascript
NaN === NaN;               // false — use Number.isNaN() instead
```

**Trap 3: typeof null**

```javascript
typeof null;               // "object" — use value === null instead
```

**Trap 4: Floating point precision**

```javascript
0.1 + 0.2 === 0.3;         // false — 0.1 + 0.2 = 0.30000000000000004
```

**Trap 5: Checking if a value is a number**

```javascript
typeof NaN;                // "number" — NaN is technically a number type
```

**Trap 6: Comma operator in conditions**

```javascript
const a = (1, 2, 3);       // a = 3 — comma returns the last operand
```

## Study Cases

### Case 1: Safe Number Parsing

**Scenario:** A function receives user input that should be a number but might be a string like "42" or "42px" or completely invalid.

```javascript
function parsePositiveInt(input) {
    const num = Number(input);
    if (!Number.isInteger(num) || num <= 0) {
        return null;
    }
    return num;
}

parsePositiveInt("42");    // 42
parsePositiveInt("42.5");  // null — not integer
parsePositiveInt("-5");    // null — not positive
parsePositiveInt("abc");   // null — NaN
parsePositiveInt("");      // null — 0, not positive
```

### Case 2: Deep Equality Check

**Scenario:** Comparing objects by value instead of reference.

```javascript
function deepEqual(a, b) {
    if (a === b) return true;
    if (typeof a !== "object" || a === null ||
        typeof b !== "object" || b === null) {
        return false;
    }
    const keysA = Object.keys(a);
    const keysB = Object.keys(b);
    if (keysA.length !== keysB.length) return false;
    return keysA.every(key =>
        keysB.includes(key) && deepEqual(a[key], b[key])
    );
}
```

### Case 3: Validating Type at Runtime

**Scenario:** A function that expects a specific type from an untrusted source.

```javascript
function safeString(value, fallback = "") {
    if (typeof value === "string") return value;
    if (typeof value === "number") return String(value);
    return fallback;
}
```

## Examples

### Example 1: Type-Coercion-Safe Comparison

```javascript
function looseCompare(a, b) {
    if (a == null && b == null) return true;
    return Object.is(a, b);
}
```

### Example 2: Truthiness-Based Defaults

```javascript
function greet(name) {
    name = name || "Guest";
    return `Hello, ${name}!`;
}
// But be careful — name = "" would give "Guest" too
// Better: nullish coalescing
function greetBetter(name) {
    name = name ?? "Guest";
    return `Hello, ${name}!`;
}
```

### Example 3: Type-Safe Addition

```javascript
function add(a, b) {
    if (typeof a !== "number" || typeof b !== "number") {
        throw new TypeError("Both arguments must be numbers");
    }
    return a + b;
}
```

## Glossary

| Term | Definition |
|------|------------|
| BigInt | A primitive for integers of arbitrary size |
| Coercion | Implicit type conversion performed by JavaScript |
| Falsy | A value that coerces to `false` (0, "", null, undefined, NaN, false) |
| Mutation | Changing an object's properties or contents |
| Null | The primitive value representing intentional absence |
| Primitive | A basic data type that is immutable and compared by value |
| Reference | A pointer to an object in memory |
| Strict equality | `===` — comparison without type coercion |
| Symbol | A unique, immutable primitive used as object keys |
| Truthy | Any value that is not falsy |
| Type coercion | Automatic conversion between types |
| Undefined | The primitive value for uninitialised variables |

## Quick References

- [MDN: JavaScript Types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures) — data structures reference
- [MDN: Equality Comparisons](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Equality_comparisons_and_sameness) — equality table
- [JavaScript Equality Table](https://dorey.github.io/JavaScript-Equality-Table/) — visual coercion guide
- [You Don't Know JS: Types](https://github.com/getify/You-Dont-Know-JS/tree/2nd-ed/types-grammar) — in-depth types book

## Next Steps

- [Functions in Depth](functions-in-depth.md) — closures, scope, and this binding
- [Objects & Prototypes](objects-and-prototypes.md) — inheritance and classes
- [ES6+ Features](es6-features.md) — modern syntax
