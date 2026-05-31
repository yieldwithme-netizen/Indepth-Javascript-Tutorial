# Properties in JavaScript

## Definition

**Properties** are key-value pairs that belong to objects. They can be data properties (holding values) or accessor properties (getter/setter functions). Properties are fundamental to how objects store and manage data in JavaScript.

---

## Basic Property Access

```javascript
const person = {
  name: "Alice",
  age: 30,
  "job title": "Developer" // Quoted for special characters
};

// Dot notation (preferred)
console.log(person.name); // "Alice"

// Bracket notation (required for special cases)
console.log(person["job title"]); // "Developer"
console.log(person["age"]); // 30

// Dynamic property access
const key = "name";
console.log(person[key]); // "Alice"

// Setting properties
person.email = "alice@example.com";
person["phone"] = "555-1234";
```

---

## Property Descriptors

```javascript
const car = { make: "Toyota", model: "Camry" };

// View property descriptor
console.log(Object.getOwnPropertyDescriptor(car, "make"));
// { value: "Toyota", writable: true, enumerable: true, configurable: true }

// Define a new property with descriptor
Object.defineProperty(car, "year", {
  value: 2024,
  writable: false, // Can't be changed
  enumerable: false, // Won't show in for...in
  configurable: false // Can't be deleted or redefined
});

car.year = 2025; // Silently fails (or throws in strict mode)
console.log(car.year); // 2024
```

---

## Property Flags

### Writable

```javascript
const obj = {};
Object.defineProperty(obj, "secret", {
  value: 42,
  writable: false
});

obj.secret = 100; // Silently fails
console.log(obj.secret); // 42

// Strict mode throws error
"use strict";
obj.secret = 100; // TypeError: Cannot assign to read only property
```

### Enumerable

```javascript
const obj = { a: 1, b: 2 };
Object.defineProperty(obj, "c", {
  value: 3,
  enumerable: false
});

console.log(obj); // { a: 1, b: 2 } - c not shown
console.log(obj.c); // 3 - still accessible

for (const key in obj) {
  console.log(key); // "a", "b" - c is skipped
}

Object.keys(obj); // ["a", "b"]
Object.values(obj); // [1, 2]
Object.entries(obj); // [["a", 1], ["b", 2]]
```

### Configurable

```javascript
const obj = {};
Object.defineProperty(obj, "x", {
  value: 1,
  configurable: false
});

delete obj.x; // Silently fails
Object.defineProperty(obj, "x", { enumerable: true }); // TypeError
```

---

## Getters and Setters

```javascript
const user = {
  firstName: "John",
  lastName: "Doe",
  
  // Getter - called when property is accessed
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
  
  // Setter - called when property is assigned
  set fullName(value) {
    const [first, last] = value.split(" ");
    this.firstName = first;
    this.lastName = last;
  }
};

console.log(user.fullName); // "John Doe" (getter)
user.fullName = "Jane Smith"; // (setter)
console.log(user.firstName); // "Jane"
console.log(user.lastName); // "Smith"
```

### Computed Properties

```javascript
const prefix = "user";
const obj = {
  [`${prefix}Name`]: "Alice",
  [`${prefix}Age`]: 30
};
// { userName: "Alice", userAge: 30 }

// Dynamic property names
function createObject(key1, key2) {
  return {
    [key1]: "value1",
    [key2]: "value2"
  };
}
```

---

## Property Iteration

```javascript
const obj = { a: 1, b: 2, c: 3 };

// for...in - iterates enumerable properties (including inherited)
for (const key in obj) {
  console.log(`${key}: ${obj[key]}`);
}

// Object.keys() - own enumerable properties
console.log(Object.keys(obj)); // ["a", "b", "c"]

// Object.values() - own enumerable values
console.log(Object.values(obj)); // [1, 2, 3]

// Object.entries() - own enumerable [key, value] pairs
console.log(Object.entries(obj)); // [["a", 1], ["b", 2], ["c", 3]]

// Object.getOwnPropertyNames() - all own properties
Object.defineProperty(obj, "hidden", { value: 42, enumerable: false });
console.log(Object.getOwnPropertyNames(obj)); // ["a", "b", "c", "hidden"]
```

---

## Common Use Cases

### Data Models

