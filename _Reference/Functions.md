# Functions

## Definition

Functions are reusable blocks of code designed to perform specific tasks. They are fundamental building blocks in JavaScript, enabling code reuse, abstraction, and modular programming. Functions can accept inputs (parameters), execute logic, and return outputs.

## Syntax

```javascript
// Function declaration
function functionName(parameters) {
  // code
  return result;
}

// Function expression
const functionName = function(parameters) {
  // code
  return result;
};

// Arrow function
const functionName = (parameters) => {
  // code
  return result;
};
```

## Code Examples

### Function Declaration

```javascript
function greet(name) {
  return `Hello, ${name}!`;
}

console.log(greet("Alice")); // "Hello, Alice!"
```

### Function Expression

```javascript
const multiply = function(a, b) {
  return a * b;
};

console.log(multiply(3, 4)); // 12
```

### Arrow Functions

```javascript
// Single parameter (no parentheses needed)
const square = x => x * x;

// Multiple parameters
const add = (a, b) => a + b;

// Multi-line arrow function
const calculateArea = (width, height) => {
  const area = width * height;
  return area;
};

console.log(square(5));      // 25
console.log(add(2, 3));      // 5
console.log(calculateArea(4, 6)); // 24
```

### Default Parameters

```javascript
function greet(name = "Guest", greeting = "Hello") {
  return `${greeting}, ${name}!`;
}

console.log(greet());                 // "Hello, Guest!"
console.log(greet("Alice"));         // "Hello, Alice!"
console.log(greet("Bob", "Hi"));     // "Hi, Bob!"
```

### Rest Parameters

```javascript
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3, 4)); // 10
```

### Closures

```javascript
function createCounter() {
  let count = 0;
  return {
    increment: () => ++count,
    decrement: () => --count,
    getCount: () => count
  };
}

const counter = createCounter();
console.log(counter.increment()); // 1
console.log(counter.increment()); // 2
console.log(counter.getCount());  // 2
```

### Higher-Order Functions

```javascript
// Function that takes a function as argument
function repeat(fn, times) {
  for (let i = 0; i < times; i++) {
    fn(i);
  }
}

repeat((i) => console.log(`Iteration: ${i}`), 3);

// Function that returns a function
function multiplier(factor) {
  return (number) => number * factor;
}

const double = multiplier(2);
const triple = multiplier(3);

console.log(double(5));  // 10
console.log(triple(5));  // 15
```

### IIFE (Immediately Invoked Function Expression)

```javascript
(function() {
  const privateVar = "I'm private";
  console.log(privateVar);
})();

// Arrow function IIFE
(() => {
  console.log("Executed immediately");
})();
```

### Async Functions

```javascript
async function fetchUser(id) {
  try {
    const response = await fetch(`/api/users/${id}`);
    const user = await response.json();
    return user;
  } catch (error) {
    console.error("Failed to fetch user:", error);
    throw error;
  }
}

// Async arrow function
const getUser = async (id) => {
  const response = await fetch(`/api/users/${id}`);
  return response.json();
};
```

### Generator Functions

```javascript
function* numberGenerator() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = numberGenerator();
console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }
console.log(gen.next()); // { value: 3, done: false }
console.log(gen.next()); // { value: undefined, done: true }
```

## Common Use Cases

1. **Code reuse** - Avoid repeating the same logic
2. **Abstraction** - Hide complex implementation details
3. **Event handling** - Respond to user interactions
4. **Data transformation** - Process and format data
5. **Callback patterns** - Asynchronous operations

## Common Mistakes

1. **Using `this` in arrow functions** - Arrow functions don't have their own `this`
2. **Forgetting to return** - Functions return `undefined` by default
3. **Not handling edge cases** - Check for null/undefined parameters
4. **Overusing global functions** - Keep functions scoped and modular

```javascript
// WRONG: Arrow function in class method
class Timer {
  constructor() {
    this.seconds = 0;
    setInterval(() => {
      this.seconds++; // 'this' works in arrow function
    }, 1000);
  }
}

// WRONG: Regular function loses 'this'
class Timer {
  constructor() {
    this.seconds = 0;
    setInterval(function() {
      this.seconds++; // 'this' is undefined
    }, 1000);
  }
}
```

## Quick Revision Summary

- Functions are reusable code blocks that perform specific tasks
- Three types: declaration, expression, and arrow functions
- Arrow functions have lexical `this` binding
- Default parameters provide fallback values
- Closures allow functions to remember their creation scope
- Higher-order functions take or return functions
- Async functions return promises

## Related Topics

- [[Arrow-Functions]]
- [[Closures]]
- [[Higher-Order-Functions]]
- [[Async-Await]]
- [[Promises]]
- [[Rest-Parameters]]
- [[ES6-Features]]
