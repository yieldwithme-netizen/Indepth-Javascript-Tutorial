# What is [[filter]]()?

## Definition

`filter()` creates a **new [[array]]** with elements that pass a test.

## Syntax

```javascript
const newArray = array.filter(callback(element, index, array));
```

## Examples

```javascript
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];

// Even numbers
const evens = numbers.filter(num => num % 2 === 0);
console.log(evens); // [2, 4, 6, 8, 10]

// Numbers > 5
const bigNumbers = numbers.filter(num => num > 5);
console.log(bigNumbers); // [6, 7, 8, 9, 10]

// With index
const oddIndex = numbers.filter((num, i) => i % 2 !== 0);
console.log(oddIndex); // [2, 4, 6, 8, 10]

// Filter objects
const users = [
    { name: "John", age: 30, active: true },
    { name: "Jane", age: 25, active: false },
    { name: "Bob", age: 35, active: true }
];
const activeUsers = users.filter(user => user.active);
console.log(activeUsers); // [{name: "John", ...}, {name: "Bob", ...}]
```

## Chaining with [[map]]

```javascript
const users = [
    { name: "John", age: 30 },
    { name: "Jane", age: 15 },
    { name: "Bob", age: 20 }
];

// Get names of users over 18
const adultNames = users
    .filter(user => user.age >= 18)
    .map(user => user.name);

console.log(adultNames); // ["John", "Bob"]
```

## Quick Revision

- `filter()` tests each element
- Returns new [[array]] (subset)
- Elements that pass test are included
- Original [[array]] unchanged ([[immutable]])
- Use for: selecting, removing, querying

---

## Related Topics

- [[What-is-Array]] - Arrays overview
- [[Use-Filter]] - Filter usage
- [[What-is-Map]] - [[map]]
- [[What-is-Reduce]] - [[reduce]]
- [[What-is-HOF]] - [[higher-order]] [[function]]s
