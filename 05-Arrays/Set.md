# Set

## Definition

Set is a **collection of unique values**.

## Basic Usage

```javascript
const set = new Set([1, 2, 2, 3, 3]);
console.log([...set]); // [1, 2, 3]

set.add(4);
set.has(2); // true
set.size; // 4
set.delete(2);
```

## Set Operations

```javascript
const a = new Set([1, 2, 3]);
const b = new Set([2, 3, 4]);

// Union
const union = new Set([...a, ...b]);

// Intersection
const intersection = new Set([...a].filter(x => b.has(x)));

// Difference
const difference = new Set([...a].filter(x => !b.has(x)));
```

## Quick Revision

- Set = unique values
- Methods: add, has, delete, size
- Use for: deduplication, set operations

---

## Related Topics

- [[What-is-Set]] - [[What-is-Set|Set]]
- [[Set]] - [[Set|Set]]
- [[Use-Set]] - [[Use-Set|Using Set]]
- [[Set-Object]] - [[Set-Object|Set object]]
- [[Set-Operations]] - [[Set-Operations|Set operations]]
