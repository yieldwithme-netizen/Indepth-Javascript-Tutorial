# What is `hasOwnProperty`

## Definition

`hasOwnProperty()` checks if an object has a property **directly on itself** (not inherited from the prototype chain).

## Basic Usage

```javascript
const person = {
    name: "John",
    age: 30
};

console.log(person.hasOwnProperty("name"));  // true
console.log(person.hasOwnProperty("age"));   // true
console.log(person.hasOwnProperty("toString")); // false (inherited)
```

## Object.hasOwn() — Modern Alternative

```javascript
const obj = { key: "value" };

// ❌ Old way (still works but can be overridden)
console.log(obj.hasOwnProperty("key")); // true

// ✅ Modern way (ES2022+)
console.log(Object.hasOwn(obj, "key")); // true
console.log(Object.hasOwn(obj, "toString")); // false
```

## Checking Inherited Properties

```javascript
const animal = { type: "Animal" };
const dog = Object.create(animal);
dog.name = "Rex";

console.log(dog.hasOwnProperty("name"));   // true (own)
console.log(dog.hasOwnProperty("type"));   // false (inherited)
console.log("type" in dog);                // true (checks chain)
```

## With Property Descriptors

```javascript
const obj = {};

Object.defineProperty(obj, "hidden", {
    value: 42,
    enumerable: false,
    configurable: false
});

console.log(obj.hasOwnProperty("hidden")); // true
console.log(obj.hidden);                   // 42

for (let key in obj) {
    console.log(key); // nothing (not enumerable)
}
```

## Handling Edge Cases

```javascript
// Object with null prototype
const bare = Object.create(null);
bare.key = "value";

console.log(bare.hasOwnProperty("key")); // TypeError!
console.log(Object.hasOwn(bare, "key")); // true ✅

// Safely check any object
function safeHasOwn(obj, prop) {
    return Object.hasOwn(obj, prop);
}

console.log(safeHasOwn(bare, "key")); // true
```

## Common Use Cases

- **Filtering properties** — distinguish own vs inherited
- **Safe iteration** — avoid prototype pollution
- **Property validation** — check if property exists on object
- **Dictionary objects** — `Object.create(null)` with safe checks

## Common Mistakes

```javascript
// ❌ Using `in` operator (checks prototype chain)
const obj = { a: 1 };
console.log("toString" in obj); // true (inherited!)

// ✅ Use hasOwnProperty or Object.hasOwn
console.log(obj.hasOwnProperty("toString")); // false
console.log(Object.hasOwn(obj, "toString")); // false

// ❌ Calling on non-objects
// hasOwnProperty.call(null, "key"); // TypeError

// ✅ Use Object.hasOwn (handles any value)
console.log(Object.hasOwn(null, "key")); // false
console.log(Object.hasOwn(undefined, "key")); // false
```

## Quick Revision

- `hasOwnProperty()` — checks own properties only
- `Object.hasOwn()` — modern, safer alternative (ES2022)
- `in` operator — checks prototype chain too
- Use when iterating to avoid inherited properties
- `Object.hasOwn()` works with any value type

---

## Related Topics

- [[What-is-Prototype]] — Prototypes overview
- [[Check-Properties]] — Checking properties
- [[What-is-PrototypeChain]] — Prototype chain
- [[What-is-ObjectCreate]] — Object.create()
- [[Use-Proto]] — __proto__ accessor
