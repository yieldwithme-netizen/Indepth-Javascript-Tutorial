# What is Pure Function

## Definition

A pure function is a function that always produces the same output for the same input and has no side effects. It doesn't modify external state, doesn't depend on external mutable state, and its only effect is returning a value.

---

## Two Core Rules

### Rule 1: Same Input → Same Output

A pure function's return value depends only on its arguments:

```javascript
// Pure - always returns 10 for add(3, 7)
function add(a, b) {
  return a + b;
}

// Impure - result depends on external variable
let taxRate = 0.1;
function calculateTax(amount) {
  return amount * taxRate; // Changes if taxRate changes
}

// Make it pure - pass dependencies as arguments
function calculateTax(amount, taxRate) {
  return amount * taxRate;
}
```

### Rule 2: No Side Effects

A pure function doesn't modify anything outside itself:

```javascript
// Impure - modifies external variable
let count = 0;
function increment() {
  count++; // Side effect!
  return count;
}

// Impure - modifies input object
function addItem(cart, item) {
  cart.items.push(item); // Mutates input!
  return cart;
}

// Pure - creates new object
function addItem(cart, item) {
  return {
    ...cart,
    items: [...cart.items, item],
  };
}

// Pure - no external modification
function increment(value) {
  return value + 1;
}
```

---

## Examples

### Pure Functions

```javascript
// String manipulation
function capitalize(str) {
  return str.charAt(0).toUpperCase() + str.slice(1);
}

// Array operations
function double(numbers) {
  return numbers.map(n => n * 2);
}

function filterEven(numbers) {
  return numbers.filter(n => n % 2 === 0);
}

// Object operations
function updateUser(user, updates) {
  return { ...user, ...updates };
}

// Mathematical calculations
function circleArea(radius) {
  return Math.PI * radius * radius;
}

// All pure: same input always gives same output, no side effects
```

### Impure Functions

```javascript
// Reads external state
function getTime() {
  return Date.now(); // Different every call!
}

// Modifies global state
let total = 0;
function addToTotal(amount) {
  total += amount; // Side effect!
  return total;
}

// Modifies input
function sortArray(arr) {
  return arr.sort(); // Mutates original array!
}

// Makes network request
async function fetchUser(id) {
  return await fetch(`/api/users/${id}`); // Side effect!
}

// Reads DOM
function getButtonText() {
  return document.getElementById("btn").textContent; // Depends on DOM
}
```

---

## Common Use Cases

### Data Transformation

```javascript
// Pure transformations
const users = [
  { name: "Alice", age: 25, active: true },
  { name: "Bob", age: 17, active: false },
  { name: "Charlie", age: 30, active: true },
];

function getActiveAdults(users) {
  return users.filter(u => u.age >= 18 && u.active);
}

function getUserNames(users) {
  return users.map(u => u.name);
}

function sortByAge(users) {
  return [...users].sort((a, b) => a.age - b.age);
}

// All pure - original array unchanged
const adults = getActiveAdults(users);
const names = getUserNames(users);
const sorted = sortByAge(users);
```

### State Reducers

```javascript
// Pure reducer function
function todoReducer(state, action) {
  switch (action.type) {
    case "ADD":
      return [...state, { id: action.id, text: action.text, done: false }];
    case "TOGGLE":
      return state.map(todo =>
        todo.id === action.id ? { ...todo, done: !todo.done } : todo
      );
    case "REMOVE":
      return state.filter(todo => todo.id !== action.id);
    default:
      return state;
  }
}

// Usage
const todos = [];
const state1 = todoReducer(todos, { type: "ADD", id: 1, text: "Learn FP" });
const state2 = todoReducer(state1, { type: "TOGGLE", id: 1 });
```

### Composition

```javascript
// Pure functions compose well
const trim = str => str.trim();
const lowercase = str => str.toLowerCase();
const split = sep => str => str.split(sep);
const map = fn => arr => arr.map(fn);

// Compose transformations
const processName = compose(trim, lowercase);
const getWords = compose(split(" "), map(capitalize));

processName("  ALICE  "); // "alice"
getWords("hello world");   // ["Hello", "World"]
```

