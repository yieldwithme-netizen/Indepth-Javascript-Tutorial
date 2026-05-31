# Functions in JavaScript

## Definition

A **function** is a reusable block of code designed to perform a particular task. Functions are first-class objects in JavaScript, meaning they can be assigned to variables, passed as arguments, returned from other functions, and have properties and methods.

Functions are defined using the `function` keyword, arrow function syntax (`=>`), or the `Function` constructor.

---

## Syntax

### Function Declaration
```javascript
function greet(name) {
  return `Hello, ${name}!`;
}
```

### Function Expression
```javascript
const greet = function(name) {
  return `Hello, ${name}!`;
};
```

### Arrow Function
```javascript
const greet = (name) => {
  return `Hello, ${name}!`;
};

// Short form (implicit return)
const greet = (name) => `Hello, ${name}!`;
```

### Function Constructor
```javascript
const greet = new Function('name', 'return `Hello, ${name}!`');
```

---

## Code Examples

### Basic Function with Parameters and Return Value
```javascript
function add(a, b) {
  return a + b;
}

console.log(add(5, 3)); // Output: 8
```

### Default Parameters
```javascript
function greet(name = 'Guest') {
  return `Hello, ${name}!`;
}

console.log(greet());       // Output: Hello, Guest!
console.log(greet('John')); // Output: Hello, John!
```

### Rest Parameters
```javascript
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3, 4)); // Output: 10
```

### Higher-Order Function
```javascript
function multiply(factor) {
  return function(number) {
    return number * factor;
  };
}

const double = multiply(2);
console.log(double(5));  // Output: 10
console.log(double(10)); // Output: 20
```

### Recursive Function
```javascript
function factorial(n) {
  if (n <= 1) return 1;
  return n * factorial(n - 1);
}

console.log(factorial(5)); // Output: 120
```

### Arrow Function with Array Methods
```javascript
const numbers = [1, 2, 3, 4, 5];

const doubled = numbers.map(n => n * 2);
const evens = numbers.filter(n => n % 2 === 0);
const sum = numbers.reduce((acc, n) => acc + n, 0);

console.log(doubled); // Output: [2, 4, 6, 8, 10]
console.log(evens);   // Output: [2, 4]
console.log(sum);     // Output: 15
```

### Closures in Functions
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
counter.increment();
counter.increment();
console.log(counter.getCount()); // Output: 2
```

---

## Common Use Cases

| Use Case | Description |
|----------|-------------|
| **Code Reusability** | Write once, use multiple times |
| **Abstraction** | Hide complex logic behind simple interfaces |
| **Modularity** | Break code into manageable pieces |
| **Callbacks** | Pass functions as arguments for async operations |
| **Event Handling** | Execute code when events occur |
| **Data Transformation** | Process and transform data collections |

---

## Common Mistakes

### 1. Hoisting Confusion
```javascript
// Function declarations are hoisted
greet(); // Works!

function greet() {
  console.log('Hello');
}

// Function expressions are NOT hoisted
sayHi(); // Error: sayHi is not a function

var sayHi = function() {
  console.log('Hi');
};
```

### 2. Arrow Functions and `this`
```javascript
const obj = {
  name: 'Alice',
  greet: function() {
    // Regular function: 'this' refers to obj
    setTimeout(function() {
      console.log(`Hello, ${this.name}`);
    }, 100);
  },
  greetArrow: function() {
    // Arrow function: 'this' inherits from outer scope
    setTimeout(() => {
      console.log(`Hello, ${this.name}`);
    }, 100);
  }
};

obj.greet();       // Output: Hello, undefined
obj.greetArrow();  // Output: Hello, Alice
```

### 3. Not Returning Explicitly
```javascript
function add(a, b) {
  a + b; // Missing return statement!
}

console.log(add(5, 3)); // Output: undefined
```

---

## Quick Revision Summary

- Functions are reusable blocks of code that perform specific tasks
- Three main ways to define functions: declarations, expressions, and arrow functions
- Function declarations are hoisted; expressions and arrows are not
- Arrow functions do not have their own `this` binding
- Functions are first-class objects — they can be passed as arguments and returned
- Default parameters and rest parameters provide flexible argument handling
- Closures allow functions to access variables from their outer scope

---

## Related Topics

- [[Function-Scope-and-Closures]] - Understanding scope and closure behavior
- [[IIFE]] - Immediately Invoked Function Expressions
- [[let]] - Block scoping with let keyword
- [[JavaScript]] - JavaScript language overview
- [[JSDoc]] - Documenting functions with JSDoc comments
