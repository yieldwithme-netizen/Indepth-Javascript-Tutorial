# What is Destructuring?

## Definition

Destructuring **extracts values** from arrays/objects into variables.

## Object Destructuring

```javascript
const person = { name: "John", age: 30, city: "NYC" };

// Basic
const { name, age } = person;
console.log(name); // "John"

// Rename
const { name: userName, age: userAge } = person;
console.log(userName); // "John"

// Default
const { name, gender = "male" } = person;
console.log(gender); // "male"

// Nested
const user = { name: "John", address: { city: "NYC" } };
const { address: { city } } = user;
console.log(city); // "NYC"
```

## Array Destructuring

```javascript
const arr = [1, 2, 3, 4, 5];

// Basic
const [a, b] = arr;
console.log(a, b); // 1, 2

// Skip elements
const [a, , c] = arr;
console.log(a, c); // 1, 3

// Rest
const [a, ...rest] = arr;
console.log(rest); // [2, 3, 4, 5]

// Swap variables
let x = 1, y = 2;
[x, y] = [y, x];
console.log(x, y); // 2, 1
```

## Function Parameters

```javascript
// Object destructuring in parameters
function greet({ name, age }) {
    return `Hello, ${name}! You are ${age}.`;
}
greet({ name: "John", age: 30 });

// Array destructuring in parameters
function first([first, ...rest]) {
    return first;
}
first([1, 2, 3]); // 1
```

## Quick Revision

- Destructuring extracts values to variables
- Works with objects and arrays
- Use `{}` for objects, `[]` for arrays
- Supports defaults, renaming, nesting
- Great for function parameters

---

## Related Topics

- [[What-is-Destructuring]] - Destructuring overview
- [[Destructure-Objects]] - Object destructuring
- [[Destructure-Arrays]] - Array destructuring
- [[What-is-Spread]] - Spread operator
- [[What-is-RestParam]] - Rest parameters
