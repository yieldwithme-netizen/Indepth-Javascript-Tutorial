# Objects in JavaScript

## Definition

Objects are collections of key-value pairs where keys are strings (or Symbols) and values can be any data type. Objects are the fundamental building blocks of JavaScript and are used to represent complex entities, organize data, and implement abstractions.

---

## Creating Objects

```javascript
// Object literal (most common)
const obj1 = {
  name: "Alice",
  age: 25,
  greet() {
    console.log(`Hello, I'm ${this.name}`);
  }
};

// Constructor function
function Person(name, age) {
  this.name = name;
  this.age = age;
}
const person1 = new Person("Alice", 25);

// Object.create
const proto = { greet() { return `Hi, ${this.name}`; } };
const obj2 = Object.create(proto);
obj2.name = "Bob";

// Class (ES6)
class Animal {
  constructor(name) {
    this.name = name;
  }
  speak() {
    return `${this.name} makes a sound`;
  }
}
const dog = new Animal("Rex");

// Object.assign
const obj3 = Object.assign({}, { a: 1 }, { b: 2 });

// Spread operator
const obj4 = { ...{ a: 1 }, ...{ b: 2 } };
```

---

## Property Access

### Dot vs Bracket Notation

```javascript
const person = { name: "Alice", age: 25 };

// Dot notation
console.log(person.name); // "Alice"

// Bracket notation (required for dynamic keys)
const key = "age";
console.log(person[key]); // 25

// Bracket with computed property
const prop = "name";
console.log(person[prop]); // "Alice"

// Bracket for special characters
const obj = { "my-key": 1, "123": 2 };
console.log(obj["my-key"]); // 1
console.log(obj[123]); // 2 (auto-converted to string)
```

### Optional Chaining

```javascript
const user = {
  name: "Alice",
  address: {
    city: "New York"
  }
};

// Without optional chaining
const zip = user.address && user.address.zip; // undefined

// With optional chaining
const zip2 = user.address?.zip; // undefined (no error)
const street = user.address?.street; // undefined

// With methods
const result = user.getName?.(); // undefined if method doesn't exist
```

---

## Property Descriptors

```javascript
const obj = {};

// Define property with descriptor
Object.defineProperty(obj, "name", {
  value: "Alice",
  writable: false,    // Cannot be changed
  enumerable: true,   // Shows in for...in
  configurable: false // Cannot be deleted or reconfigured
});

obj.name = "Bob"; // Silently fails (or throws in strict mode)
console.log(obj.name); // "Alice"

// Get all descriptors
console.log(Object.getOwnPropertyDescriptors(obj));
```

### Getters and Setters

```javascript
const person = {
  firstName: "Alice",
  lastName: "Smith",

  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },

  set fullName(value) {
    const [first, last] = value.split(" ");
    this.firstName = first;
    this.lastName = last;
  }
};

console.log(person.fullName); // "Alice Smith" (getter)
person.fullName = "Bob Jones"; // Setter
console.log(person.firstName); // "Bob"
```

---

## Object Methods

### Object.keys(), values(), entries()

```javascript
const obj = { a: 1, b: 2, c: 3 };

// Keys as array
console.log(Object.keys(obj)); // ["a", "b", "c"]

// Values as array
console.log(Object.values(obj)); // [1, 2, 3]

// Entries as array of [key, value] pairs
console.log(Object.entries(obj)); // [["a", 1], ["b", 2], ["c", 3]]

// Iterate with entries
for (const [key, val] of Object.entries(obj)) {
  console.log(`${key}: ${val}`);
}
```

### Object.freeze() and Object.seal()

```javascript
const obj = { a: 1, b: 2 };

// Freeze: cannot add, modify, or delete properties
Object.freeze(obj);
obj.a = 99; // Silently fails
obj.c = 3; // Silently fails
delete obj.a; // Silently fails

// Seal: cannot add or delete, but can modify
const obj2 = { a: 1, b: 2 };
Object.seal(obj2);
obj2.a = 99; // Works!
obj2.c = 3; // Silently fails
delete obj2.a; // Silently fails
```

### Object.fromEntries()

```javascript
// Convert entries back to object
const entries = [["a", 1], ["b", 2], ["c", 3]];
const obj = Object.fromEntries(entries);
console.log(obj); // { a: 1, b: 2, c: 3 }

// Useful with Map
const map = new Map([["a", 1], ["b", 2]]);
const obj2 = Object.fromEntries(map);

// Transform objects
const transformed = Object.fromEntries(
  Object.entries({ a: 1, b: 2, c: 3 })
    .map(([key, val]) => [key, val * 2])
);
// { a: 2, b: 4, c: 6 }
```

---

## Common Use Cases

### Data Transformation

```javascript
const users = [
  { id: 1, name: "Alice", role: "admin" },
  { id: 2, name: "Bob", role: "user" },
  { id: 3, name: "Charlie", role: "admin" }
];

