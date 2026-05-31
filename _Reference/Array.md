# Arrays in JavaScript

## Definition

An Array is an ordered collection of elements stored in a single variable. Arrays in JavaScript are dynamic, can hold mixed data types, and provide powerful built-in methods for manipulation, iteration, and transformation.

---

## Creating Arrays

```javascript
// Array literal (recommended)
const arr1 = [1, 2, 3];
const arr2 = ["a", "b", "c"];
const arr3 = [1, "two", true, null]; // Mixed types

// Constructor
const arr4 = new Array(1, 2, 3);
const arr5 = new Array(5); // Creates array with 5 empty slots

// From string
const arr6 = Array.from("hello"); // ['h', 'e', 'l', 'l', 'o']

// Of
const arr7 = Array.of(1, 2, 3); // [1, 2, 3]

// From iterable
const arr8 = Array.from(new Set([1, 2, 2, 3])); // [1, 2, 3]
```

---

## Common Array Methods

### Adding & Removing Elements

```javascript
const arr = [1, 2, 3];

// push - adds to end
arr.push(4); // [1, 2, 3, 4]

// pop - removes from end
arr.pop(); // [1, 2, 3]

// unshift - adds to beginning
arr.unshift(0); // [0, 1, 2, 3]

// shift - removes from beginning
arr.shift(); // [1, 2, 3]

// splice - adds/removes at any position
arr.splice(1, 1); // Removes 1 element at index 1 → [1, 3]
arr.splice(1, 0, 2); // Inserts 2 at index 1 → [1, 2, 3]
```

### Searching

```javascript
const arr = [1, 2, 3, 4, 5];

// indexOf - first index of element
arr.indexOf(3); // 2

// lastIndexOf - last index of element
arr.lastIndexOf(3); // 2

// includes - checks if element exists
arr.includes(3); // true

// find - first element matching condition
arr.find(x => x > 3); // 4

// findIndex - index of first matching element
arr.findIndex(x => x > 3); // 3
```

### Transforming

```javascript
const arr = [1, 2, 3, 4, 5];

// map - transforms each element
arr.map(x => x * 2); // [2, 4, 6, 8, 10]

// filter - keeps matching elements
arr.filter(x => x > 3); // [4, 5]

// reduce - accumulates to single value
arr.reduce((sum, x) => sum + x, 0); // 15

// flat - flattens nested arrays
[1, [2, 3], [4, [5]]].flat(); // [1, 2, 3, 4, [5]]
[1, [2, 3], [4, [5]]].flat(Infinity); // [1, 2, 3, 4, 5]

// flatMap - map + flatten
[1, 2, 3].flatMap(x => [x, x * 2]); // [1, 2, 2, 4, 3, 6]
```

### Sorting

```javascript
const arr = [3, 1, 4, 1, 5, 9];

// sort - sorts in place (lexicographic by default!)
arr.sort(); // [1, 1, 3, 4, 5, 9]

// Numeric sort (requires comparator)
[10, 9, 80, 1].sort((a, b) => a - b); // [1, 9, 10, 80]
[10, 9, 80, 1].sort((a, b) => b - a); // [80, 10, 9, 1]

// reverse - reverses in place
[1, 2, 3].reverse(); // [3, 2, 1]
```

### Iteration

```javascript
const arr = ["a", "b", "c"];

// forEach - executes function for each element
arr.forEach((item, index) => {
  console.log(`${index}: ${item}`);
});

// for...of - iterates values
for (const item of arr) {
  console.log(item);
}

// entries - returns [index, value] pairs
for (const [i, val] of arr.entries()) {
  console.log(i, val);
}
```

### Copying & Slicing

```javascript
const arr = [1, 2, 3, 4, 5];

// slice - returns shallow copy of portion
arr.slice(1, 3); // [2, 3]
arr.slice(); // [1, 2, 3, 4, 5] (full copy)

// concat - merges arrays
arr.concat([6, 7]); // [1, 2, 3, 4, 5, 6, 7]

// spread operator (recommended for copying)
const copy = [...arr];

// Array.from
const copy2 = Array.from(arr);
```

---

## Common Use Cases

### Flattening Nested Data

```javascript
const users = [
  { id: 1, name: "Alice", tags: ["admin", "user"] },
  { id: 2, name: "Bob", tags: ["user"] }
];

const allTags = users.flatMap(u => u.tags);
// ["admin", "user", "user"]

const uniqueTags = [...new Set(allTags)];
// ["admin", "user"]
```

### Grouping Elements

```javascript
const items = [
  { type: "fruit", name: "apple" },
  { type: "vegetable", name: "carrot" },
  { type: "fruit", name: "banana" }
];

const grouped = items.reduce((acc, item) => {
  if (!acc[item.type]) acc[item.type] = [];
  acc[item.type].push(item);
  return acc;
}, {});
// { fruit: [...], vegetable: [...] }
```

### Chaining Methods

```javascript
const result = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
  .filter(x => x % 2 === 0)
  .map(x => x * x)
  .reduce((sum, x) => sum + x, 0);

// 4 + 16 + 36 + 64 + 100 = 220
```

### Creating Ranges

```javascript
// Generate numbers 1-10
const range = Array.from({ length: 10 }, (_, i) => i + 1);

// Generate letters a-z
const letters = Array.from({ length: 26 }, (_, i) =>
  String.fromCharCode(97 + i)
);
```

---

## Common Mistakes

### Mistake 1: Mutating Original Array

```javascript
// Wrong: mutates original
const arr = [1, 2, 3];
arr.sort(); // Changes arr

// Correct: create copy first
const sorted = [...arr].sort((a, b) => a - b);
```

### Mistake 2: Missing Return in map

```javascript
// Wrong
const doubled = arr.map(x => {
  x * 2; // Returns undefined!
});

// Correct
const doubled = arr.map(x => x * 2);
```

### Mistake 3: Using `for...in` on Arrays

```javascript
// Wrong: for...in gives indices, iterates prototypes
const arr = [1, 2, 3];
for (const key in arr) {
  console.log(key); // "0", "1", "2" (strings, not numbers)
}

// Correct: use for...of
for (const value of arr) {
  console.log(value); // 1, 2, 3
}
```

### Mistake 4: Creating Empty Slots

```javascript
// This creates sparse array with empty slots
const arr = new Array(3);
arr[0] = 1;
console.log(arr); // [1, empty × 2]
arr.map(x => x * 2); // [2, empty × 2] - empty slots remain!
```

---

## Quick Revision Summary

| Operation | Method | Returns |
|-----------|--------|---------|
| Add to end | `push()` | New length |
| Remove from end | `pop()` | Removed element |
| Add to start | `unshift()` | New length |
| Remove from start | `shift()` | Removed element |
| Find element | `find()` | Element or undefined |
| Transform | `map()` | New array |
| Filter | `filter()` | New array |
| Accumulate | `reduce()` | Single value |
| Sort | `sort()` | Sorted array |
| Copy | `[...arr]` | New array |

---

## Related Topics

- [[Symbol-Iterator]] - How arrays implement iteration protocol
- [[Array-Access]] - Accessing and manipulating array elements
- [[this]] - `this` context in array methods
- [[loop]] - Looping through arrays
- [[Object]] - Converting between objects and arrays