```javascript
class User {
  #id; // Private field
  #email;
  
  constructor(id, name, email) {
    this.#id = id;
    this.name = name;
    this.#email = email;
  }
  
  get email() {
    return this.#email;
  }
  
  set email(value) {
    if (!value.includes("@")) {
      throw new Error("Invalid email");
    }
    this.#email = value;
  }
  
  get profile() {
    return {
      id: this.#id,
      name: this.name,
      email: this.#email
    };
  }
}

const user = new User(1, "Alice", "alice@example.com");
console.log(user.profile); // { id: 1, name: "Alice", email: "alice@example.com" }
```

### Configuration Objects

```javascript
function createConfig(options = {}) {
  return {
    host: options.host || "localhost",
    port: options.port || 3000,
    debug: options.debug || false,
    get url() {
      return `http://${this.host}:${this.port}`;
    }
  };
}

const config = createConfig({ port: 8080 });
console.log(config.url); // "http://localhost:8080"
```

### Validation

```javascript
function createValidatedObject(schema) {
  return new Proxy({}, {
    set(target, prop, value) {
      if (schema[prop]) {
        if (!schema[prop](value)) {
          throw new TypeError(`Invalid value for ${prop}`);
        }
      }
      target[prop] = value;
      return true;
    }
  });
}

const user = createValidatedObject({
  name: v => typeof v === "string" && v.length > 0,
  age: v => typeof v === "number" && v > 0
});

user.name = "Alice"; // Works
user.age = 30; // Works
// user.age = -5; // TypeError: Invalid value for age
```

---

## Common Mistakes

### Mistake 1: Accessing Non-existent Properties

```javascript
const obj = { a: 1 };

console.log(obj.b); // undefined
console.log(obj.b.c); // TypeError: Cannot read properties of undefined

// Solution: optional chaining
console.log(obj?.b?.c); // undefined
```

### Mistake 2: Modifying Prototype Properties

```javascript
function Person(name) {
  this.name = name;
}
Person.prototype.species = "Human";

const alice = new Person("Alice");

// Wrong: modifies prototype for ALL instances
Person.prototype.species = "Robot";

// Correct: add own property
alice.species = "Human"; // Only affects alice
```

### Mistake 3: Checking Property Existence

```javascript
const obj = { a: 1 };

// Wrong: falsy values cause issues
if (obj.b) {} // false (undefined is falsy)

// Wrong: in operator checks prototype too
"toString" in obj; // true (inherited)

// Correct: use hasOwnProperty
obj.hasOwnProperty("a"); // true
obj.hasOwnProperty("toString"); // false

// Or use Object.hasOwn() (modern)
Object.hasOwn(obj, "a"); // true
```

### Mistake 4: Confusing Property and Element

```javascript
const arr = [1, 2, 3];

// Arrays have numeric properties
console.log(arr[0]); // 1
console.log(arr.length); // 3

// Can add properties to arrays
arr.customProp = "hello";
console.log(arr.customProp); // "hello"

// But don't do this - use objects instead
```

---

## Property Shorthand

```javascript
const name = "Alice";
const age = 30;

// ES6 shorthand
const person = { name, age };
// Equivalent to: { name: name, age: age }

// Method shorthand
const calculator = {
  value: 0,
  add(n) { // Shorthand for add: function(n)
    this.value += n;
    return this;
  },
  getResult() {
    return this.value;
  }
};
```

---

## Quick Revision Summary

| Operation | Syntax | Example |
|-----------|--------|---------|
| Access | `obj.prop` or `obj["prop"]` | `obj.name` |
| Set | `obj.prop = value` | `obj.name = "Bob"` |
| Delete | `delete obj.prop` | `delete obj.name` |
| Check | `"prop" in obj` | `"name" in obj` |
| Own | `obj.hasOwnProperty("prop")` | `obj.hasOwnProperty("name")` |
| Keys | `Object.keys(obj)` | `["name", "age"]` |
| Values | `Object.values(obj)` | `["Bob", 25]` |
| Entries | `Object.entries(obj)` | `[["name", "Bob"]]` |
| Freeze | `Object.freeze(obj)` | Immutable properties |

---

## Related Topics

- [[Object]] - Working with objects
- [[Getters-Setters]] - Accessor properties
- [[Object-Methods]] - Object method patterns
- [[Object-Destructuring]] - Destructuring properties
- [[this]] - Property access with `this`
- [[class]] - Class properties
- [[Symbol-Iterator]] - Symbol properties