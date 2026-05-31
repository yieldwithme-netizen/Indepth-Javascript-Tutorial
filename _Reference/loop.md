# Loops in JavaScript

## Definition

Loops are control structures that repeat a block of code until a specified condition is met. JavaScript provides several loop types: `for`, `while`, `do...while`, `for...in`, `for...of`, and higher-order array methods.

---

## Loop Types

### for Loop

```javascript
// Traditional for loop
for (let i = 0; i < 5; i++) {
  console.log(i); // 0, 1, 2, 3, 4
}

// Counting backwards
for (let i = arr.length - 1; i >= 0; i--) {
  console.log(arr[i]);
}

// Step by 2
for (let i = 0; i < 10; i += 2) {
  console.log(i); // 0, 2, 4, 6, 8
}
```

### while Loop

```javascript
// Basic while
let i = 0;
while (i < 5) {
  console.log(i);
  i++;
}

// Read until EOF
let line;
while ((line = readline()) !== null) {
  processLine(line);
}

// Wait for condition
while (isLoading) {
  await new Promise(resolve => setTimeout(resolve, 100));
}
```

### do...while Loop

```javascript
// Executes at least once
let count = 0;
do {
  console.log(count); // Runs even if count >= 5
  count++;
} while (count < 5);

// Input validation
let input;
do {
  input = prompt("Enter a number between 1 and 10:");
} while (isNaN(input) || input < 1 || input > 10);
```

### for...in Loop

```javascript
// Iterates over enumerable properties (including prototype)
const obj = { a: 1, b: 2, c: 3 };

for (const key in obj) {
  console.log(key, obj[key]);
}

// Always check hasOwnProperty
for (const key in obj) {
  if (obj.hasOwnProperty(key)) {
    console.log(key, obj[key]);
  }
}

// Best for: objects (not arrays!)
```

### for...of Loop

```javascript
// Iterates over iterable values
const arr = [1, 2, 3, 4, 5];

for (const val of arr) {
  console.log(val); // 1, 2, 3, 4, 5
}

// Works with strings
for (const char of "hello") {
  console.log(char); // h, e, l, l, o
}

// Works with Map
const map = new Map([["a", 1], ["b", 2]]);
for (const [key, val] of map) {
  console.log(key, val);
}

// Works with Set
const set = new Set([1, 2, 3]);
for (const val of set) {
  console.log(val);
}

// With entries
for (const [index, val] of arr.entries()) {
  console.log(index, val);
}
```

---

## Array Iteration Methods

### forEach()

```javascript
const arr = ["a", "b", "c"];

arr.forEach((val, index) => {
  console.log(`${index}: ${val}`);
});

// Cannot break out early!
arr.forEach((val) => {
  if (val === "b") return; // Only skips this iteration
  console.log(val); // "a", "c"
});
```

### map() - Transform

```javascript
const arr = [1, 2, 3, 4, 5];
const doubled = arr.map(x => x * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

// Chaining
const result = arr
  .map(x => x * 2)
  .filter(x => x > 4)
  .map(x => x + 1);
console.log(result); // [7, 9, 11]
```

### filter() - Select

```javascript
const arr = [1, 2, 3, 4, 5, 6];
const evens = arr.filter(x => x % 2 === 0);
console.log(evens); // [2, 4, 6]
```

### reduce() - Accumulate

```javascript
const arr = [1, 2, 3, 4, 5];
const sum = arr.reduce((acc, val) => acc + val, 0);
console.log(sum); // 15

// Group by
const items = [
  { type: "fruit", name: "apple" },
  { type: "veggie", name: "carrot" },
  { type: "fruit", name: "banana" }
];

const grouped = items.reduce((acc, item) => {
  if (!acc[item.type]) acc[item.type] = [];
  acc[item.type].push(item);
  return acc;
}, {});
```

### find() and findIndex()

```javascript
const arr = [1, 2, 3, 4, 5];

const found = arr.find(x => x > 3); // 4
const index = arr.findIndex(x => x > 3); // 3
```

### some() and every()

```javascript
const arr = [1, 2, 3, 4, 5];

arr.some(x => x > 4); // true (at least one)
arr.every(x => x > 0); // true (all)
arr.every(x => x > 3); // false (not all)
```

---

## Common Use Cases

### Processing Collections

