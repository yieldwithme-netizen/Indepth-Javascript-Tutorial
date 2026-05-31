# Return Statement in JavaScript

## Definition

The **return** statement exits a function and optionally passes a value back to the caller. Every function returns a value—if no return statement is specified, the function returns `undefined`. Return is essential for creating reusable, composable functions.

---

## Basic Return

```javascript
// Explicit return
function add(a, b) {
  return a + b;
}

const result = add(2, 3); // 5

// Implicit return (arrow functions)
const multiply = (a, b) => a * b;

// No return - returns undefined
function logMessage(msg) {
  console.log(msg);
  // Returns undefined
}

const nothing = logMessage("Hello"); // undefined
```

---

## Early Return

```javascript
// Exit function early based on condition
function processUser(user) {
  if (!user) return null;
  if (!user.name) return "Name required";
  if (!user.email) return "Email required";
  
  return { id: Date.now(), ...user };
}

// Guard clauses
function divide(a, b) {
  if (b === 0) return Infinity;
  return a / b;
}

// Validation
function saveData(data) {
  if (!data) return { success: false, error: "No data" };
  if (!data.id) return { success: false, error: "No ID" };
  
  // Save logic...
  return { success: true };
}
```

---

## Multiple Returns

```javascript
// Different returns based on condition
function getDiscount(membership) {
  switch (membership) {
    case "gold": return 0.2;
    case "silver": return 0.1;
    case "bronze": return 0.05;
    default: return 0;
  }
}

// Ternary operator
function isEven(num) {
  return num % 2 === 0 ? true : false;
}

// Short-circuit evaluation
function getConfig(key) {
  return config[key] ?? defaultValue;
}
```

---

## Returning Objects

```javascript
// Return object literal (wrap in parentheses)
const createUser = (name, age) => ({ name, age, active: true });

// Return multiple values
function getMinMax(arr) {
  return { min: Math.min(...arr), max: Math.max(...arr) };
}

const { min, max } = getMinMax([1, 5, 3, 9, 2]);

// Return array
function getCoordinates() {
  return [x, y, z];
}

const [x, y, z] = getCoordinates();
```

---

## Return in Arrow Functions

```javascript
// With body (explicit return)
const add1 = (a, b) => {
  return a + b;
};

// Concise body (implicit return)
const add2 = (a, b) => a + b;

// Returning object (need parentheses)
const makeUser = (name) => ({ name, id: Date.now() });

// Multi-line arrow function
const process = (x) => {
  const doubled = x * 2;
  const squared = x ** 2;
  return { doubled, squared };
};
```

---

## Common Use Cases

### Data Transformation

```javascript
// Transform array
const numbers = [1, 2, 3, 4, 5];
const doubled = numbers.map(n => n * 2);
const evens = numbers.filter(n => n % 2 === 0);
const sum = numbers.reduce((acc, n) => acc + n, 0);

// Transform object
const updateUser = (user, updates) => ({
  ...user,
  ...updates,
  updatedAt: new Date()
});
```

### Higher-Order Functions

```javascript
// Function that returns a function
function createMultiplier(multiplier) {
  return (number) => number * multiplier;
}

const double = createMultiplier(2);
const triple = createMultiplier(3);

double(5); // 10
triple(5); // 15

// Function factory
function createValidator(pattern) {
  return (value) => pattern.test(value);
}

const isEmail = createValidator(/^\S+@\S+$/);
const isPhone = createValidator(/^\d{10}$/);
```

### Closures

```javascript
function createCounter(initial = 0) {
  let count = initial;
  
  return {
    increment: () => ++count,
    decrement: () => --count,
    getCount: () => count,
    reset: () => (count = initial)
  };
}

const counter = createCounter(10);
counter.increment(); // 11
counter.increment(); // 12
counter.getCount(); // 12
counter.reset(); // 10
```

### Promise Resolution

```javascript
// Return promise
function fetchData(url) {
  return fetch(url)
    .then(response => response.json())
    .catch(error => ({ error: error.message }));
}

// Async function
async function getUser(id) {
  const response = await fetch(`/api/users/${id}`);
  if (!response.ok) throw new Error("User not found");
  return response.json();
}
```

---

## Common Mistakes

### Mistake 1: Forgetting to Return

```javascript
// Wrong: returns undefined
function calculateTotal(items) {
  items.reduce((sum, item) => sum + item.price, 0);
}

// Correct: add return
function calculateTotal(items) {
  return items.reduce((sum, item) => sum + item.price, 0);
}
```

### Mistake 2: Returning Multiple Values

```javascript
// Wrong: returns only first value
function getMinMax(arr) {
  return Math.min(...arr), Math.max(...arr);
}

// Correct: return array or object
function getMinMax(arr) {
  return [Math.min(...arr), Math.max(...arr)];
}
```

### Mistake 3: Arrow Function Parentheses

```javascript
// Wrong: treated as function body
const getUser = () => { name: "Alice" }; // undefined

// Correct: wrap in parentheses
const getUser = () => ({ name: "Alice" }); // { name: "Alice" }
```

### Mistake 4: Return in Loops

```javascript
// Returns on first iteration
function findFirst(arr, condition) {
  for (const item of arr) {
    if (condition(item)) {
      return item; // Exits entire function
    }
  }
  return null;
}

// Wrong: only returns first match
function findAll(arr, condition) {
  const results = [];
  for (const item of arr) {
    if (condition(item)) {
      return item; // Returns immediately!
    }
    results.push(item);
  }
  return results;
}

// Correct: push to array, return at end
function findAll(arr, condition) {
  return arr.filter(item => condition(item));
}
```

---

## Void Operator

```javascript
// Explicitly return undefined
function log(msg) {
  console.log(msg);
  return void 0; // Same as no return
}

// In arrow functions
const log2 = (msg) => (console.log(msg), void 0);
```

---

## Quick Revision Summary

| Pattern | Example | Returns |
|---------|---------|---------|
| Explicit | `return value` | `value` |
| Implicit | `(a, b) => a + b` | sum |
| Early | `if (cond) return x` | `x` |
| None | `function f() {}` | `undefined` |
| Object | `return { a: 1 }` | `{ a: 1 }` |
| Arrow object | `() => ({ a: 1 })` | `{ a: 1 }` |
| Promise | `return fetch(url)` | Promise |

---

## Related Topics

- [[function]] - Function fundamentals
- [[Arrow]] - Arrow function return
- [[expression]] - Expressions in return
- [[Closures]] - Returning closures
- [[Higher-Order-Functions]] - Functions returning functions
- [[Promise]] - Async returns
- [[async]] - Async/await returns