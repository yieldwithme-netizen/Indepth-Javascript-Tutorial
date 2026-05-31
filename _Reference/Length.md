# Length Property (Array & String)

## Definition

The **length** property returns the number of elements in an array or the number of characters in a string. It is a built-in property available on all Array and String objects in JavaScript.

For arrays, `length` is dynamic and adjusts automatically when elements are added or removed. For strings, `length` is read-only and counts UTF-16 code units.

---

## Syntax

### Array Length
```javascript
const array = [1, 2, 3, 4, 5];
console.log(array.length); // Output: 5
```

### String Length
```javascript
const string = 'Hello';
console.log(string.length); // Output: 5
```

---

## Code Examples

### Array Length Basics
```javascript
const fruits = ['apple', 'banana', 'cherry'];
console.log(fruits.length); // Output: 3

// Add element
fruits.push('date');
console.log(fruits.length); // Output: 4

// Remove element
fruits.pop();
console.log(fruits.length); // Output: 3
```

### Array Length is Dynamic
```javascript
const arr = [1, 2, 3];
console.log(arr.length); // Output: 3

// Manually set length (removes elements)
arr.length = 1;
console.log(arr); // Output: [1]

// Extend array with empty slots
arr.length = 5;
console.log(arr); // Output: [1, empty × 4]
```

### String Length Basics
```javascript
const name = 'JavaScript';
console.log(name.length); // Output: 10

const empty = '';
console.log(empty.length); // Output: 0

const space = ' ';
console.log(space.length); // Output: 1
```

### Length with Unicode Characters
```javascript
// Emoji and special characters
const emoji = '😀🎉';
console.log(emoji.length); // Output: 4 (not 2!)

const chinese = '你好世界';
console.log(chinese.length); // Output: 4

// Using Array.from for correct count
console.log(Array.from(emoji).length); // Output: 2
```

### Checking if Array is Empty
```javascript
const emptyArray = [];
const nonEmptyArray = [1, 2, 3];

if (emptyArray.length === 0) {
  console.log('Array is empty');
}

if (nonEmptyArray.length) {
  console.log('Array has elements');
}
```

### Iterating with Length
```javascript
const numbers = [10, 20, 30, 40, 50];

// Classic for loop
for (let i = 0; i < numbers.length; i++) {
  console.log(numbers[i]);
}

// Cache length for performance
const len = numbers.length;
for (let i = 0; i < len; i++) {
  console.log(numbers[i]);
}
```

### Using Length with Slice and Splice
```javascript
const arr = [1, 2, 3, 4, 5];

// Get last element
const last = arr[arr.length - 1];
console.log(last); // Output: 5

// Get last N elements
const lastTwo = arr.slice(-2);
console.log(lastTwo); // Output: [4, 5]

// Remove from end
arr.splice(-2, 2);
console.log(arr); // Output: [1, 2, 3]
```

### String Length with Trimming
```javascript
const padded = '   Hello   ';
console.log(padded.length);        // Output: 11
console.log(padded.trim().length); // Output: 5
```

### Multidimensional Arrays
```javascript
const matrix = [
  [1, 2, 3],
  [4, 5, 6],
  [7, 8, 9]
];

console.log(matrix.length);        // Output: 3 (rows)
console.log(matrix[0].length);     // Output: 3 (columns)
console.log(matrix[1].length);     // Output: 3
```

### Converting Length to Array
```javascript
// Create array from length
const arr1 = Array(5);
console.log(arr1); // Output: [empty × 5]

// Create filled array
const arr2 = Array.from({ length: 5 }, (_, i) => i + 1);
console.log(arr2); // Output: [1, 2, 3, 4, 5]

// Create range
const range = Array.from({ length: 10 }, (_, i) => i);
console.log(range); // Output: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

---

## Common Use Cases

| Use Case | Description |
|----------|-------------|
| **Boundary Checks** | Prevent index out of bounds errors |
| **Iteration** | Loop through arrays/strings |
| **Emptiness Check** | Determine if array/string is empty |
| **Array Manipulation** | Add/remove elements by setting length |
| **Performance** | Cache length for faster loops |
| **Unicode Handling** | Handle multi-byte characters correctly |

---

## Common Mistakes

### 1. Modifying Array Length
```javascript
const arr = [1, 2, 3, 4, 5];
arr.length = 2;
console.log(arr); // Output: [1, 2] - Elements 3, 4, 5 are deleted!
```

### 2. String Length with Unicode
```javascript
const emoji = '😀';
console.log(emoji.length); // Output: 2 (not 1!)

// Use Array.from for correct count
console.log(Array.from(emoji).length); // Output: 1
```

### 3. Empty vs Undefined
```javascript
const arr = [1, 2, 3];
arr.length = 5;
console.log(arr[3]); // Output: undefined (not null)

// Check for actual elements
console.log(3 in arr); // Output: false
```

---

## Quick Revision Summary

- `length` returns the number of elements (array) or characters (string)
- Array `length` is dynamic and can be modified
- String `length` is read-only
- Unicode characters may count as more than 1 length
- Use `Array.from()` for accurate Unicode length
- Cache `length` in variables for better loop performance
- `arr[arr.length - 1]` gets the last element

---

## Related Topics

- [[function]] - Functions that work with arrays/strings
- [[JavaScript]] - JavaScript language overview
- [[Local-Storage]] - Storing arrays/strings
- [[let]] - Loop variables with length
- [[Logical-Operators]] - Checking length in conditions
