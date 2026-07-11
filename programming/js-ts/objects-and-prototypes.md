# Objects & Prototypes

## Description

Objects are JavaScript's fundamental data structure. From plain objects to classes, prototypes to property descriptors, understanding objects is essential to mastering the language.

## Prerequisites

- [Values & Types](values-and-types.md) — type system and reference types
- [Functions in Depth](functions-in-depth.md) — closures and this binding

## Table of Contents

- [Object Literals](#object-literals)
- [Property Access & Assignment](#property-access--assignment)
- [Computed Properties](#computed-properties)
- [Property Descriptors](#property-descriptors)
- [Getters & Setters](#getters--setters)
- [Object Methods & `this`](#object-methods--this)
- [The Prototype Chain](#the-prototype-chain)
- [Constructor Functions](#constructor-functions)
- [The `class` Syntax](#the-class-syntax)
- [Static Methods & Properties](#static-methods--properties)
- [Private Fields](#private-fields)
- [Inheritance with `extends`](#inheritance-with-extends)
- [Mixin Pattern](#mixin-pattern)
- [Object Utilities](#object-utilities)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Object Literals

The simplest way to create an object is with literal syntax:

```javascript
const user = {
    name: "Alice",
    age: 30,
    email: "alice@example.com"
};
```

**Shorthand properties** — when the variable name matches the key:

```javascript
const name = "Alice";
const age = 30;

const user = { name, age };
// { name: "Alice", age: 30 }
```

**Shorthand methods:**

```javascript
const user = {
    name: "Alice",
    greet() {           // instead of greet: function()
        return `Hello, ${this.name}`;
    }
};
```

**Nested objects:**

```javascript
const user = {
    name: "Alice",
    address: {
        street: "123 Main St",
        city: "Portland",
        zip: "97201"
    },
    preferences: {
        theme: "dark",
        notifications: true
    }
};
```

### Property Access & Assignment

Two syntaxes for property access:

```javascript
const user = { name: "Alice" };

// Dot notation — static keys only
user.name;               // "Alice"
user.age;                // undefined

// Bracket notation — dynamic keys
const key = "name";
user[key];               // "Alice"
user["age"] = 30;        // assignment with bracket

// Bracket notation supports special characters
user["first-name"] = "Alice"; // hyphen requires brackets
```

Checking if a property exists:

```javascript
"name" in user;           // true — checks own + inherited
user.hasOwnProperty("name"); // true — own only
user.name !== undefined;  // works but false for { name: undefined }

// Optional chaining — safe deep access
user.address?.city;       // "Portland" if address exists
user.profile?.bio;        // undefined — no error
```

Deleting properties:

```javascript
delete user.age;          // removes age property
user.age;                 // undefined
```

### Computed Properties

Dynamic property keys with bracket notation in literals:

```javascript
const key = "dynamicKey";
const obj = {
    [key]: "dynamic value",
    [`${key}2`]: "another"
};

// Use case: mapping values
function createMap(items) {
    const map = {};
    for (const item of items) {
        map[item.id] = item;
    }
    return map;
}
```

### Property Descriptors

Every property has descriptors that control its behaviour:

```javascript
const user = { name: "Alice" };

const descriptor = Object.getOwnPropertyDescriptor(user, "name");
// {
//   value: "Alice",
//   writable: true,    — can be reassigned
//   enumerable: true,  — appears in for...in / Object.keys
//   configurable: true — can be deleted or reconfigured
// }
```

Defining properties with custom descriptors:

```javascript
Object.defineProperty(user, "id", {
    value: 12345,
    writable: false,      // read-only
    enumerable: false,    // hidden from enumeration
    configurable: false   // cannot be deleted or changed
});

user.id;                 // 12345
user.id = 999;           // fails silently (or throws in strict mode)
Object.keys(user);       // ["name"] — id is hidden
```

`Object.defineProperties` defines multiple at once:

```javascript
Object.defineProperties(user, {
    id: { value: 12345, writable: false },
    role: { value: "admin", writable: true }
});
```

### Getters & Setters

Getters and setters are computed properties accessed like data properties:

```javascript
const user = {
    firstName: "Alice",
    lastName: "Smith",

    get fullName() {
        return `${this.firstName} ${this.lastName}`;
    },

    set fullName(value) {
        [this.firstName, this.lastName] = value.split(" ");
    }
};

user.fullName;           // "Alice Smith"
user.fullName = "Bob Jones";
user.firstName;          // "Bob"
```

Getters/setters with `Object.defineProperty`:

```javascript
function createUser(first, last) {
    const obj = { firstName: first, lastName: last };
    Object.defineProperty(obj, "fullName", {
        get() { return `${this.firstName} ${this.lastName}`; },
        set(value) {
            [this.firstName, this.lastName] = value.split(" ");
        },
        enumerable: true
    });
    return obj;
}
```

Use cases:
- Computed properties (full name, age from birth date)
- Validation before assignment
- Lazy loading expensive values
- Logging or tracking changes

### Object Methods & `this`

Methods are functions stored as properties. The `this` value depends on how the method is called:

```javascript
const user = {
    name: "Alice",
    greet() {
        return `Hi, I'm ${this.name}`;
    }
};

user.greet();            // "Hi, I'm Alice"

const ref = user.greet;
ref();                   // "Hi, I'm undefined" — lost this context
```

Arrow functions as methods — they inherit `this` from the enclosing scope:

```javascript
function createUser(name) {
    return {
        name,
        greet: () => `Hi, I'm ${this.name}` // BAD — this is from createUser scope
    };
}

// Better: use method shorthand with regular function
function createUser(name) {
    return {
        name,
        greet() {
            return `Hi, I'm ${this.name}`;
        }
    };
}
```

### The Prototype Chain

JavaScript uses prototype-based inheritance. Every object has an internal `[[Prototype]]` link to another object:

```javascript
const parent = { species: "human" };
const child = Object.create(parent);
child.name = "Alice";

child.name;              // "Alice" — own property
child.species;           // "human" — inherited from parent
child.toString();        // inherited from Object.prototype
```

The prototype chain lookup:
1. Check own properties
2. If not found, check `[[Prototype]]`
3. If not found, check the prototype's prototype
4. Continue until `null` (end of chain)

```javascript
const grandparent = { a: 1 };
const parent = Object.create(grandparent);
parent.b = 2;
const child = Object.create(parent);
child.c = 3;

child.c;                 // 3 — own
child.b;                 // 2 — from parent
child.a;                 // 1 — from grandparent
child.d;                 // undefined — not found anywhere
```

Checking the prototype:

```javascript
Object.getPrototypeOf(child);   // parent
Object.getPrototypeOf(parent);  // grandparent
Object.getPrototypeOf({});      // Object.prototype
Object.getPrototypeOf(Object.prototype); // null
```

### Constructor Functions

Before `class` syntax, constructor functions created objects with shared methods:

```javascript
function User(name, email) {
    // this = new object
    this.name = name;
    this.email = email;
    // return this (implicit)
}

// Methods go on the prototype — shared across all instances
User.prototype.greet = function() {
    return `Hello, I'm ${this.name}`;
};

User.prototype.changeEmail = function(email) {
    this.email = email;
};

const alice = new User("Alice", "alice@example.com");
const bob = new User("Bob", "bob@example.com");

alice.greet();           // "Hello, I'm Alice"
bob.greet();             // "Hello, I'm Bob"
alice.greet === bob.greet; // true — same function
```

The `new` keyword does:
1. Creates a new empty object
2. Links the object's prototype to the constructor's `prototype` property
3. Binds `this` to the new object
4. Returns the new object (unless the constructor returns an object)

```javascript
// Manual new
function fakeNew(Constructor, ...args) {
    const obj = Object.create(Constructor.prototype);
    const result = Constructor.apply(obj, args);
    return result instanceof Object ? result : obj;
}
```

### The `class` Syntax

ES6 classes are syntactic sugar over prototypes:

```javascript
class User {
    constructor(name, email) {
        this.name = name;
        this.email = email;
    }

    greet() {
        return `Hello, I'm ${this.name}`;
    }

    changeEmail(email) {
        this.email = email;
    }
}

const alice = new User("Alice", "alice@example.com");
alice.greet();           // "Hello, I'm Alice"
```

Classes are still functions with prototypes:

```javascript
typeof User;             // "function"
User.prototype.greet;    // the greet method
alice instanceof User;   // true
```

Class differences from constructor functions:
- Must be called with `new` (TypeError otherwise)
- Methods are non-enumerable
- Strict mode is implied
- Hoisting works differently (not hoisted like functions)

**Class fields** — declare instance properties in the class body:

```javascript
class Counter {
    count = 0;           // instance field — set on every new instance

    increment() {
        this.count++;
    }
}
```

### Static Methods & Properties

Static members belong to the class, not instances:

```javascript
class User {
    constructor(name) {
        this.name = name;
    }

    greet() {
        return `Hello, I'm ${this.name}`;
    }

    static createAnonymous() {
        return new User("Anonymous");
    }

    static role = "user";    // static property (ES2022+)
}

const guest = User.createAnonymous();
guest.greet();           // "Hello, I'm Anonymous"

User.role;               // "user"
guest.role;              // undefined

// Common use cases:
// - Factory methods
// - Utility functions
// - Caching instances
// - Configuration defaults
```

### Private Fields

Private fields (`#`) are truly private — enforced by the language:

```javascript
class BankAccount {
    #balance = 0;        // private field

    constructor(initial) {
        this.#balance = initial;
    }

    deposit(amount) {
        if (amount <= 0) throw new Error("Invalid amount");
        this.#balance += amount;
    }

    getBalance() {
        return this.#balance;
    }
}

const account = new BankAccount(1000);
account.getBalance();    // 1000
account.#balance;        // SyntaxError — private
```

Private methods are also supported:

```javascript
class Logger {
    #format(level, message) {
        return `[${level}] ${new Date().toISOString()}: ${message}`;
    }

    log(message) {
        console.log(this.#format("INFO", message));
    }

    error(message) {
        console.error(this.#format("ERROR", message));
    }
}
```

### Inheritance with `extends`

Class-based inheritance:

```javascript
class Animal {
    constructor(name) {
        this.name = name;
    }

    speak() {
        return `${this.name} makes a sound`;
    }
}

class Dog extends Animal {
    constructor(name, breed) {
        super(name);         // call parent constructor
        this.breed = breed;
    }

    speak() {
        return `${this.name} barks`;
    }

    fetch() {
        return `${this.name} fetches the ball`;
    }
}

const dog = new Dog("Rex", "Labrador");
dog.speak();             // "Rex barks"
dog.fetch();             // "Rex fetches the ball"
dog.name;                // "Rex" — inherited
```

`super` keywords:
- `super()` — calls the parent constructor
- `super.method()` — calls the parent method

```javascript
class Child extends Parent {
    constructor() {
        super();
        this.ownProp = "value";
    }

    overriddenMethod() {
        const parentResult = super.overriddenMethod();
        return `${parentResult} + child logic`;
    }
}
```

Inheritance vs. composition:

```javascript
// Inheritance — "is-a"
class Button extends Component { /* ... */ }

// Composition — "has-a"
class Button {
    constructor(label, onClick) {
        this.label = label;
        this.onClick = onClick;
    }

    render() {
        return `<button>${this.label}</button>`;
    }
}
```

### Mixin Pattern

Mixins combine behaviour from multiple sources:

```javascript
const TimestampMixin = {
    createdAt: new Date(),

    getAge() {
        return Date.now() - this.createdAt.getTime();
    }
};

const SerializableMixin = {
    toJSON() {
        return JSON.stringify(this);
    }
};

class User {
    constructor(name) {
        this.name = name;
    }
}

// Apply mixins
Object.assign(User.prototype, TimestampMixin, SerializableMixin);

const user = new User("Alice");
user.getAge();           // some number
user.toJSON();           // '{"name":"Alice","createdAt":"..."}'
```

A `withMixins` helper:

```javascript
function withMixins(Base, ...mixins) {
    mixins.forEach(mixin => {
        Object.assign(Base.prototype, mixin);
    });
    return Base;
}
```

### Object Utilities

Static methods on `Object` are essential for working with objects:

**Enumeration:**

```javascript
const obj = { a: 1, b: 2, c: 3 };

Object.keys(obj);        // ["a", "b", "c"] — own enumerable string keys
Object.values(obj);      // [1, 2, 3]
Object.entries(obj);     // [["a", 1], ["b", 2], ["c", 3]]

for (const [key, value] of Object.entries(obj)) {
    console.log(key, value);
}
```

**Merging and copying:**

```javascript
const target = { a: 1 };
const source1 = { b: 2 };
const source2 = { c: 3 };

Object.assign(target, source1, source2);
// target: { a: 1, b: 2, c: 3 }

// Shallow clone
const clone = Object.assign({}, source1);

// Spread operator (ES2018)
const merged = { ...source1, ...source2 };
```

**Freezing and sealing:**

```javascript
const obj = { name: "Alice" };

Object.freeze(obj);      // cannot add, delete, or change properties
obj.name = "Bob";        // fails silently
obj.age = 30;            // fails silently

Object.seal(obj);        // can change existing properties, cannot add/delete

Object.isFrozen(obj);    // true
Object.isSealed(obj);    // true
```

**fromEntries** — inverse of `entries`:

```javascript
const entries = [["a", 1], ["b", 2]];
const obj = Object.fromEntries(entries);
// { a: 1, b: 2 }

// Transform objects via entries
const doubled = Object.fromEntries(
    Object.entries({ a: 1, b: 2 }).map(([k, v]) => [k, v * 2])
);
// { a: 2, b: 4 }
```

**hasOwn** (ES2022+) — safe own-property check:

```javascript
Object.hasOwn(obj, "name");   // true — modern version
Object.hasOwn(obj, "toString"); // false — inherited
```

## Glossary

| Term | Definition |
|------|------------|
| Class | Syntactic sugar over constructor functions and prototypes |
| Computed property | A property key defined by an expression in brackets |
| Constructor | A function called with `new` to create instances |
| Descriptor | An object that configures a property's behaviour |
| Encapsulation | Bundling data and methods that operate on it |
| Enumeration | Iterating over an object's properties |
| Freeze | Make an object immutable |
| Getter | A method accessed as a property that computes a value |
| Inheritance | A mechanism for one object to access another's properties |
| Instance | An object created by a constructor or class |
| Mixin | A collection of methods added to a class's prototype |
| Prototype | The internal link from an object to its parent |
| Proxy | An object that wraps another to intercept operations |
| Setter | A method accessed as a property that validates and assigns |
| Static | Belonging to the class, not instances |

## Quick References

- [MDN: Objects](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Working_with_Objects) — working with objects guide
- [MDN: Classes](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes) — class reference
- [MDN: Prototype Chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain) — inheritance guide
- [MDN: Proxy](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Proxy) — proxy reference
- [You Don't Know JS: Objects & Classes](https://github.com/getify/You-Dont-Know-JS/tree/2nd-ed/objects-classes) — in-depth book

## Next Steps

- [Arrays & Collections](arrays-and-collections.md) — array methods, Map, Set
- [ES6+ Features](es6-features.md) — modern JavaScript syntax
- [DOM Manipulation](dom-manipulation.md) — interacting with the page
