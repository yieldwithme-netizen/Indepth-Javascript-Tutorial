# Spread Operator

## Definition

The spread operator (`...`) **expands** an iterable into individual elements.

## Array Spread

```javascript
// Copy
const arr2 = [...arr1];

// Merge
const merged = [...arr1, ...arr2];

// Add elements
const newArr = [0, ...arr, 4];
```

## Object Spread

```javascript
// Copy
const obj2 = { ...obj1 };

// Merge
const merged = { ...obj1, ...obj2 };

// Override
const custom = { ...defaults, color: "blue" };
```

## Function Arguments

```javascript
Math.max(...[1, 2, 3]); // 3
```

## Quick Revision

- `...` spreads iterable
- Copy arrays/objects
- Merge arrays/objects
- Pass as arguments

---

## Related Topics

- [[What-is-Spread]] - [[What-is-Spread|Spread]]
- [[Spread Operator]] - [[Spread Operator|Spread operator]]
- [[Use-Spread]] - [[Use-Spread|Using spread]]
- [[What-is-RestParam]] - [[What-is-RestParam|Rest parameters]]
