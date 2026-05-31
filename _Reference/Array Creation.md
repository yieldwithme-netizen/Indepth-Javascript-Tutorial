# Array Creation

## Definition

Array creation involves **different ways** to create arrays in JavaScript.

## Methods

```javascript
// Array literal
const arr = [1, 2, 3];

// Constructor
const arr2 = new Array(1, 2, 3);

// Array.from
const arr3 = Array.from("hello");

// Array.of
const arr4 = Array.of(1, 2, 3);

// Spread
const arr5 = [...[1, 2], 3, 4];

// Fill
const arr6 = Array(5).fill(0);
```

## Quick Revision

- `[]` literal (preferred)
- `new Array()`
- `Array.from()` from iterables
- `Array.of()` from arguments
- Spread for merging

---

## Related Topics

- [[What-is-Array]] - [[What-is-Array|Arrays]]
- [[Create-Array]] - [[Create-Array|Creating arrays]]
- [[Array Creation]] - [[Array Creation|Array creation]]
