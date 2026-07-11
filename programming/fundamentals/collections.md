# Collections

## Description

Collections let you group, organize, and manipulate multiple values as a single unit. Master arrays, dictionaries, sets, and tuples to write data-driven code that is both expressive and efficient.

## Prerequisites

- [Functions](../functions.md) — understanding function calls and returns needed for callbacks like map/filter/reduce
- [Error Handling](../error-handling.md) — handling index-out-of-range and key-not-found errors

## Table of Contents

- [Why Collections Exist](#why-collections-exist)
- [Arrays and Lists](#arrays-and-lists)
- [Objects, Dicts, and Maps](#objects-dicts-and-maps)
- [Sets](#sets)
- [Tuples (Python)](#tuples-python)
- [Mutability](#mutability)
- [Aliasing and Copying](#aliasing-and-copying)
- [Comprehensions (Python)](#comprehensions-python)
- [Destructuring](#destructuring)
- [Common Patterns](#common-patterns)
- [Glossary](#glossary)
- [Quick References](#quick-references)
- [Next Steps](#next-steps)

## Content / Material

### Why Collections Exist

A single variable holds one value. Real programs process hundreds, thousands, or millions of values. Collections provide the data structures to store, organise, and iterate over these groups with standardised operations — adding, removing, finding, sorting, transforming — that work regardless of the number of elements.

```python
users = ["Alice", "Bob", "Charlie"]
for user in users:
    print(user)
```

Choosing the right collection type (array, dict, set) affects performance, memory, and code clarity.

### Arrays and Lists

Arrays (called lists in Python, arrays in JavaScript) are ordered, indexed sequences of elements.

#### Creation and Indexing

```python
numbers = [1, 2, 3, 4, 5]
from_range = list(range(5))
```

```javascript
const numbers = [1, 2, 3, 4, 5];
const fromArray = Array.from({length: 5}, (_, i) => i);
```

Zero-based indexing. Python supports negative indices; JavaScript uses `.at(-1)` (ES2022).

```python
fruits = ["apple", "banana", "cherry"]
print(fruits[0])   # "apple"
print(fruits[-1])  # "cherry"
print(len(fruits)) # 3
```

```javascript
const fruits = ["apple", "banana", "cherry"];
console.log(fruits[0]);       // "apple"
console.log(fruits.at(-1));   // "cherry"
console.log(fruits.length);   // 3
```

#### Adding and Removing

```python
stack = [1, 2, 3]
stack.append(4)            # [1, 2, 3, 4]
last = stack.pop()         # 4
queue = [2, 3, 4]
queue.insert(0, 1)         # [1, 2, 3, 4]
items = [1, 2, 3, 2, 4]
items.remove(2)            # removes first 2: [1, 3, 2, 4]
del items[1]               # remove by index
```

```javascript
const stack = [1, 2, 3];
stack.push(4);             // [1, 2, 3, 4]
const last = stack.pop();  // 4
const queue = [2, 3, 4];
queue.unshift(1);          // [1, 2, 3, 4]
queue.shift();             // 1
const items = [1, 2, 4, 5];
items.splice(2, 0, 3);     // insert at index 2
items.splice(1, 1);        // remove at index 1
```

#### Slicing

Extracts a subsequence without modifying the original.

```python
nums = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
print(nums[2:5])     # [2, 3, 4]
print(nums[:4])      # [0, 1, 2, 3]
print(nums[6:])      # [6, 7, 8, 9]
print(nums[::2])     # [0, 2, 4, 6, 8]
print(nums[::-1])    # reversed copy

# slice assignment (like splice)
arr = [10, 20, 30, 40, 50]
arr[1:3] = [25, 35]  # arr is now [10, 25, 35, 40, 50]
```

```javascript
const nums = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
console.log(nums.slice(2, 5));    // [2, 3, 4]
console.log(nums.slice(6));       // [6, 7, 8, 9]
console.log(nums.slice().reverse());  // reversed copy

// splice mutates — removes and inserts in place
const arr = [10, 20, 30, 40, 50];
arr.splice(1, 2, 25, 35);  // removes [20, 30], inserts 25, 35
```

#### Iteration

```python
for item in items:
    print(item)

for i, item in enumerate(items):
    print(i, item)
```

```javascript
for (const item of items) {
    console.log(item);
}

items.forEach((item, index) => {
    console.log(index, item);
});
```

#### Transformation: Map, Filter, Reduce

**Map** transforms every element. **Filter** keeps elements satisfying a predicate. **Reduce** accumulates into a single value.

```python
numbers = [1, 2, 3, 4, 5]
squared = [x ** 2 for x in numbers]                 # map
evens = [x for x in numbers if x % 2 == 0]           # filter

from functools import reduce
total = reduce(lambda acc, x: acc + x, numbers, 0)   # reduce: 15
total = sum(numbers)                                  # specific case
```

```javascript
const numbers = [1, 2, 3, 4, 5];
const squared = numbers.map(x => x ** 2);             // map
const evens = numbers.filter(x => x % 2 === 0);        // filter
const total = numbers.reduce((acc, x) => acc + x, 0); // reduce: 15
```

Reduce is the most general — map and filter can be expressed with reduce, but dedicated functions are clearer.

#### Finding Elements

```python
items = ["a", "b", "c", "b"]
print(items.index("b"))        # 1 (first occurrence)
print("b" in items)            # True (membership)
found = next((x for x in items if x > 20), None)  # first match
print(any(x > 40 for x in items))   # True
print(all(x > 0 for x in items))    # True
```

```javascript
const items = ["a", "b", "c", "b"];
console.log(items.indexOf("b"));      // 1
console.log(items.includes("b"));     // true
console.log(items.find(x => x > 20)); // first match
console.log(items.some(x => x > 40)); // true
console.log(items.every(x => x > 0)); // true
```

#### Sorting and Reversing

```python
nums = [3, 1, 4, 1, 5]
nums.sort()                     # in-place
sorted_nums = sorted(nums)      # new sorted copy
words = ["banana", "apple", "cherry"]
words.sort(key=len)             # sort by length

nums.reverse()                  # in-place
reversed_nums = nums[::-1]      # reversed copy
```

```javascript
let nums = [3, 1, 4, 1, 5];
nums.sort((a, b) => a - b);   // numeric sort (default is lexicographic!)
const words = ["banana", "apple", "cherry"];
words.sort((a, b) => a.length - b.length);

nums.reverse();                // in-place
const reversed = [...nums].reverse();  // reversed copy
```

JavaScript sorts lexicographically by default — `[10, 2].sort()` gives `[10, 2]`. Always pass a compare function for numeric sort.

#### Flattening, Joining, Splitting

```python
flat = [item for sublist in [[1, 2], [3, 4]] for item in sublist]
date = "-".join(["2024", "12", "25"])
items = "a,b,c".split(",")
```

```javascript
const flat = [[1, 2], [3, 4]].flat();
const deep = [1, [2, [3, 4]]].flat(Infinity);
const date = ["2024", "12", "25"].join("-");
const items = "a,b,c".split(",");
```

#### Spread Operator

Unpacks elements for copying, merging, and function arguments.

```python
a = [1, 2, 3]
b = [4, 5, 6]
combined = [*a, *b]       # [1, 2, 3, 4, 5, 6]
```

```javascript
const a = [1, 2, 3];
const b = [4, 5, 6];
const combined = [...a, ...b];
const copy = [...a];
const config = { ...defaults, port: 8080 };  // object spread
```

### Objects, Dicts, and Maps

Dictionaries (Python) and objects (JavaScript) store key-value pairs. Lookup is O(1) average via hash tables.

```python
person = {"name": "Alice", "age": 30}
print(person["name"])            # "Alice"
print(person.get("age", 0))      # 30 (or default)
print("name" in person)          # True
print(person.keys(), person.values(), person.items())

person["email"] = "a@b.com"      # add or update
person.update({"age": 31})       # batch update
del person["email"]              # delete
person.pop("age")                # delete and return
```

```javascript
const person = { name: "Alice", age: 30 };
console.log(person.name);            // "Alice"
console.log(person?.salary ?? 0);    // 0
console.log(Object.keys(person));

person.email = "a@b.com";
Object.assign(person, { age: 31 });
delete person.email;
```

#### Map (JavaScript)

JavaScript's `Map` improves on plain objects: any key type, insertion order, `.size` property, no prototype pollution.

```javascript
const map = new Map();
map.set("name", "Alice");
map.set(42, "answer");
console.log(map.get("name"));  // "Alice"
console.log(map.size);         // 2
for (const [k, v] of map) console.log(k, v);
```

Python's dict has been insertion-ordered since Python 3.7:

```python
d = {"z": 1, "a": 2}
print(list(d.keys()))  # ["z", "a"]
```

#### Nesting and Deep Access

```python
company = {"depts": [{"employees": [{"name": "Alice", "skills": ["Python"]}]}]}
print(company["depts"][0]["employees"][0]["skills"])
```

```javascript
const company = { depts: [{ employees: [{ name: "Alice", skills: ["Python"] }] }] };
console.log(company?.depts?.[0]?.employees?.[0]?.skills);
```

#### Merging

```python
config = {**{"host": "localhost", "port": 3000}, **{"port": 8080, "debug": True}}
# Python 3.9+: config = defaults | overrides
```

```javascript
const config = { ...defaults, ...overrides };
```

### Sets

A set is an unordered collection of unique elements.

```python
numbers = {1, 2, 3, 2, 1}
numbers.add(4)
numbers.remove(2)        # KeyError if not found
numbers.discard(99)      # no error
```

```javascript
const numbers = new Set([1, 2, 3, 2, 1]);
numbers.add(4);
numbers.delete(2);       // true
numbers.has(99);         // false
```

#### Set Operations

```python
a = {1, 2, 3}
b = {3, 4, 5}
print(a | b)   # union:      {1, 2, 3, 4, 5}
print(a & b)   # intersect:  {3}
print(a - b)   # difference: {1, 2}
print(a ^ b)   # sym diff:   {1, 2, 4, 5}
print({1, 2}.issubset({1, 2, 3}))  # True
```

```javascript
const union = new Set([...a, ...b]);
const intersection = new Set([...a].filter(x => b.has(x)));
const difference = new Set([...a].filter(x => !b.has(x)));
```

Set membership is O(1) — prefer sets over lists for containment checks:

```python
blacklist = {"192.168.1.1", "10.0.0.1"}
if ip in blacklist: print("blocked")  # O(1)
```

### Tuples (Python)

A tuple is an immutable ordered sequence — useful for returning multiple values and as dict keys.

```python
point = (3, 4)
x, y = point                       # unpacking
def min_max(items):
    return min(items), max(items)
low, high = min_max([3, 1, 4, 1, 5])
locations = {(51.5, -0.12): "London"}  # tuple as dict key
```

JavaScript lacks a built-in tuple — arrays with destructuring simulate it:

```javascript
function minMax(items) { return [Math.min(...items), Math.max(...items)]; }
const [low, high] = minMax([3, 1, 4, 1, 5]);
```

TypeScript adds a tuple type: `let point: [number, number] = [3, 4];`.

### Mutability

**Mutable** (can change in place): Python list, dict, set — JavaScript array, object, Map, Set.

```python
items = [1, 2, 3]
items[0] = 99
items.append(4)
```

```javascript
const items = [1, 2, 3];  // const prevents reassignment, not mutation
items[0] = 99;
items.push(4);
```

**Immutable** (cannot change): Python tuple, frozenset — strings. Immutability makes values safe as dict keys and easier to reason about.

### Aliasing and Copying

#### Reference Semantics

Assignment creates an alias, not a copy:

```python
a = [1, 2, 3]
b = a                 # alias
b.append(4)
print(a)              # [1, 2, 3, 4] — a changed too!
print(a is b)         # True
```

```javascript
const a = [1, 2, 3];
const b = a;          // alias
b.push(4);
console.log(a);       // [1, 2, 3, 4]
console.log(a === b); // true
```

#### Shallow Copy

A new collection, but nested elements are still shared references.

```python
original = [[1, 2], [3, 4]]
copy = list(original)
copy[0].append(99)
print(original)   # [[1, 2, 99], [3, 4]] — inner list shared!
```

```javascript
const original = [[1, 2], [3, 4]];
const copy = [...original];
copy[0].push(99);
console.log(original);  // [[1, 2, 99], [3, 4]] — inner array shared!
```

#### Deep Copy

Duplicates everything recursively — no shared references.

```python
import copy
original = [[1, 2], [3, 4]]
deep = copy.deepcopy(original)
deep[0].append(99)
print(original)  # [[1, 2], [3, 4]] — unchanged
```

```javascript
const original = [[1, 2], [3, 4]];
const deep = structuredClone(original);         // ES2023
// Alternatives: JSON.parse(JSON.stringify(obj)) but limited
```

### Comprehensions (Python)

Concise syntax for building collections from iterables.

```python
# List: [expr for item in iterable if condition]
squares = [x ** 2 for x in range(10)]
evens = [x for x in range(20) if x % 2 == 0]
flat = [item for row in matrix for item in row]

# Dict: {key_expr: val_expr for item in iterable}
square_dict = {x: x ** 2 for x in range(5)}
person = {k: v for k, v in zip(["name", "age"], ["Alice", 30])}
filtered = {k: v * 2 for k, v in {"a": 1, "b": 2}.items() if v > 1}
swapped = {v: k for k, v in {"a": 1, "b": 2}.items()}

# Set: {expr for item in iterable}
unique_lengths = {len(w) for w in ["hello", "world", "hi"]}
```

JavaScript uses `map`/`filter` instead of comprehension syntax:

```javascript
const squares = Array.from({ length: 10 }, (_, i) => i ** 2);
const evens = Array.from({ length: 20 }, (_, i) => i).filter(x => x % 2 === 0);
```

### Destructuring

Unpacking elements from collections into individual variables.

#### Array Destructuring

```python
point = (3, 4)
x, y = point

head, *rest = [1, 2, 3, 4, 5]
print(head, rest)            # 1 [2, 3, 4, 5]

first, _, third, *tail = [10, 20, 30, 40, 50]
print(first, third, tail)    # 10 30 [40, 50]

a, b = 1, 2
a, b = b, a                  # swap
```

```javascript
const point = [3, 4];
const [x, y] = point;

const [head, ...rest] = [1, 2, 3, 4, 5];
console.log(head, rest);          // 1 [2, 3, 4, 5]

const [first, , third] = [10, 20, 30, 40];
console.log(first, third);        // 10 30

const [a = 0, b = 0] = [1];
console.log(a, b);                // 1 0
```

#### Object Destructuring

```python
# Python uses ** for unpacking in function calls
def greet(**kwargs):
    print(kwargs.get("name"))

person = {"name": "Alice", "age": 30}
greet(**person)  # "Alice"
```

```javascript
const person = { name: "Alice", age: 30, city: "London" };
const { name, age } = person;
const { name: userName } = person;         // renaming
const { salary = 50000 } = person;         // default value
const { address: { city } } = { address: { city: "London" } };  // nested
const { name, ...rest } = person;          // rest
```

### Common Patterns

#### Group-By

```python
from collections import defaultdict
users = [{"name": "Alice", "role": "admin"}, {"name": "Bob", "role": "user"}]
groups = defaultdict(list)
for user in users:
    groups[user["role"]].append(user)
```

```javascript
const users = [{ name: "Alice", role: "admin" }, { name: "Bob", role: "user" }];
const groups = Object.groupBy(users, user => user.role);

// Manual via reduce:
const manual = users.reduce((acc, u) => ((acc[u.role] ||= []).push(u), acc), {});
```

#### Counting Frequencies

```python
from collections import Counter
items = ["apple", "banana", "apple", "orange", "banana", "apple"]
counts = Counter(items)
print(counts.most_common(2))  # [("apple", 3), ("banana", 2)]

# Manual
counts = {}
for item in items:
    counts[item] = counts.get(item, 0) + 1
```

```javascript
const items = ["apple", "banana", "apple", "orange", "banana", "apple"];
const counts = items.reduce((acc, item) => {
    acc[item] = (acc[item] || 0) + 1;
    return acc;
}, {});
// { apple: 3, banana: 2, orange: 1 }
```

#### Deduplication

```python
items = [1, 2, 2, 3, 3, 3, 4]
unique = list(set(items))                    # order NOT preserved
unique_ordered = list(dict.fromkeys(items))  # preserves order (3.7+)
```

```javascript
const items = [1, 2, 2, 3, 3, 3, 4];
const unique = [...new Set(items)];  // preserves insertion order
```

#### Chaining Map / Filter / Reduce

```python
orders = [{"price": 10, "qty": 3}, {"price": 25, "qty": 1}, {"price": 5, "qty": 10}]
total = sum(o["price"] * o["qty"] for o in orders if o["qty"] > 0)
print(total)  # 105
```

```javascript
const orders = [{ price: 10, qty: 3 }, { price: 25, qty: 1 }, { price: 5, qty: 10 }];
const total = orders.filter(o => o.qty > 0).map(o => o.price * o.qty).reduce((a, v) => a + v, 0);
console.log(total);  // 105
```

## Glossary

| Term | Definition |
|------|------------|
| Collection | A data structure that holds multiple values |
| Array | An ordered, indexed sequence of elements |
| List | Python's term for a mutable ordered sequence |
| Tuple | An immutable ordered sequence (Python) |
| Dictionary | A key-value mapping (Python) |
| Map | A key-value collection with arbitrary key types (JavaScript) |
| Set | An unordered collection of unique elements |
| Key | A unique identifier used to look up a value |
| Value | The data associated with a key in a key-value pair |
| Index | A numeric position identifying an element in a sequence |
| Slice | A contiguous subsequence extracted from a sequence |
| Shallow copy | A copy where nested objects remain shared references |
| Deep copy | A recursive copy where no references are shared |
| Alias | An alternative name referencing the same object |
| Reference | A value that points to an object in memory |
| Mutation | Changing the contents of a collection in place |
| Iteration | Processing each element of a collection in turn |
| Enumeration | Iterating with both index and value |
| Comprehension | Concise syntax for creating collections from iterables (Python) |
| Destructuring | Unpacking elements from a collection into variables |
| Spread | Operator that unpacks elements from an iterable |
| Rest | Syntax that collects remaining elements into a new collection |
| Reduce | Higher-order function accumulating elements into a single value |
| Filter | Higher-order function selecting elements by a predicate |
| Map | Higher-order function transforming every element |
| Predicate | A function returning a boolean |
| Callback | A function passed as an argument to another function |
| Accumulator | The intermediate value carried through a reduction |
| Hash table | Data structure enabling O(1) key lookup |
| Bucket | A slot in a hash table for colliding keys |
| Collision | When two different keys hash to the same bucket |
| Immutable | Cannot be changed after creation |

## Quick References

- [MDN Array](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array) — comprehensive JavaScript array method reference with examples
- [Python Data Structures (Official Docs)](https://docs.python.org/3/tutorial/datastructures.html) — Python official tutorial on lists, tuples, dicts, and sets
- [Python collections Module](https://docs.python.org/3/library/collections.html) — defaultdict, Counter, deque, and other specialised collections
- [MDN Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map) — JavaScript Map reference with comparisons to plain objects
- [MDN Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set) — JavaScript Set reference and set operation patterns

## Next Steps

- Back to [Programming Fundamentals](intro/programming-fundamentals.md) to review
- [Error Handling](../error-handling.md) — catching errors when working with collections
- [Functions](../functions.md) — understanding callbacks in map/filter/reduce