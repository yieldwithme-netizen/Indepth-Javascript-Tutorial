# Array Sort

## Definition
The `Array.prototype.sort()` method sorts the elements of an array in place and returns the sorted array. By default, it sorts elements as strings in alphabetical order.

## Syntax
```javascript
array.sort(compareFn)
```

- **compareFn**: Optional. Function that determines order. Returns negative, zero, or positive.

## Code Examples

### Default Sort (String)
```javascript
const fruits = ['banana', 'apple', 'cherry'];
fruits.sort();
console.log(fruits); // ['apple', 'banana', 'cherry']

const numbers = [10, 9, 80, 1];
numbers.sort();
console.log(numbers); // [1, 10, 80, 9] (string sort!)
```

### Numeric Sort
```javascript
const numbers = [10, 9, 80, 1];
numbers.sort((a, b) => a - b);
console.log(numbers); // [1, 9, 10, 80]

// Descending
numbers.sort((a, b) => b - a);
console.log(numbers); // [80, 10, 9, 1]
```

### Sort Objects by Property
```javascript
const users = [
  { name: 'Alice', age: 30 },
  { name: 'Bob', age: 25 },
  { name: 'Charlie', age: 35 }
];

users.sort((a, b) => a.age - b.age);
console.log(users);
// [{ name: 'Bob', age: 25 }, { name: 'Alice', age: 30 }, { name: 'Charlie', age: 35 }]
```

### Sort Strings (Case-Insensitive)
```javascript
const words = ['Banana', 'apple', 'Cherry'];

// Case-insensitive sort
words.sort((a, b) => a.toLowerCase().localeCompare(b.toLowerCase()));
console.log(words); // ['apple', 'Banana', 'Cherry']
```

### Stable Sort
```javascript
const items = [
  { name: 'Alice', order: 2 },
  { name: 'Bob', order: 1 },
  { name: 'Charlie', order: 2 }
];

// Stable sort preserves relative order for equal elements
items.sort((a, b) => a.order - b.order);
// Bob first, then Alice and Charlie in original order
```

### Reverse Sort
```javascript
const numbers = [1, 2, 3, 4, 5];
numbers.reverse();
console.log(numbers); // [5, 4, 3, 2, 1]

// Or sort descending
const desc = [...numbers].sort((a, b) => b - a);
```

### Sort with localeCompare
```javascript
const cities = ['New York', 'Los Angeles', 'Chicago', 'San Francisco'];
cities.sort((a, b) => a.localeCompare(b));
console.log(cities);
// ['Chicago', 'Los Angeles', 'New York', 'San Francisco']
```

## Common Use Cases
- Sorting search results
- Organizing data tables
- Priority queues
- Alphabetical lists

## Common Mistakes
- **Forgetting numeric sort**: Default sort is string-based
- **Mutating original array**: `sort()` mutates; use `[...arr].sort()` to preserve
- **Not handling undefined**: Compare function should handle equal values
- **Locale issues**: Use `localeCompare()` for proper string sorting

## Related Topics
- [[Array-Methods]]
- [[Array-Filter]]
- [[Array-Map]]
- [[Array-Reduce]]
- [[Sorting-Algorithms]]

## Quick Revision
- `sort()` mutates the original array
- Default sort is string-based, not numeric
- Use compare function `(a, b) => a - b` for numbers
- Use `localeCompare()` for proper string sorting
- Modern JS engines use stable sort
