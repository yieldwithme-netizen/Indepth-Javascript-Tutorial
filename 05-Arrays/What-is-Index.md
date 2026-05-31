# What is an Array [[Index]]?

## Definition

An [[index]] is the **numeric position** of an element in an [[array]], starting from 0.

## Index Basics

```javascript
const fruits = ["apple", "banana", "orange"];
//              [0]      [1]       [2]

fruits[0]; // "apple"
fruits[1]; // "banana"
fruits[2]; // "orange"
```

## Accessing Elements

```javascript
const arr = [10, 20, 30, 40, 50];

// Positive index (from start)
arr[0];  // 10
arr[2];  // 30

// Negative index (from end) - ES2022
arr.at(-1); // 50
arr.at(-2); // 40

// Last element
arr[arr.length - 1]; // 50
```

## Modifying Elements

```javascript
const arr = [1, 2, 3];
arr[1] = 20;
console.log(arr); // [1, 20, 3]
```

## Adding Elements by [[Index]]

```javascript
const arr = [1, 2, 3];
arr[5] = 6; // Sparse array
console.log(arr); // [1, 2, 3, empty × 2, 6]
```

## Common Mistakes

```javascript
const arr = [1, 2, 3];

// ❌ Wrong: Out of bounds
console.log(arr[10]); // undefined (no error!)

// ✅ Right: Check length
if (index < arr.length) {
    console.log(arr[index]);
}
```

## Quick Revision

- [[index]] starts at 0
- Access: `arr[index]`
- Use `.at(-1)` for last element
- Out of bounds returns undefined
- [[array]]s are zero-indexed

---

## Related Topics

- [[What-is-Array]] - Arrays overview
- [[Access-Elements]] - Accessing elements
- [[What-is-Indexing]] - String indexing
- [[Get-Length]] - Array [[length]]
