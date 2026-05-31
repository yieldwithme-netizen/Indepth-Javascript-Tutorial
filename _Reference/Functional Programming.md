# Functional Programming

## Definition

Functional programming is a **paradigm** treating computation as function evaluation.

## Core Principles

```javascript
// Pure functions
const add = (a, b) => a + b;

// Immutability
const arr = [1, 2, 3];
const newArr = [...arr, 4];

// Higher-order functions
const doubled = arr.map(x => x * 2);
```

## Quick Revision

- Pure functions: no side effects
- Immutability: don't change data
- Higher-order: take/return functions
- Use: map, filter, reduce

---

## Related Topics

- [[What-is-FP]] - [[What-is-FP|Functional programming]]
- [[Functional Programming]] - [[Functional Programming|Functional programming]]
- [[What-is-Pure]] - [[What-is-Pure|Pure functions]]
- [[What-is-Immutability]] - [[What-is-Immutability|Immutability]]
