# Parameters in JavaScript

## Definition

**Parameters** are variables listed in a function's definition that act as placeholders for the values (**arguments**) the function receives when called. JavaScript offers flexible parameter handling including default values, rest parameters, and destructuring.

---

## Basic Parameters

```javascript
// Function with parameters
function greet(name, greeting) {
  return `${greeting}, ${name}!`;
}

// Arguments are passed during function call
greet("Alice", "Hello"); // "Hello, Alice!"
greet("Bob", "Hi"); // "Hi, Bob!"

// Parameters are local to the function
function test(param) {
  console.log(param); // Accessible here
}
// console.log(param); // Error: param is not defined
```

---

## Default Parameters

```javascript
// ES6 default parameter syntax
function greet(name = "Guest", greeting = "Hello") {
  return `${greeting}, ${name}!`;
}

greet(); // "Hello, Guest!"
greet("Alice"); // "Hello, Alice!"
greet("Bob", "Hi"); // "Hi, Bob!"

// Default can be any expression
function createArray(size = 10, defaultValue = null) {
  return Array(size).fill(defaultValue);
}

// Defaults are evaluated left to right
function test(a = 1, b = a + 1) {
  return [a, b];
}
test(); // [1, 2]
```

---

## Rest Parameters

```javascript
// Collects remaining arguments into an array
function sum(...numbers) {
  return numbers.reduce((total, n) => total + n, 0);
}

sum(1, 2, 3); // 6
sum(1, 2, 3, 4, 5); // 15
sum(); // 0

// Rest must be last parameter
function log(prefix, ...messages) {
  messages.forEach(msg => console.log(`${prefix}: ${msg}`));
}

log("INFO", "Server started", "Listening on port 3000");
// INFO: Server started
// INFO: Listening on port 3000

// Combining with regular parameters
function multiply(multiplier, ...numbers) {
  return numbers.map(n => n * multiplier);
}

multiply(2, 1, 2, 3); // [2, 4, 6]
```

---

## Arguments Object

```javascript
// Traditional way to access all arguments
function oldSum() {
  let total = 0;
  for (let i = 0; i < arguments.length; i++) {
    total += arguments[i];
  }
  return total;
}

oldSum(1, 2, 3); // 6

// Arrow functions don't have arguments object
const arrowSum = () => {
  // console.log(arguments); // ReferenceError
};

// Convert arguments to array
function toArray() {
  return Array.from(arguments);
}

toArray(1, 2, 3); // [1, 2, 3]
```

---

## Parameter Destructuring

```javascript
// Object destructuring in parameters
function createUser({ name, age, email = "N/A" }) {
  return { name, age, email };
}

createUser({ name: "Alice", age: 30 }); // { name: "Alice", age: 30, email: "N/A" }

// Array destructuring in parameters
function getCoordinates([x, y, z = 0]) {
  return { x, y, z };
}

getCoordinates([1, 2, 3]); // { x: 1, y: 2, z: 3 }
getCoordinates([1, 2]); // { x: 1, y: 2, z: 0 }

// Nested destructuring
function processUser({ name, address: { city, country } }) {
  return `${name} from ${city}, ${country}`;
}
```

---

## Passing Objects as Parameters

```javascript
// Named parameters pattern (readable)
function createUser(options) {
  const { name, age, email, role = "user" } = options;
  return { name, age, email, role };
}

createUser({
  name: "Bob",
  age: 25,
  email: "bob@example.com"
});

// Alternative: destructuring directly
function createUser2({ name, age, email, role = "user" }) {
  return { name, age, email, role };
}
```

---

## Common Use Cases

### Event Handlers

```javascript
// Destructuring event parameter
button.addEventListener("click", ({ target, clientX, clientY }) => {
  console.log(`Clicked ${target.tagName} at (${clientX}, ${clientY})`);
});

// Multiple optional parameters
function handleEvent(event, options = {}) {
  const { preventDefault = true, stopPropagation = false } = options;
  if (preventDefault) event.preventDefault();
  if (stopPropagation) event.stopPropagation();
}
```

### API Functions

```javascript
// Clean API with defaults
function fetchUser({
  id,
  baseUrl = "https://api.example.com",
  timeout = 5000,
  retries = 3
}) {
  return fetch(`${baseUrl}/users/${id}`, {
    signal: AbortSignal.timeout(timeout)
  });
}

fetchUser({ id: 123 }); // Uses defaults
fetchUser({ id: 123, baseUrl: "https://other.com" }); // Custom base
```

### Curry-like Patterns

```javascript
// Partial application with defaults
function configure({
  host = "localhost",
  port = 3000,
  secure = false
} = {}) {
  const protocol = secure ? "https" : "http";
  return `${protocol}://${host}:${port}`;
}

configure(); // "http://localhost:3000"
configure({ port: 8080, secure: true }); // "https://localhost:8080"
```

---

## Common Mistakes

### Mistake 1: Swapping Parameters and Arguments

```javascript
// Parameter order matters!
function divide(a, b) {
  return a / b;
}

// Wrong order
divide(10, 2); // 5 ✓
divide(2, 10); // 0.2 ✗

// Solution: use named parameters object
function divide({ dividend, divisor }) {
  return dividend / divisor;
}
divide({ dividend: 10, divisor: 2 }); // 5 ✓
```

### Mistake 2: Missing Return Statement

```javascript
// Wrong: function returns undefined
function add(a, b) {
  a + b; // Missing return!
}

// Correct
function add(a, b) {
  return a + b;
}
```

### Mistake 3: Forgetting Default Parameter Order

```javascript
// Wrong: non-default before default
function test(a = 1, b) {} // SyntaxError in strict mode

// Correct: defaults must come after required params
function test(b, a = 1) {}
```

### Mistake 4: Mutating Parameters

```javascript
// Wrong: modifies original object
function updateUser(user) {
  user.name = "Modified"; // Mutates input!
  return user;
}

// Correct: create copy
function updateUser(user) {
  return { ...user, name: "Modified" };
}
```

---

## Quick Revision Summary

| Type | Syntax | Example |
|------|--------|---------|
| Required | `function test(a)` | `test(1)` |
| Default | `function test(a = 1)` | `test()` |
| Rest | `function test(...a)` | `test(1,2,3)` |
| Destructuring | `function test({a})` | `test({a:1})` |
| Optional chain | `options?.prop` | `options?.name` |

---

## Related Topics

- [[Arrow]] - Arrow function parameters
- [[Destructure-Arrays]] - Array destructuring
- [[Rest-Parameters]] - Rest parameter syntax
- [[Default-Parameters]] - Default parameter values
- [[Function-Scope-and-Closures]] - Parameter scope
- [[Callback]] - Passing functions as parameters
- [[Spread-Operator]] - Spreading arguments