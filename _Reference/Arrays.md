# Arrays

## Definition

An array is an **ordered collection** of values stored in a variable with numeric indices.

## Creating Arrays

```javascript
// Array literal
const arr = [1, 2, 3, 4, 5];

// Constructor
const arr2 = new Array(1, 2, 3);

// Empty array
const empty = [];
```

## Accessing Elements

```javascript
const fruits = ["apple", "banana", "orange"];

fruits[0]; // "apple"
fruits[1]; // "banana"
fruits[fruits.length - 1]; // "orange"
```

## Common Methods

```javascript
// Add/Remove
arr.push(6);        // Add to end
arr.pop();          // Remove from end
arr.unshift(0);     // Add to start
arr.shift();        // Remove from start

// Transform
arr.reverse();      // Reverse order
arr.sort();         // Sort alphabetically

// Search
arr.includes(3);    // true
arr.indexOf(3);     // 2
arr.find(x => x > 3); // 4

// Iterate
arr.forEach(x => console.log(x));
arr.map(x => x * 2);
arr.filter(x => x > 2);
arr.reduce((sum, x) => sum + x, 0);
```

## Quick Revision

- Array = ordered list of values
- Index starts at 0
- Created with `[]` or `new Array()`
- `.length` for size
- Methods: push, pop, map, filter, reduce

---

## Related Topics

- [[What-is-Array]] - [[What-is-Array|Arrays]] overview
- [[Create-Array]] - [[Create-Array|Creating arrays]]
- [[Access-Elements]] - [[Access-Elements|Accessing elements]]
- [[Array-Methods]] - [[Array-Methods|Array methods]]
