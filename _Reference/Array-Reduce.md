# Array Reduce

## Definition

`reduce()` reduces an array to a **single value** by applying a function.

## Basic Syntax

```javascript
const result = array.reduce(callback(accumulator, currentValue, index, array), initialValue);
```

## Examples

```javascript
const numbers = [1, 2, 3, 4, 5];

// Sum
const sum = numbers.reduce((acc, num) => acc + num, 0);
console.log(sum); // 15

// Product
const product = numbers.reduce((acc, num) => acc * num, 1);
console.log(product); // 120

// Find max
const max = numbers.reduce((acc, num) => num > acc ? num : acc, numbers[0]);
console.log(max); // 5

// Flatten array
const nested = [[1, 2], [3, 4], [5, 6]];
const flat = nested.reduce((acc, arr) => acc.concat(arr), []);
console.log(flat); // [1, 2, 3, 4, 5, 6]

// Count occurrences
const fruits = ["apple", "banana", "apple", "orange", "banana"];
const count = fruits.reduce((acc, fruit) => {
    acc[fruit] = (acc[fruit] || 0) + 1;
    return acc;
}, {});
console.log(count); // {apple: 2, banana: 2, orange: 1}
```

## Quick Revision

- `reduce()` accumulates array to single value
- Needs initial value (always provide!)
- Accumulator carries result through
- Use for: sum, product, max, flatten, group

---

## Related Topics

- [[What-is-Reduce]] - [[What-is-Reduce|Reduce]] overview
- [[Array-Reduce]] - [[Array-Reduce|Array reduce]]
- [[Use-Reduce]] - [[Use-Reduce|Using reduce]]
- [[What-is-Map]] - [[What-is-Map|Map]]
- [[What-is-Filter]] - [[What-is-Filter|Filter]]
