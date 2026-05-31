# What is an [[Array]]?

## Definition

An [[array]] is an **ordered collection** of values, stored in a [[variable]] with a numeric [[index]].

## Creating Arrays

```javascript
// Array literal (preferred)
const fruits = ["apple", "banana", "orange"];

// Constructor
const numbers = new Array(1, 2, 3, 4, 5);

// Empty array
const empty = [];

// Array with values
const mixed = [1, "hello", true, null, { name: "John" }];
```

## Array Properties

```javascript
const arr = [1, 2, 3, 4, 5];

arr.length; // 5
arr[0];     // 1 (first element)
arr[arr.length - 1]; // 5 (last element)
```

## Array Methods

```javascript
// Add/Remove
arr.push(6);        // Add to end
arr.pop();          // Remove from end
arr.unshift(0);     // Add to start
arr.shift();        // Remove from start

// Transform
arr.reverse();      // Reverse order
arr.sort();         // Sort alphabetically
arr.fill(0, 1, 3); // Fill range with value

// Search
arr.includes(3);    // true
arr.indexOf(3);     // 2
arr.find(x => x > 3); // 4
arr.findIndex(x => x > 3); // 3

// Iterate
arr.forEach(x => console.log(x));
arr.map(x => x * 2);
arr.filter(x => x > 2);
arr.reduce((sum, x) => sum + x, 0);

// Extract
arr.slice(1, 3);    // [2, 3]
arr.splice(1, 2);   // Remove 2 elements at index 1
```

## Array is an [[Object]]

```javascript
const arr = [1, 2, 3];
typeof arr; // "object"
Array.isArray(arr); // true
arr instanceof Array; // true
```

## Quick Revision

- [[array]] = ordered list of values
- [[index]] starts at 0
- Created with `[]` or `new Array()`
- `.length` for size
- Methods: [[push]], [[pop]], [[map]], [[filter]], [[reduce]]

---

## Related Topics

- [[Create-Array]] - Creating arrays
- [[Access-Elements]] - Accessing elements
- [[Get-Length]] - Array [[length]]
- [[Add-Remove-End]] - Push/pop
- [[Use-Map]] - [[map]] method
- [[Use-Filter]] - [[filter]] method
- [[Use-Reduce]] - [[reduce]] method
