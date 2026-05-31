# What is Immutability

Immutability is the concept that once a value is created, it cannot be changed. Instead of modifying existing data, you create new copies with the desired changes.

## Definition

Immutability means data is **read-only** after creation. Any "modification" produces a new value while the original remains unchanged.

```javascript
// Mutable (avoid)
const arr = [1, 2, 3];
arr.push(4); // Original array changed

// Immutable (preferred)
const arr = [1, 2, 3];
const newArr = [...arr, 4]; // New array, original unchanged
```

## Why Immutability Matters

- Predictable state changes
- Easier debugging and testing
- Safe concurrent operations
- Prevents unintended side effects

## Common Techniques

### Object Spread
```javascript
const user = { name: "Alice", age: 25 };
const updatedUser = { ...user, age: 26 };
// user.age is still 25
```

### Array Spread
```javascript
const items = [1, 2, 3];
const newItems = [...items, 4];
// items is still [1, 2, 3]
```

### Object.assign
```javascript
const config = { theme: "dark", lang: "en" };
const newConfig = Object.assign({}, config, { lang: "fr" });
```

### Array Methods That Return New Arrays
```javascript
const nums = [1, 2, 3, 4];

const doubled = nums.map(n => n * 2);      // [2, 4, 6, 8]
const evens = nums.filter(n => n % 2 === 0); // [2, 4]
const sum = nums.reduce((acc, n) => acc + n, 0); // 10
```

## Common Mistakes

- Mutating state directly in React or Redux
- Forgetting spread creates a **shallow** copy only
- Using `push`, `splice`, `sort` which mutate in place

```javascript
// Wrong - mutates original
const sorted = arr.sort((a, b) => a - b);

// Correct - creates new array
const sorted = [...arr].sort((a, b) => a - b);
```

## Quick Revision

- Immutability = data cannot change after creation
- Use spread operator `...` to create copies
- Use `.map()`, `.filter()`, `.reduce()` instead of mutating methods
- Always create new objects/arrays instead of modifying existing ones

## Related Topics

- [[What-is-Currying]]
- [[What-is-Composition]]
- [[What-is-Memoization]]
- [[React-State-Management]]
- [[Redux-Patterns]]
