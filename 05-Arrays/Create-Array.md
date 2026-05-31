# How to Create Arrays

## Array Literal (Preferred)

```javascript
// Empty array
const arr = [];

// With values
const fruits = ["apple", "banana", "orange"];

// Mixed types
const mixed = [1, "hello", true, null, { name: "John" }];

// Nested arrays
const nested = [[1, 2], [3, 4], [5, 6]];
```

## Constructor

```javascript
// Empty array
const arr = new Array();

// With values
const numbers = new Array(1, 2, 3, 4, 5);

// With length
const empty = new Array(10); // Creates array with 10 empty slots
```

## Array.from()

```javascript
// From string
const arr = Array.from("hello");
console.log(arr); // ["h", "e", "l", "l", "o"]

// From range
const range = Array.from({ length: 5 }, (_, i) => i);
console.log(range); // [0, 1, 2, 3, 4]

// From Set
const set = new Set([1, 2, 3]);
const arr = Array.from(set);
console.log(arr); // [1, 2, 3]
```

## Array.of()

```javascript
// Creates array from arguments
const arr = Array.of(1, 2, 3);
console.log(arr); // [1, 2, 3]

// Single number (unlike Array constructor)
const arr = Array.of(5);
console.log(arr); // [5] (not empty array!)
```

## Quick Revision

- Array literal: `[]` (preferred)
- Constructor: `new Array()`
- `Array.from()` creates from iterables
- `Array.of()` creates from arguments
- Arrays can hold any data type

---

## Related Topics

- [[What-is-Array]] - [[What-is-Array|Arrays]] overview
- [[Create-Array]] - [[Create-Array|Creating arrays]]
- [[Access-Elements]] - [[Access-Elements|Accessing elements]]
- [[Get-Length]] - [[Get-Length|Array length]]
