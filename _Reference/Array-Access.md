# Array Access Patterns

## Definition

Array access refers to how you read, write, and manipulate elements in an array. JavaScript arrays use zero-based indexing and support various access patterns including direct indexing, destructuring, and method-based access.

---

## Basic Access Patterns

### Direct Indexing

```javascript
const arr = ["a", "b", "c", "d", "e"];

// Access by index (0-based)
console.log(arr[0]); // "a"
console.log(arr[2]); // "c"
console.log(arr[arr.length - 1]); // "e" (last element)

// Negative indexing (doesn't work!)
console.log(arr[-1]); // undefined

// Modify element
arr[1] = "B";
console.log(arr); // ["a", "B", "c", "d", "e"]
```

### .at() Method (ES2022)

```javascript
const arr = ["a", "b", "c", "d", "e"];

// Positive indexing
console.log(arr.at(0)); // "a"
console.log(arr.at(2)); // "c"

// Negative indexing (works!)
console.log(arr.at(-1)); // "e"
console.log(arr.at(-2)); // "d"
```

### .at() for Strings

```javascript
const str = "hello";
console.log(str.at(-1)); // "o"
```

---

## Destructuring Access

### Basic Destructuring

```javascript
const arr = [1, 2, 3, 4, 5];

// Extract values
const [a, b, c] = arr;
console.log(a, b, c); // 1, 2, 3

// Skip elements
const [first, , third] = arr;
console.log(first, third); // 1, 3

// Rest pattern
const [head, ...tail] = arr;
console.log(head); // 1
console.log(tail); // [2, 3, 4, 5]

// Last element
const [...allButLast] = arr;
const last = allButLast[allButLast.length - 1];
```

### Swapping Variables

```javascript
let a = 1;
let b = 2;

// Swap without temporary variable
[a, b] = [b, a];
console.log(a, b); // 2, 1
```

### Default Values

```javascript
const [a = 1, b = 2, c = 3] = [10, 20];
console.log(a, b, c); // 10, 20, 3
```

### Nested Destructuring

```javascript
const matrix = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];

const [[a, b], , [c]] = matrix;
console.log(a, b, c); // 1, 2, 7
```

---

## Slice and Splice Access

### slice() - Read without Mutating

```javascript
const arr = [1, 2, 3, 4, 5];

// Get sub-array
console.log(arr.slice(1, 3)); // [2, 3]
console.log(arr.slice(2)); // [3, 4, 5]
console.log(arr.slice(-2)); // [4, 5]
console.log(arr.slice()); // [1, 2, 3, 4, 5] (copy)

// arr is unchanged
console.log(arr); // [1, 2, 3, 4, 5]
```

### splice() - Modify in Place

```javascript
const arr = [1, 2, 3, 4, 5];

// Remove elements
const removed = arr.splice(1, 2); // Remove 2 elements at index 1
console.log(removed); // [2, 3]
console.log(arr); // [1, 4, 5]

// Insert elements
arr.splice(1, 0, 2, 3); // Insert at index 1
console.log(arr); // [1, 2, 3, 4, 5]

// Replace elements
arr.splice(1, 2, "a", "b"); // Replace 2 elements at index 1
console.log(arr); // [1, "a", "b", 4, 5]
```

---

## Finding Elements

### find() and findIndex()

```javascript
const users = [
  { id: 1, name: "Alice", age: 25 },
  { id: 2, name: "Bob", age: 30 },
  { id: 3, name: "Charlie", age: 35 }
];

// Find first match
const bob = users.find(u => u.name === "Bob");
console.log(bob); // { id: 2, name: "Bob", age: 30 }

// Find index
const index = users.findIndex(u => u.id === 2);
console.log(index); // 1

// Find last (ES2023)
const older = users.findLast(u => u.age > 28);
console.log(older); // { id: 3, name: "Charlie", age: 35 }
```

### includes() for Primitives

```javascript
const arr = [1, 2, 3, 4, 5];

console.log(arr.includes(3)); // true
console.log(arr.includes(6)); // false
console.log(arr.includes(3, 2)); // true (start from index 2)
```

