# What is Optional Chaining?

## Definition

Optional chaining (`?.`) **short-circuits** if the value is `null` or `undefined`.

## Basic Usage

```javascript
const user = { name: "John", address: { city: "NYC" } };

// Without optional chaining
const city1 = user && user.address && user.address.city;

// With optional chaining
const city2 = user?.address?.city; // "NYC"

// Non-existent property
const zip = user?.address?.zip; // undefined (no error!)
```

## Methods and Functions

```javascript
const user = { name: "John" };

// Method call
const result = user.getName?.(); // undefined (no error!)

// Function call
const result = greet?.("John"); // undefined (no error!)
```

## Arrays

```javascript
const arr = [1, 2, 3];

const first = arr?.[0]; // 1
const tenth = arr?.[9]; // undefined (no error!)
```

## Quick Revision

- `?.` checks for null/undefined
- Short-circuits if null/undefined
- Works with: properties, methods, arrays
- Replaces verbose null checks
- Returns undefined if chain breaks

---

## Related Topics

- [[What-is-OptionalChaining]] - Optional chaining overview
- [[Use-OptionalChaining]] - Using optional chaining
- [[What-is-Nullish]] - Nullish coalescing
- [[What-is-Null]] - null
- [[What-is-Undefined]] - undefined
