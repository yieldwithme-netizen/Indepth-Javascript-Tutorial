# What is Array [[Length]]?

## Definition

The `length` property returns the **number of elements** in an [[array]].

## Basic Usage

```javascript
const arr = [1, 2, 3, 4, 5];
console.log(arr.length); // 5
```

## Truncating Arrays

```javascript
const arr = [1, 2, 3, 4, 5];
arr.length = 3;
console.log(arr); // [1, 2, 3]
```

## Extending Arrays

```javascript
const arr = [1, 2, 3];
arr.length = 5;
console.log(arr); // [1, 2, 3, empty × 2]
```

## Common Patterns

```javascript
// Last element
const last = arr[arr.length - 1];

// Check if empty
if (arr.length === 0) {
    console.log("Array is empty");
}

// Loop through array
for (let i = 0; i < arr.length; i++) {
    console.log(arr[i]);
}

// Empty array
arr.length = 0;
```

## Quick Revision

- `.length` = number of elements
- Can be used to truncate (shorten) [[array]]
- Can be used to extend (add empties)
- Use for loops and checking emptiness
- Last element: `arr[arr.length - 1]`

---

## Related Topics

- [[What-is-Array]] - Arrays overview
- [[Get-Length]] - Getting length
- [[What-is-Index]] - Array [[index]]ing
- [[Access-Elements]] - Accessing elements
