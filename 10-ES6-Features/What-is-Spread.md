# What is Spread Operator?

## Definition

The spread operator (`...`) **expands** an iterable into individual elements.

## Array Spread

```javascript
// Copy array
const arr1 = [1, 2, 3];
const arr2 = [...arr1];

// Merge arrays
const arr1 = [1, 2];
const arr2 = [3, 4];
const merged = [...arr1, ...arr2]; // [1, 2, 3, 4]

// Add to array
const arr = [1, 2, 3];
const newArr = [0, ...arr, 4]; // [0, 1, 2, 3, 4]
```

## Object Spread

```javascript
// Copy object
const obj1 = { a: 1, b: 2 };
const obj2 = { ...obj1 };

// Merge objects
const obj1 = { a: 1 };
const obj2 = { b: 2 };
const merged = { ...obj1, ...obj2 }; // { a: 1, b: 2 }

// Override properties
const defaults = { color: "red", size: "medium" };
const custom = { ...defaults, color: "blue" }; // { color: "blue", size: "medium" }
```

## Function Arguments

```javascript
// Pass array as arguments
const numbers = [1, 2, 3];
const sum = Math.max(...numbers); // 3

// Copy arguments
function sum(...args) {
    return args.reduce((a, b) => a + b, 0);
}
```

## Spread vs Rest

```javascript
// Spread: expands
const arr = [1, 2, 3];
const copy = [...arr]; // expands arr

// Rest: collects
function sum(...args) { // collects args into array
    return args.reduce((a, b) => a + b, 0);
}
```

## Quick Revision

- `...` spreads iterable into elements
- Copy arrays/objects
- Merge arrays/objects
- Pass as function arguments
- Don't mutate originals

---

## Related Topics

- [[What-is-Spread]] - Spread overview
- [[Use-Spread]] - Using spread
- [[What-is-RestParam]] - Rest parameters
- [[What-is-Destructuring]] - Destructuring
- [[Destructure-Objects]] - Object destructuring