---

## Benefits of Pure Functions

### Testability

```javascript
// Easy to test - no setup, no mocking
function add(a, b) {
  return a + b;
}

test("add returns sum of inputs", () => {
  expect(add(2, 3)).toBe(5);
  expect(add(-1, 1)).toBe(0);
  expect(add(0, 0)).toBe(0);
});

// Impure function requires mocking
let db;
function getUser(id) {
  return db.query(`SELECT * FROM users WHERE id = ${id}`);
}

// Need to mock database
test("getUser returns user", async () => {
  db = { query: jest.fn().mockResolvedValue({ id: 1 }) };
  const user = await getUser(1);
  expect(user.id).toBe(1);
});
```

### Predictability

```javascript
// Pure - predictable
function calculateDiscount(price, discountPercent) {
  return price * (1 - discountPercent / 100);
}

calculateDiscount(100, 20); // Always 80

// Impure - unpredictable
let currentDiscount = 0;
function getDiscountedPrice(price) {
  return price * (1 - currentDiscount / 100);
}

// Result depends on external state
currentDiscount = 20;
getDiscountedPrice(100); // 80
currentDiscount = 30;
getDiscountedPrice(100); // 70 - different!
```

### Memoization

```javascript
// Pure functions can be memoized
function memoize(fn) {
  const cache = new Map();
  return function(...args) {
    const key = JSON.stringify(args);
    if (cache.has(key)) {
      return cache.get(key);
    }
    const result = fn.apply(this, args);
    cache.set(key, result);
    return result;
  };
}

const expensivePure = memoize(function(n) {
  console.log("Computing...");
  return n * n;
});

expensivePure(4); // Computing... 16
expensivePure(4); // 16 (cached, no "Computing...")
```

---

## Common Mistakes

**Mistake 1: Mutating arguments**

```javascript
// Bad - mutates input
function addItem(cart, item) {
  cart.items.push(item); // Mutation!
  return cart;
}

// Good - return new object
function addItem(cart, item) {
  return { ...cart, items: [...cart.items, item] };
}
```

**Mistake 2: Relying on external state**

```javascript
// Bad - depends on external variable
let multiplier = 2;
function multiply(n) {
  return n * multiplier;
}

// Good - pass all dependencies
function multiply(n, multiplier) {
  return n * multiplier;
}
```

**Mistake 3: Performing I/O operations**

```javascript
// Bad - side effect
function processUser(user) {
  console.log("Processing:", user); // I/O!
  return user.name.toUpperCase();
}

// Good - return data, let caller handle I/O
function formatUserName(user) {
  return user.name.toUpperCase();
}

// Caller handles logging
const name = formatUserName(user);
console.log("Processed:", name);
```

---

## Converting Impure to Pure

```javascript
// Impure: modifies array
function sortInPlace(arr) {
  return arr.sort((a, b) => a - b);
}

// Pure: returns new array
function sort(arr) {
  return [...arr].sort((a, b) => a - b);
}

// Impure: modifies object
function addProperty(obj, key, value) {
  obj[key] = value;
  return obj;
}

// Pure: returns new object
function addProperty(obj, key, value) {
  return { ...obj, [key]: value };
}

// Impure: depends on time
function isExpired(date) {
  return date < new Date();
}

// Pure: pass current time
function isExpired(date, now) {
  return date < now;
}
```

---

## Related Topics

- [[What-is-FP]]
- [[What-is-Immutability]]
- [[What-is-Composition]]
- [[What-is-HOF]]
- [[What-is-Curry]]

---

## Quick Revision

- Pure function: same input → same output, no side effects
- Don't modify arguments or external state
- Don't perform I/O (console, network, DOM)
- Pass all dependencies as arguments
- Pure functions are easy to test and compose
- Convert impure functions by returning new values instead of mutating
- Memoization works only with pure functions
