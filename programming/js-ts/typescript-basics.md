# TypeScript Basics

## Description

TypeScript is a typed superset of JavaScript that compiles to plain JavaScript. It adds static type checking, interfaces, generics, and modern language features that scale from solo projects to large enterprise codebases.

## Prerequisites

- [ES6+ Features](es6-plus.md) — modules, destructuring, classes
- [Values & Types](values-and-types.md) — JavaScript type system fundamentals

## Table of Contents

- [What TypeScript Adds](#what-typescript-adds)
- [Setting Up TypeScript](#setting-up-typescript)
- [Basic Types](#basic-types)
- [Type Inference](#type-inference)
- [Interfaces](#interfaces)
- [Type Aliases](#type-aliases)
- [Union & Intersection Types](#union--intersection-types)
- [Literal Types](#literal-types)
- [Type Guards](#type-guards)
- [Generics](#generics)
- [Enums](#enums)
- [Utility Types](#utility-types)
- [Modules & Declaration Files](#modules--declaration-files)
- [TypeScript with React](#typescript-with-react)
- [Study Cases](#study-cases)
- [Examples](#examples)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### What TypeScript Adds

TypeScript catches entire categories of bugs at compile time:

| JavaScript bug | TypeScript catches |
|---|---|
| `"hello" - 1` → `NaN` | Type error: string minus number |
| `user.name` when `user` is `null` | Accessing property of nullable object |
| `fn(a, b)` when function expects `(b, a)` | Wrong argument types |
| `obj[typo]` instead of `obj[correctKey]` | Unknown property access |

**Compile-time vs. runtime:**

TypeScript types exist only at compile time. They are erased when compiled to JavaScript. There is zero runtime overhead.

```typescript
// TypeScript (with types)
function greet(name: string): string {
    return `Hello, ${name.toUpperCase()}`;
}

// Compiled JavaScript (no types)
function greet(name) {
    return `Hello, ${name.toUpperCase()}`;
}
```

### Setting Up TypeScript

**Installation:**

```bash
npm install -D typescript
npx tsc --init                                 # creates tsconfig.json
```

**Basic tsconfig.json:**

```json
{
    "compilerOptions": {
        "target": "ES2022",                    // output JS version
        "module": "ESNext",                    // module system
        "moduleResolution": "bundler",         // use bundler-style resolution
        "strict": true,                        // enable all strict checks
        "outDir": "./dist",                    // output directory
        "rootDir": "./src",                    // source directory
        "sourceMap": true,                     // debug with original TS
        "esModuleInterop": true,               // better CJS compatibility
        "skipLibCheck": true                   // skip .d.ts type checking
    },
    "include": ["src"]
}
```

**Key compiler options:**

| Option | Purpose |
|--------|---------|
| `strict: true` | Enables all strict checks (recommended for all projects) |
| `noImplicitAny` | Error when type cannot be inferred |
| `strictNullChecks` | `null`/`undefined` are not assignable to other types |
| `noUnusedLocals` | Error on unused variables |
| `noUnusedParameters` | Error on unused function parameters |
| `exactOptionalPropertyTypes` | Stricter handling of optional properties |

**Compilation:**

```bash
npx tsc                                       # compile once
npx tsc --watch                                # watch mode
npx tsc --noEmit                               # type-check only (no output)
```

For most projects, the build tool (Vite, webpack, etc.) handles TypeScript compilation. Use `tsc --noEmit` for type checking only:

```json
{
    "scripts": {
        "typecheck": "tsc --noEmit",
        "build": "npm run typecheck && vite build"
    }
}
```

### Basic Types

**Primitive types:**

```typescript
let name: string = "Alice";
let age: number = 30;
let isActive: boolean = true;
let nothing: null = null;
let notDefined: undefined = undefined;
let big: bigint = 100n;
let unique: symbol = Symbol("id");
```

**Type inference — TypeScript can usually figure out the type:**

```typescript
let name = "Alice";                            // inferred as string
let age = 30;                                  // inferred as number
let isActive = true;                           // inferred as boolean
```

**Arrays:**

```typescript
let numbers: number[] = [1, 2, 3];
let names: Array<string> = ["Alice", "Bob"];   // generic syntax
let mixed: (string | number)[] = [1, "two"];   // union array

// Readonly array
let readonly: readonly number[] = [1, 2, 3];
// readonly.push(4);                           // Error: push does not exist on readonly
```

**Tuples — fixed-length arrays with known types:**

```typescript
let pair: [string, number] = ["Alice", 30];
let httpResponse: [number, string] = [200, "OK"];

// Optional elements
let optional: [string, number?] = ["hello"];

// Labeled tuples (TS 4.0+)
let user: [name: string, age: number] = ["Alice", 30];
```

**Objects:**

```typescript
let user: { name: string; age: number } = {
    name: "Alice",
    age: 30
};

// Optional properties
let config: { host: string; port?: number } = {
    host: "localhost"
};

// Index signature — dynamic keys
let dict: { [key: string]: number } = {
    apples: 5,
    oranges: 3
};
```

**Function types:**

```typescript
// Parameter types + return type
function add(a: number, b: number): number {
    return a + b;
}

// Arrow function with type
const multiply = (a: number, b: number): number => a * b;

// Void return
function log(message: string): void {
    console.log(message);
    // no return
}

// Never — function never returns (throws or infinite loop)
function throwError(message: string): never {
    throw new Error(message);
}

// Optional parameters
function greet(name: string, greeting?: string): string {
    return `${greeting ?? "Hello"}, ${name}!`;
}

// Default parameters (type inferred)
function createUser(name: string, age: number = 18) {
    return { name, age };
}

// Rest parameters
function sum(...numbers: number[]): number {
    return numbers.reduce((a, b) => a + b, 0);
}
```

**Function type as value:**

```typescript
type MathFn = (a: number, b: number) => number;

const add: MathFn = (x, y) => x + y;
const subtract: MathFn = (x, y) => x - y;

function compute(fn: MathFn, a: number, b: number): number {
    return fn(a, b);
}
```

### Type Inference

TypeScript infers types even without explicit annotations:

```typescript
let x = 10;                                    // number
let y = "hello";                               // string
let z = true;                                  // boolean

const arr = [1, 2, 3];                         // number[]
const obj = { a: 1, b: "two" };               // { a: number; b: string }

// Return type inferred
function add(a: number, b: number) {
    return a + b;                              // inferred: number
}
```

**Contextual typing — type from usage context:**

```typescript
const names = ["Alice", "Bob", "Charlie"];

names.forEach((name) => {                      // name inferred as string
    console.log(name.toUpperCase());
});
```

**Best common type — when inferring from arrays:**

```typescript
const mixed = [1, "hello", true];              // (string | number | boolean)[]
```

**Literal inference — const vs. let:**

```typescript
const role = "admin";                          // type: "admin" (literal)
let role2 = "admin";                           // type: string

// Force literal with const assertion
let role3 = "admin" as const;                  // type: "admin"
const config = {
    host: "localhost",
    port: 8080
} as const;                                     // type: { readonly host: "localhost"; readonly port: 8080 }
```

### Interfaces

Interfaces define the shape of an object:

```typescript
interface User {
    id: number;
    name: string;
    email: string;
    age?: number;                               // optional
    readonly createdAt: Date;                   // read-only (can't reassign)
}

function createUser(user: User): User {
    return user;
}

const alice: User = {
    id: 1,
    name: "Alice",
    email: "alice@example.com",
    createdAt: new Date()
};

// alice.createdAt = new Date();                // Error: readonly
```

**Extending interfaces:**

```typescript
interface BaseEntity {
    id: number;
    createdAt: Date;
}

interface User extends BaseEntity {
    name: string;
    email: string;
}

interface Admin extends User {
    permissions: string[];
    role: "admin";
}
```

**Interface merging (declaration merging):**

```typescript
interface Request {
    body: string;
}

interface Request {
    headers: Record<string, string>;
}

// Request now has both body and headers
const req: Request = {
    body: "data",
    headers: { "Content-Type": "application/json" }
};
```

**Interface methods:**

```typescript
interface Comparator {
    (a: number, b: number): number;            // call signature
}

const sortAsc: Comparator = (a, b) => a - b;

interface Counter {
    count: number;
    increment(): void;                          // method
    reset(): void;
}
```

### Type Aliases

Type aliases name a type:

```typescript
type ID = number | string;
type JSONValue = string | number | boolean | null | JSONValue[] | { [key: string]: JSONValue };

type Point = {
    x: number;
    y: number;
};

type Color = "red" | "green" | "blue";
```

**Interface vs. type alias:**

| Feature | Interface | Type alias |
|---------|-----------|------------|
| Extending | `extends` | `&` (intersection) |
| Merging | Yes (declaration merging) | No |
| Primitives/Unions | No | Yes |
| Performance | Better (cached) | — |

```typescript
// Extending with interfaces
interface Animal { name: string }
interface Dog extends Animal { breed: string }

// Extending with types
type Animal = { name: string };
type Dog = Animal & { breed: string };
```

Choose `interface` for public API shapes (they merge and give better error messages). Choose `type` for unions, intersections, and complex compositions.

### Union & Intersection Types

**Union types — value can be one of several types:**

```typescript
type Status = "active" | "inactive" | "pending";
type Result = number | string;
type Nullable<T> = T | null | undefined;

function printId(id: number | string) {
    if (typeof id === "string") {
        console.log(id.toUpperCase());          // narrowed to string
    } else {
        console.log(id.toFixed(2));             // narrowed to number
    }
}
```

**Discriminated unions — common property to distinguish:**

```typescript
type Shape =
    | { kind: "circle"; radius: number }
    | { kind: "rectangle"; width: number; height: number }
    | { kind: "triangle"; base: number; height: number };

function area(shape: Shape): number {
    switch (shape.kind) {
        case "circle":
            return Math.PI * shape.radius ** 2;
        case "rectangle":
            return shape.width * shape.height;
        case "triangle":
            return (shape.base * shape.height) / 2;
    }
}
```

**Intersection types — combine multiple types:**

```typescript
type WithId = { id: number };
type WithTimestamp = { createdAt: Date; updatedAt: Date };

type Entity = WithId & WithTimestamp;

const entity: Entity = {
    id: 1,
    createdAt: new Date(),
    updatedAt: new Date()
};
```

### Literal Types

Specific values as types:

```typescript
type Direction = "north" | "south" | "east" | "west";
type HTTPMethod = "GET" | "POST" | "PUT" | "DELETE" | "PATCH";
type Port = 80 | 443 | 3000;
type Truthy = true;

function move(direction: Direction, distance: number) {
    console.log(`Moving ${direction} by ${distance}`);
}

move("north", 10);                             // OK
// move("up", 10);                             // Error: "up" not in Direction
```

### Type Guards

Runtime checks that narrow types:

**typeof guard:**

```typescript
function format(value: string | number): string {
    if (typeof value === "string") {
        return value.toUpperCase();             // narrowed to string
    }
    return value.toFixed(2);                    // narrowed to number
}
```

**instanceof guard:**

```typescript
class ApiError extends Error { status: number; }
class NetworkError extends Error { retryable: boolean; }

function handleError(error: ApiError | NetworkError) {
    if (error instanceof ApiError) {
        console.log(`API Error: ${error.status}`);
    } else {
        console.log(`Network Error, retry: ${error.retryable}`);
    }
}
```

**in guard:**

```typescript
type Fish = { swim: () => void };
type Bird = { fly: () => void };

function move(animal: Fish | Bird) {
    if ("swim" in animal) {
        animal.swim();
    } else {
        animal.fly();
    }
}
```

**Custom type guard:**

```typescript
interface User { name: string; email: string; }
interface Admin extends User { role: "admin"; permissions: string[]; }

function isAdmin(user: User): user is Admin {
    return "role" in user && user.role === "admin";
}

function processUser(user: User) {
    if (isAdmin(user)) {
        console.log(user.permissions);          // narrowed to Admin
    }
}
```

**Assertion function:**

```typescript
function assertNonNull<T>(value: T): asserts value is NonNullable<T> {
    if (value === null || value === undefined) {
        throw new Error("Value is null or undefined");
    }
}

function getLength(value: string | null) {
    assertNonNull(value);
    return value.length;                        // narrowed to string
}
```

### Generics

Generics let you write reusable code that works with many types:

```typescript
function identity<T>(value: T): T {
    return value;
}

const num = identity(42);                      // type: number
const str = identity("hello");                 // type: string
```

**Generic constraints:**

```typescript
interface HasLength {
    length: number;
}

function logLength<T extends HasLength>(value: T): void {
    console.log(value.length);                  // T must have .length
}

logLength("hello");                             // OK: string has length
logLength([1, 2, 3]);                           // OK: array has length
// logLength(42);                               // Error: number has no length
```

**Generic interfaces:**

```typescript
interface Repository<T> {
    get(id: number): Promise<T | null>;
    getAll(): Promise<T[]>;
    create(data: Omit<T, "id">): Promise<T>;
    update(id: number, data: Partial<T>): Promise<T>;
    delete(id: number): Promise<void>;
}

class UserRepository implements Repository<User> {
    async get(id: number): Promise<User | null> {
        // implementation
    }
    // ...
}
```

**Generic functions with multiple type parameters:**

```typescript
function pair<A, B>(a: A, b: B): [A, B] {
    return [a, b];
}

const result = pair("hello", 42);              // [string, number]
```

**Generic default types:**

```typescript
function createArray<T = string>(length: number, value: T): T[] {
    return Array(length).fill(value);
}

const arr1 = createArray(3, "hello");          // string[]
const arr2 = createArray<number>(3, 42);       // number[]
```

**Inferring generic from callback:**

```typescript
function mapArray<T, U>(arr: T[], fn: (item: T) => U): U[] {
    return arr.map(fn);
}

const lengths = mapArray(["a", "bb", "ccc"], s => s.length);
// type: number[]
```

### Enums

**Numeric enums:**

```typescript
enum Direction {
    North,                                      // 0
    East,                                       // 1
    South,                                      // 2
    West                                        // 3
}

console.log(Direction.North);                   // 0
console.log(Direction[0]);                      // "North" (reverse mapping)
```

**String enums (preferred):**

```typescript
enum Color {
    Red = "RED",
    Green = "GREEN",
    Blue = "BLUE"
}

console.log(Color.Red);                         // "RED"
// No reverse mapping for string enums
```

**Const enums — completely inlined at compile time:**

```typescript
const enum Size {
    Small = "S",
    Medium = "M",
    Large = "L"
}

const mySize = Size.Medium;                     // compiled to: "M"
```

### Utility Types

Built-in generic type transformations:

```typescript
interface User {
    id: number;
    name: string;
    email: string;
    role: "admin" | "user";
}

// Partial — all properties optional
type PartialUser = Partial<User>;

// Required — all properties required
type RequiredUser = Required<PartialUser>;

// Readonly — all properties readonly
type ReadonlyUser = Readonly<User>;

// Pick — select specific properties
type UserSummary = Pick<User, "id" | "name">;

// Omit — remove specific properties
type UserWithoutEmail = Omit<User, "email">;

// Record — key-value map
type UserMap = Record<number, User>;

// Extract — extract types that match
type Numbers = Extract<string | number | boolean, number | string>;

// Exclude — remove types that match
type StringsOnly = Exclude<string | number | boolean, number | boolean>;

// NonNullable — remove null/undefined
type NonNull = NonNullable<string | null | undefined>;

// ReturnType — get return type of function
function fn() { return { x: 1, y: 2 }; }
type FnReturn = ReturnType<typeof fn>;          // { x: number; y: number }

// Parameters — get parameter types as tuple
type FnParams = Parameters<typeof fn>;          // []

// Awaited — unwrap Promise
type Data = Awaited<Promise<string>>;           // string

// InstanceType — instance type from constructor
class MyClass { method() {} }
type Instance = InstanceType<typeof MyClass>;
```

## Glossary

| Term | Definition |
|------|------------|
| TypeScript | Typed superset of JavaScript that compiles to plain JS |
| Type annotation | Explicitly declaring the type of a variable or parameter |
| Type inference | TypeScript deducing the type without annotations |
| Interface | A named type definition for object shapes |
| Type alias | A named type (can be union, intersection, primitive) |
| Union type | Value can be one of several types (`A \| B`) |
| Intersection type | Value must satisfy all types (`A & B`) |
| Discriminated union | Union types distinguished by a common property |
| Generic | Reusable type parameter (`<T>`) |
| Type guard | Runtime check that narrows a type |
| Enum | Named set of constants |
| Utility type | Built-in type transformations (Partial, Pick, etc.) |
| Mapped type | Transforms properties of an existing type |
| Conditional type | Type that depends on a condition (`T extends U ? X : Y`) |
| Declaration file | `.d.ts` file describing JavaScript module types |
| Strict mode | TypeScript compiler option enabling all strict checks |
| Never | Type for values that never occur (unreachable code) |
| Unknown | Type-safe counterpart of `any` — must be narrowed before use |

## Quick References

- [TypeScript Handbook](https://www.typescriptlang.org/docs/handbook/intro.html)
- [TypeScript Playground](https://www.typescriptlang.org/play)

## Next Steps

- [JavaScript Modules & Tooling](modules-and-tooling.md)
- [ES6+ Features](es6-plus.md)