---

## Common Use Cases

### Accessing First and Last Elements

```javascript
const arr = [1, 2, 3, 4, 5];

// First element
const first = arr[0];
const [firstDestructured] = arr;

// Last element
const last = arr[arr.length - 1];
const lastAt = arr.at(-1);
const [...rest] = arr;
const lastDestructured = rest[rest.length - 1];
```

### Safe Property Access in Arrays

```javascript
const data = [
  { user: { profile: { name: "Alice" } } },
  { user: null },
  {}
];

// Safe access with optional chaining
const names = data.map(d => d?.user?.profile?.name ?? "Unknown");
console.log(names); // ["Alice", "Unknown", "Unknown"]
```

### Chunking Arrays

```javascript
function chunk(arr, size) {
  const chunks = [];
  for (let i = 0; i < arr.length; i += size) {
    chunks.push(arr.slice(i, i + size));
  }
  return chunks;
}

console.log(chunk([1, 2, 3, 4, 5], 2));
// [[1, 2], [3, 4], [5]]
```

### Flattening Access

```javascript
const data = [
  { id: 1, tags: ["js", "react"] },
  { id: 2, tags: ["node", "express"] }
];

// Access nested values
const allTags = data.flatMap(item => item.tags);
console.log(allTags); // ["js", "react", "node", "express"]
```

### Pagination

```javascript
function paginate(arr, page, perPage) {
  const start = (page - 1) * perPage;
  const end = start + perPage;
  return {
    data: arr.slice(start, end),
    total: arr.length,
    page,
    pages: Math.ceil(arr.length / perPage)
  };
}

const result = paginate([1, 2, 3, 4, 5, 6, 7, 8, 9, 10], 2, 3);
console.log(result);
// { data: [4, 5, 6], total: 10, page: 2, pages: 4 }
```

---

## Common Mistakes

### Mistake 1: Accessing Out of Bounds

```javascript
const arr = [1, 2, 3];
console.log(arr[10]); // undefined (no error!)

// Safe access
const value = arr[10] ?? "default";
console.log(value); // "default"
```

### Mistake 2: Mutating While Iterating

```javascript
const arr = [1, 2, 3, 4, 5];

// Wrong: skips elements
arr.forEach((val, i) => {
  if (val === 3) arr.splice(i, 1);
});

// Correct: filter creates new array
const filtered = arr.filter(val => val !== 3);
```

### Mistake 3: Using splice() Incorrectly

```javascript
const arr = [1, 2, 3, 4, 5];

// Wrong: splice returns removed elements, not the array
const result = arr.splice(0, 2);
console.log(result); // [1, 2] (removed elements)
console.log(arr); // [3, 4, 5] (mutated)

// Correct: if you need the array after removal
const remaining = arr.slice();
remaining.splice(0, 2);
```

### Mistake 4: Shallow vs Deep Copy

```javascript
const arr = [[1, 2], [3, 4]];

// Wrong: shallow copy
const shallow = [...arr];
shallow[0][0] = 99;
console.log(arr[0][0]); // 99 (original changed!)

// Correct: deep copy
const deep = structuredClone(arr);
deep[0][0] = 99;
console.log(arr[0][0]); // 1 (original unchanged)
```

---

## Quick Revision Summary

| Pattern | Syntax | Use Case |
|---------|--------|----------|
| Direct index | `arr[i]` | Read/write single element |
| Negative index | `arr.at(-1)` | Access from end |
| Destructuring | `const [a, b] = arr` | Extract multiple values |
| Slice | `arr.slice(start, end)` | Read sub-array |
| Splice | `arr.splice(start, count)` | Modify in place |
| Find | `arr.find(fn)` | Find by condition |
| Spread | `[...arr]` | Copy array |

---

## Related Topics

- [[Array]] - Array creation and methods
- [[Symbol-Iterator]] - Iteration protocol for arrays
- [[Object]] - Accessing object properties
- [[loop]] - Looping through array elements
