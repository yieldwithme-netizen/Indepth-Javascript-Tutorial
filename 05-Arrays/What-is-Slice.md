# What is [[slice]]?

## Definition

`slice()` extracts a **portion of an [[array]]** without modifying the original.

## Syntax

```javascript
array.slice(start, end)
```

- `start`: [[index]] to start (inclusive)
- `end`: [[index]] to end (exclusive)

## Examples

```javascript
const arr = [1, 2, 3, 4, 5];

// Extract middle
const middle = arr.slice(1, 3);
console.log(middle); // [2, 3]

// Extract from index to end
const from2 = arr.slice(2);
console.log(from2); // [3, 4, 5]

// Extract first 2
const first2 = arr.slice(0, 2);
console.log(first2); // [1, 2]

// Copy entire array
const copy = arr.slice();
console.log(copy); // [1, 2, 3, 4, 5]

// Negative index (from end)
const last2 = arr.slice(-2);
console.log(last2); // [4, 5]
```

## slice vs splice

```javascript
// slice: doesn't modify original
const arr = [1, 2, 3, 4, 5];
const sliced = arr.slice(1, 3);
console.log(arr); // [1, 2, 3, 4, 5] (unchanged)
console.log(sliced); // [2, 3]

// splice: modifies original
const arr2 = [1, 2, 3, 4, 5];
const spliced = arr2.splice(1, 3);
console.log(arr2); // [1, 5] (changed!)
console.log(spliced); // [2, 3, 4]
```

## Quick Revision

- `slice(start, end)` extracts portion
- Doesn't modify original (non-mutating / [[immutable]])
- Returns new [[array]]
- `end` [[index]] is exclusive
- Use negative indices for from-end

---

## Related Topics

- [[What-is-Array]] - Arrays overview
- [[Use-Slice]] - Slice usage
- [[What-is-Splice]] - [[splice]] ([[mutable]])
- [[Extract-Substring]] - [[string]] slice