// Group by role
const grouped = users.reduce((acc, user) => {
  if (!acc[user.role]) acc[user.role] = [];
  acc[user.role].push(user);
  return acc;
}, {});

// Create lookup map
const userMap = Object.fromEntries(
  users.map(user => [user.id, user])
);
console.log(userMap[1]); // { id: 1, name: "Alice", role: "admin" }
```

### Merging Objects

```javascript
const defaults = { color: "red", size: "medium", enabled: true };
const custom = { color: "blue", size: "large" };

// Spread (shallow merge)
const result1 = { ...defaults, ...custom };
// { color: "blue", size: "large", enabled: true }

// Deep merge
function deepMerge(target, source) {
  const result = { ...target };
  for (const key of Object.keys(source)) {
    if (source[key] instanceof Object && key in target) {
      result[key] = deepMerge(target[key], source[key]);
    } else {
      result[key] = source[key];
    }
  }
  return result;
}

const a = { nested: { x: 1, y: 2 }, z: 3 };
const b = { nested: { y: 99, w: 100 } };
console.log(deepMerge(a, b));
// { nested: { x: 1, y: 99, w: 100 }, z: 3 }
```

### Cloning Objects

```javascript
const original = { a: 1, nested: { b: 2 } };

// Shallow copy
const shallow1 = { ...original };
const shallow2 = Object.assign({}, original);
const shallow3 = Object.fromEntries(Object.entries(original));

// Deep copy
const deep1 = structuredClone(original);
const deep2 = JSON.parse(JSON.stringify(original)); // Has limitations

// Modify deep copy without affecting original
deep1.nested.b = 99;
console.log(original.nested.b); // 2 (unchanged)
```

### Memoization

```javascript
function memoize(fn) {
  const cache = new Map();

  return function(...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      return cache.get(key);
    }

    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

const expensiveCalc = memoize((n) => {
  console.log("Computing...");
  return n * n;
});

expensiveCalc(4); // Computing... 16
expensiveCalc(4); // 16 (cached)
```

---

## Common Mistakes

### Mistake 1: Modifying Frozen Objects

```javascript
const obj = Object.freeze({ a: 1 });
obj.a = 99; // Silently fails
console.log(obj.a); // 1

// Use strict mode to catch errors
"use strict";
const obj2 = Object.freeze({ a: 1 });
// obj2.a = 99; // TypeError
```

### Mistake 2: Shallow Comparison

```javascript
const a = { x: 1 };
const b = { x: 1 };

console.log(a === b); // false (different references)

// Deep comparison
function deepEqual(x, y) {
  if (x === y) return true;
  if (typeof x !== "object" || typeof y !== "object") return false;
  const keysX = Object.keys(x);
  const keysY = Object.keys(y);
  if (keysX.length !== keysY.length) return false;
  return keysX.every(key => key in y && deepEqual(x[key], y[key]));
}

console.log(deepEqual(a, b)); // true
```

### Mistake 3: Iterating Prototype Properties

```javascript
function Person(name) {
  this.name = name;
}
Person.prototype.greet = function() { return `Hi, ${this.name}`; };

const p = new Person("Alice");

// Wrong: includes prototype methods
for (const key in p) {
  console.log(key); // "name", "greet"
}

// Correct: use hasOwnProperty check
for (const key in p) {
  if (p.hasOwnProperty(key)) {
    console.log(key); // "name" only
  }
}

// Better: use Object.keys
console.log(Object.keys(p)); // ["name"]
```

### Mistake 4: Destructuring Non-Existent Properties

```javascript
const obj = { a: 1 };

// Gives undefined without error
const { b } = obj;
console.log(b); // undefined

// Default values help
const { c = "default" } = obj;
console.log(c); // "default"

// Optional chaining in destructuring
const { d: { e } = {} } = obj;
// TypeError: Cannot destructure property 'e' of undefined

const { d: { e } = {} } = obj || {};
console.log(e); // undefined
```

---

## Quick Revision Summary

| Operation | Syntax | Description |
|-----------|--------|-------------|
| Create | `{}` or `Object.create()` | Create object |
| Access | `obj.key` or `obj[key]` | Read property |
| Add/Update | `obj.key = value` | Set property |
| Delete | `delete obj.key` | Remove property |
| Check | `"key" in obj` | Check existence |
| Keys | `Object.keys(obj)` | Get all keys |
| Values | `Object.values(obj)` | Get all values |
| Entries | `Object.entries(obj)` | Get [key, value] pairs |
| Freeze | `Object.freeze(obj)` | Make immutable |
| Clone | `{ ...obj }` | Shallow copy |

---

## Related Topics

- [[this]] - Object method context
- [[Array]] - Arrays are objects
- [[Symbol-Iterator]] - Custom iteration for objects
- [[Promise]] - Async object patterns
