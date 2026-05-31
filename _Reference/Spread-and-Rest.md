# Spread and Rest

## Definition

- **Spread**: expands iterable into individual elements
- **Rest**: collects multiple elements into an array

## Spread

```javascript
// Array
const arr1 = [1, 2];
const arr2 = [...arr1, 3, 4]; // [1, 2, 3, 4]

// Object
const obj1 = { a: 1 };
const obj2 = { ...obj1, b: 2 }; // { a: 1, b: 2 }
```

## Rest

```javascript
// Function parameters
function sum(...args) {
    return args.reduce((a, b) => a + b, 0);
}
sum(1, 2, 3); // 6

// Destructuring
const [first, ...rest] = [1, 2, 3, 4];
// first = 1, rest = [2, 3, 4]
```

## Quick Revision

- Spread: `...` expands
- Rest: `...` collects
- Spread for copying/merging
- Rest for variable arguments

---

## Related Topics

- [[What-is-Spread]] - [[What-is-Spread|Spread]]
- [[Use-Spread]] - [[Use-Spread|Using spread]]
- [[What-is-RestParam]] - [[What-is-RestParam|Rest parameters]]
- [[Spread-and-Rest]] - [[Spread-and-Rest|Spread and rest]]
