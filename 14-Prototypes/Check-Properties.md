# How to Check Properties

## Definition

JavaScript provides multiple ways to check for properties on objects, each with different behavior regarding the prototype chain.

## Property Checking Methods

```javascript
const car = { make: "Toyota", model: "Camry" };

// 1. hasOwnProperty — own properties only
console.log(car.hasOwnProperty("make"));  // true
console.log(car.hasOwnProperty("drive")); // false

// 2. Object.hasOwn — modern, safer (ES2022)
console.log(Object.hasOwn(car, "make"));  // true

// 3. `in` operator — checks prototype chain
console.log("make" in car);   // true
console.log("toString" in car); // true (inherited)

// 4. Direct property access
console.log(car.make);        // "Toyota"
console.log(car.drive);       // undefined
```

## Comparing Methods

```javascript
const animal = { type: "Animal" };
const dog = Object.create(animal);
dog.name = "Rex";

// Check "type" (inherited)
console.log(dog.hasOwnProperty("type")); // false (own only)
console.log(Object.hasOwn(dog, "type")); // false (own only)
console.log("type" in dog);              // true (chain)
console.log(dog.type !== undefined);     // true (chain)

// Check "name" (own)
console.log(dog.hasOwnProperty("name")); // true
console.log(Object.hasOwn(dog, "name")); // true
console.log("name" in dog);              // true
console.log(dog.name !== undefined);     // true
```

## Checking with `!== undefined`

```javascript
const obj = { a: 1, b: undefined };

console.log(obj.a !== undefined); // true
console.log(obj.b !== undefined); // false (value IS undefined!)
console.log(obj.c !== undefined); // false (doesn't exist)

// ❌ Unreliable for properties that may be undefined
console.log(obj.hasOwnProperty("b")); // true
```

## Checking Multiple Properties

```javascript
function hasAllProperties(obj, ...props) {
    return props.every(prop => Object.hasOwn(obj, prop));
}

const user = { name: "John", email: "john@example.com" };

console.log(hasAllProperties(user, "name", "email")); // true
console.log(hasAllProperties(user, "name", "age"));   // false
```

## Safe Property Access

```javascript
const config = {
    database: {
        host: "localhost",
        port: 5432
    }
};

// ❌ Unsafe — may throw if nested property missing
// console.log(config.database.host);

// ✅ Safe optional chaining
console.log(config?.database?.host);  // "localhost"
console.log(config?.cache?.ttl);      // undefined (no error)

// ✅ Safe with hasOwnProperty
if (config.hasOwnProperty("database")) {
    console.log(config.database.host);
}
```

## Iterating Own Properties

```javascript
const obj = { a: 1, b: 2, c: 3 };

// for...in (includes inherited — use hasOwnProperty to filter)
for (let key in obj) {
    if (Object.hasOwn(obj, key)) {
        console.log(key, obj[key]);
    }
}

// Object.keys() — own enumerable properties only
console.log(Object.keys(obj)); // ["a", "b", "c"]

// Object.entries() — key-value pairs
console.log(Object.entries(obj)); // [["a", 1], ["b", 2], ["c", 3]]
```

## Common Use Cases

- **Form validation** — check required fields exist
- **API responses** — verify expected properties
- **Configuration** — check for optional settings
- **Debugging** — inspect object structure

## Common Mistakes

```javascript
// ❌ Relying on `in` for own properties
const data = { id: 1 };
console.log("id" in data);              // true
console.log("toString" in data);        // true (inherited!)

// ❌ Checking truthiness for property existence
const obj = { count: 0, name: "" };
if (obj.count) { }   // false! (0 is falsy)
if (obj.name) { }    // false! ("" is falsy)

// ✅ Use Object.hasOwn or typeof
if (Object.hasOwn(obj, "count")) { } // true
if (typeof obj.name === "string") { } // true
```

## Quick Revision

- `hasOwnProperty()` — own properties only
- `Object.hasOwn()` — modern, safer (ES2022)
- `in` operator — checks prototype chain
- `!== undefined` — unreliable for undefined values
- `Object.keys()` — list own enumerable properties
- Use optional chaining for safe nested access

---

## Related Topics

- [[What-is-HasOwn]] — hasOwnProperty
- [[What-is-PrototypeChain]] — Prototype chain
- [[What-is-Prototype]] — Prototypes overview
- [[What-is-ObjectCreate]] — Object.create()
- [[What-is-InstanceOf]] — instanceof operator