```javascript
const users = [
  { id: 1, name: "Alice", active: true },
  { id: 2, name: "Bob", active: false },
  { id: 3, name: "Charlie", active: true }
];

// Get active user names
const activeNames = users
  .filter(u => u.active)
  .map(u => u.name);
console.log(activeNames); // ["Alice", "Charlie"]

// Sum of IDs
const totalId = users.reduce((sum, u) => sum + u.id, 0);
console.log(totalId); // 6
```

### Nested Loop Patterns

```javascript
// Matrix traversal
const matrix = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];

// Traditional nested for
for (let i = 0; i < matrix.length; i++) {
  for (let j = 0; j < matrix[i].length; j++) {
    console.log(matrix[i][j]);
  }
}

// forEach
matrix.forEach(row => {
  row.forEach(cell => {
    console.log(cell);
  });
});

// flat + for...of
for (const cell of matrix.flat()) {
  console.log(cell);
}
```

### Async Iteration

```javascript
// Sequential
async function processSequential(items) {
  for (const item of items) {
    await processItem(item);
  }
}

// Parallel
async function processParallel(items) {
  await Promise.all(items.map(item => processItem(item)));
}

// for await...of (async iterables)
async function* asyncGenerator() {
  yield 1;
  yield 2;
  yield 3;
}

for await (const val of asyncGenerator()) {
  console.log(val);
}
```

### Breaking and Continuing

```javascript
// break - exit loop
for (let i = 0; i < 10; i++) {
  if (i === 5) break;
  console.log(i); // 0, 1, 2, 3, 4
}

// continue - skip iteration
for (let i = 0; i < 10; i++) {
  if (i % 2 === 0) continue;
  console.log(i); // 1, 3, 5, 7, 9
}

// Labels (for nested loops)
outer: for (let i = 0; i < 3; i++) {
  for (let j = 0; j < 3; j++) {
    if (i === 1 && j === 1) break outer;
    console.log(i, j);
  }
}
```

---

## Common Mistakes

### Mistake 1: Using for...in on Arrays

```javascript
const arr = ["a", "b", "c"];

// Wrong: for...in gives indices as strings
for (const key in arr) {
  console.log(key, typeof key); // "0" string
}

// Correct: use for...of
for (const val of arr) {
  console.log(val); // "a", "b", "c"
}
```

### Mistake 2: Modifying Array During Iteration

```javascript
const arr = [1, 2, 3, 4, 5];

// Wrong: skips elements
for (let i = 0; i < arr.length; i++) {
  if (arr[i] === 3) arr.splice(i, 1);
}

// Correct: filter creates new array
const filtered = arr.filter(x => x !== 3);
```

### Mistake 3: Async in forEach

```javascript
const arr = [1, 2, 3];

// Wrong: doesn't wait for async
arr.forEach(async (val) => {
  await process(val); // All start immediately!
});

// Correct: use for...of
for (const val of arr) {
  await process(val); // Waits for each
}

// Or use Promise.all for parallel
await Promise.all(arr.map(val => process(val)));
```

### Mistake 4: Infinite Loops

```javascript
// Wrong: i never changes
while (true) {
  console.log("forever");
}

// Correct: ensure condition changes
let i = 0;
while (i < 10) {
  console.log(i);
  i++; // Don't forget!
}

// Or use for loop
for (let i = 0; i < 10; i++) {
  console.log(i);
}
```

### Mistake 5: Missing Return in forEach

```javascript
const arr = [1, 2, 3, 4, 5];

// Wrong: forEach doesn't return
const result = arr.forEach(x => x * 2);
console.log(result); // undefined

// Correct: use map
const doubled = arr.map(x => x * 2);
```

---

## Quick Revision Summary

| Loop | Use Case | Can Break? |
|------|----------|------------|
| `for` | Counted iteration | Yes |
| `while` | Condition-based | Yes |
| `do...while` | Execute at least once | Yes |
| `for...in` | Object keys | Yes |
| `for...of` | Iterable values | Yes |
| `forEach` | Array iteration | No |
| `map` | Transform array | No |
| `filter` | Select elements | No |
| `reduce` | Accumulate value | No |

---

## Related Topics

- [[Array]] - Array iteration methods
- [[Symbol-Iteration]] - Iteration protocol
- [[this]] - `this` in loop callbacks
- [[Promise]] - Async iteration patterns
- [[Object]] - Iterating object properties
