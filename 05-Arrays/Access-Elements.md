# How to Access Array Elements

## Index Access

```javascript
const fruits = ["apple", "banana", "orange"];

// Access by index (0-based)
console.log(fruits[0]); // "apple"
console.log(fruits[1]); // "banana"
console.log(fruits[2]); // "orange"
```

## Negative Index (ES2022)

```javascript
const arr = [1, 2, 3, 4, 5];

// at() method
console.log(arr.at(-1)); // 5 (last element)
console.log(arr.at(-2)); // 4 (second to last)
```

## Last Element

```javascript
const arr = [1, 2, 3, 4, 5];

// Method 1: length
console.log(arr[arr.length - 1]); // 5

// Method 2: at()
console.log(arr.at(-1)); // 5

// Method 3: slice
console.log(arr.slice(-1)[0]); // 5
```

## Modifying Elements

```javascript
const arr = [1, 2, 3];
arr[1] = 20;
console.log(arr); // [1, 20, 3]
```

## Out of Bounds

```javascript
const arr = [1, 2, 3];

// Returns undefined (no error!)
console.log(arr[10]); // undefined
```

## Quick Revision

- Index starts at 0
- Access: `arr[index]`
- Use `.at(-1)` for last element
- Out of bounds returns undefined
- Arrays are zero-indexed

---

## Related Topics

- [[What-is-Index]] - [[What-is-Index|Array index]] overview
- [[What-is-Array]] - [[What-is-Array|Arrays]]
- [[Access-Elements]] - [[Access-Elements|Accessing elements]]
- [[Get-Length]] - [[Get-Length|Array length]]
