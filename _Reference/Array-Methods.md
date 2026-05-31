# Array Methods

JavaScript arrays provide powerful built-in methods for transforming, searching, and manipulating data. These methods are essential for functional programming and data processing.

## Definition

Array methods are built-in functions available on all JavaScript arrays. They can be categorized into iteration methods, transformation methods, search methods, and modification methods.

## Iteration Methods

```javascript
const numbers = [1, 2, 3, 4, 5];

// forEach - execute function for each element
numbers.forEach((num, index) => {
    console.log(`Index ${index}: ${num}`);
});

// map - create new array by transforming each element
const doubled = numbers.map(num => num * 2);
// [2, 4, 6, 8, 10]

// filter - create new array with elements that pass test
const evens = numbers.filter(num => num % 2 === 0);
// [2, 4]

// reduce - accumulate values into single result
const sum = numbers.reduce((accumulator, current) => {
    return accumulator + current;
}, 0);
// 15

// some - test if at least one element passes
const hasLarge = numbers.some(num => num > 4); // true

// every - test if all elements pass
const allPositive = numbers.every(num => num > 0); // true

// find - return first element that passes test
const firstEven = numbers.find(num => num % 2 === 0); // 2

// findIndex - return index of first element that passes test
const firstEvenIndex = numbers.findIndex(num => num % 2 === 0); // 1
```

## Transformation Methods

```javascript
const arr = [3, 1, 4, 1, 5, 9, 2, 6];

// sort - sort elements (modifies original)
const sorted = arr.sort((a, b) => a - b);
// [1, 1, 2, 3, 4, 5, 6, 9]

// reverse - reverse array (modifies original)
const reversed = arr.reverse();
// [9, 6, 5, 4, 3, 2, 1]

// flat - flatten nested arrays
const nested = [1, [2, 3], [4, [5, 6]]];
const flat = nested.flat(); // [1, 2, 3, 4, [5, 6]]
const deepFlat = nested.flat(Infinity); // [1, 2, 3, 4, 5, 6]

// flatMap - map then flatten one level
const phrases = ['hello world', 'goodbye moon'];
const words = phrases.flatMap(phrase => phrase.split(' '));
// ['hello', 'world', 'goodbye', 'moon']

// slice - extract portion (does not modify)
const sliced = arr.slice(2, 5); // [4, 1, 5]

// concat - combine arrays
const arr1 = [1, 2];
const arr2 = [3, 4];
const combined = arr1.concat(arr2); // [1, 2, 3, 4]
```

## Search Methods

```javascript
const fruits = ['apple', 'banana', 'cherry', 'banana'];

// indexOf - find first index of element
const firstBanana = fruits.indexOf('banana'); // 1

// lastIndexOf - find last index of element
const lastBanana = fruits.lastIndexOf('banana'); // 3

// includes - check if element exists
const hasApple = fruits.includes('apple'); // true

// find - find first matching element
const longName = fruits.find(fruit => fruit.length > 5); // 'banana'

// findIndex - find index of first matching
const longIndex = fruits.findIndex(fruit => fruit.length > 5); // 1
```

## Modification Methods

```javascript
const arr = [1, 2, 3];

// push - add to end
arr.push(4); // [1, 2, 3, 4]

// pop - remove from end
arr.pop(); // [1, 2, 3]

// unshift - add to beginning
arr.unshift(0); // [0, 1, 2, 3]

// shift - remove from beginning
arr.shift(); // [1, 2, 3]

// splice - add/remove at position
arr.splice(1, 1, 'a', 'b'); // [1, 'a', 'b', 3]

// fill - fill with static value
arr.fill(0, 1, 3); // [1, 0, 0, 3]

// copyWithin - copy part of array to another position
arr.copyWithin(0, 2); // [0, 0, 1, 3]
```

## Common Use Cases

- Data transformation and mapping
- Filtering datasets
- Aggregating values (sum, average)
- Searching through collections
- Removing duplicates

## Common Mistakes

1. **Mutating original array** - `sort()`, `reverse()`, `splice()` modify in place
2. **Not returning in map/filter** - Arrow functions need explicit return or parentheses
3. **Using wrong sort comparator** - Default sorts alphabetically, not numerically
4. **Forgetting filter returns new array** - Original is unchanged
5. **Chaining too many methods** - Can hurt readability and performance

## Related Topics

- [[Array Creation]]
- [[Higher-Order Functions]]
- [[Functional Programming]]
- [[Spread Operator]]
- [[Destructuring]]

## Quick Revision

| Method | Purpose | Mutates? |
|--------|---------|----------|
| `map()` | Transform elements | No |
| `filter()` | Select elements | No |
| `reduce()` | Accumulate values | No |
| `forEach()` | Iterate | No |
| `find()` | Find element | No |
| `sort()` | Sort elements | Yes |
| `splice()` | Add/remove | Yes |
| `push()/pop()` | Add/remove ends | Yes |
