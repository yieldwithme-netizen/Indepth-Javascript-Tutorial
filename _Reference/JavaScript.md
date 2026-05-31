# JavaScript Overview

## Definition

**JavaScript** is a high-level, interpreted programming language that is one of the core technologies of the World Wide Web (alongside HTML and CSS). It enables interactive web pages and is an essential part of web applications. JavaScript is a versatile, multi-paradigm language that supports event-driven, functional, and imperative programming styles.

Originally created by Brendan Eich in 1995, JavaScript has evolved into a powerful language used for client-side development, server-side development (Node.js), mobile apps, desktop apps, and more.

---

## Key Features

| Feature | Description |
|---------|-------------|
| **Dynamic Typing** | Variables can hold any type of data |
| **First-Class Functions** | Functions can be assigned to variables, passed as arguments |
| **Prototype-based OOP** | Object-oriented via prototypes |
| **Event-Driven** | Responds to user interactions and events |
| **Asynchronous** | Supports callbacks, promises, and async/await |
| **Platform Independent** | Runs in any browser or runtime environment |

---

## Code Examples

### Variables and Data Types
```javascript
// Variable declarations
var name = 'Alice';        // Function scoped (legacy)
let age = 30;              // Block scoped
const PI = 3.14159;        // Block scoped, constant

// Data types
const string = 'Hello';
const number = 42;
const float = 3.14;
const boolean = true;
const nothing = null;
const undefinedVar = undefined;
const symbol = Symbol('id');
const bigInt = 9007199254740991n;

// Objects and Arrays
const person = { name: 'Alice', age: 30 };
const numbers = [1, 2, 3, 4, 5];
```

### Functions
```javascript
// Function declaration
function add(a, b) {
  return a + b;
}

// Arrow function
const multiply = (a, b) => a * b;

// Higher-order function
function applyOperation(a, b, operation) {
  return operation(a, b);
}

console.log(applyOperation(5, 3, add));      // Output: 8
console.log(applyOperation(5, 3, multiply)); // Output: 15
```

### Control Flow
```javascript
// If/else
const score = 85;
if (score >= 90) {
  console.log('A');
} else if (score >= 80) {
  console.log('B');
} else {
  console.log('C');
}

// For loop
for (let i = 0; i < 5; i++) {
  console.log(i);
}

// While loop
let count = 0;
while (count < 3) {
  console.log(count);
  count++;
}

// Switch
const day = 'Monday';
switch (day) {
  case 'Monday':
    console.log('Start of week');
    break;
  case 'Friday':
    console.log('End of week');
    break;
  default:
    console.log('Midweek');
}
```

### Arrays and Methods
```javascript
const fruits = ['apple', 'banana', 'cherry'];

// Add/Remove
fruits.push('date');         // Add to end
fruits.pop();                // Remove from end
fruits.unshift('elderberry'); // Add to beginning
fruits.shift();              // Remove from beginning

// Iteration
fruits.forEach(fruit => console.log(fruit));
const upper = fruits.map(fruit => fruit.toUpperCase());
const long = fruits.filter(fruit => fruit.length > 5);
const found = fruits.find(fruit => fruit.startsWith('b'));

// Spread operator
const moreFruits = [...fruits, 'fig', 'grape'];
```

### Objects and Destructuring
```javascript
const user = {
  name: 'Alice',
  age: 30,
  address: {
    city: 'New York',
    state: 'NY'
  }
};

// Destructuring
const { name, age } = user;
const { address: { city } } = user;

// Spread
const updatedUser = { ...user, age: 31 };

// Object methods
const calculator = {
  add(a, b) { return a + b; },
  subtract(a, b) { return a - b; }
};
```

### Promises and Async/Await
```javascript
// Promise
function fetchData() {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve({ data: 'Hello' });
    }, 1000);
  });
}

// Using Promise
fetchData()
  .then(result => console.log(result))
  .catch(error => console.error(error));

// Async/Await
async function getData() {
  try {
    const result = await fetchData();
    console.log(result);
  } catch (error) {
    console.error(error);
  }
}
```

### ES6+ Features
```javascript
// Template literals
const greeting = `Hello, ${name}!`;

// Optional chaining
const city = user?.address?.city;

// Nullish coalescing
const value = null ?? 'default';

// Array destructuring
const [first, second, ...rest] = [1, 2, 3, 4, 5];

// Default parameters
function greet(name = 'Guest') {
  return `Hello, ${name}`;
}

// Map and Set
const map = new Map();
map.set('key', 'value');

const set = new Set([1, 2, 3, 3]);
```

---

## Common Use Cases

| Use Case | Description |
|----------|-------------|
| **Web Development** | Frontend interactivity with DOM manipulation |
| **Server-Side** | Backend APIs with Node.js or Deno |
| **Mobile Apps** | Cross-platform apps with React Native |
| **Desktop Apps** | Native-like apps with Electron |
| **Game Development** | Browser-based games with Canvas/WebGL |
| **Data Visualization** | Charts and graphs with D3.js |

---

## Common Mistakes

### 1. Using `var` Instead of `let`/`const`
```javascript
// Avoid
var x = 10;

// Prefer
let x = 10;      // If reassignment needed
const x = 10;    // If value won't change
```

### 2. Double vs Triple Equals
```javascript
// Loose equality (type coercion)
console.log(5 == '5');   // true

// Strict equality (no type coercion)
console.log(5 === '5');  // false

// Always prefer ===
```

### 3. Forgetting `async`/`await`
```javascript
// WRONG - Missing await
async function getData() {
  const data = fetchData(); // Returns Promise, not data
}

// CORRECT
async function getData() {
  const data = await fetchData(); // Resolves the promise
}
```

---

## Quick Revision Summary

- JavaScript is a versatile, multi-paradigm programming language
- It supports dynamic typing, first-class functions, and prototypal inheritance
- Modern JavaScript (ES6+) includes arrow functions, destructuring, promises, and modules
- Use `let` and `const` instead of `var` for block scoping
- Always use strict equality (`===`) to avoid type coercion bugs
- JavaScript runs in browsers, servers (Node.js), mobile, and desktop environments
- Async programming is handled with promises and async/await

---

## Related Topics

- [[function]] - Function fundamentals
- [[let]] - Block-scoped variables
- [[Logical-Operators]] - Logical operations
- [[Function-Scope-and-Closures]] - Scope and closures
- [[IIFE]] - Immediately invoked function expressions
- [[Local-Storage]] - Client-side storage
- [[Length]] - Array/String length
- [[JSDoc]] - Code documentation
