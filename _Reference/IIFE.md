# IIFE (Immediately Invoked Function Expression)

## Definition

An **IIFE** (Immediately Invoked Function Expression) is a function that is defined and executed immediately after its creation. It is a design pattern used to create a new scope to avoid polluting the global namespace and to execute code once.

IIFEs are created by wrapping a function expression in parentheses and then calling it immediately with `()`.

---

## Syntax

### Standard IIFE
```javascript
(function() {
  // Code here runs immediately
})();
```

### Named IIFE
```javascript
(function myIIFE() {
  // Code here runs immediately
  console.log('IIFE executed');
})();
```

### Arrow Function IIFE
```javascript
(() => {
  // Code here runs immediately
})();
```

### With Parameters
```javascript
(function(name) {
  console.log(`Hello, ${name}!`);
})('Alice');
```

---

## Code Examples

### Basic IIFE
```javascript
(function() {
  const message = 'This is private to the IIFE';
  console.log(message);
})();

// console.log(message); // Error: message is not defined
```

### IIFE with Return Value
```javascript
const result = (function() {
  const x = 10;
  const y = 20;
  return x + y;
})();

console.log(result); // Output: 30
```

### IIFE for Initialization
```javascript
const app = (function() {
  const config = {
    apiUrl: 'https://api.example.com',
    timeout: 5000
  };

  function init() {
    console.log('App initialized with config:', config);
  }

  init();

  return { config, init };
})();

console.log(app.config.apiUrl); // Output: https://api.example.com
```

### IIFE to Avoid Global Namespace Pollution
```javascript
// Without IIFE - pollutes global scope
var libraryName = 'My Library';
function doSomething() { /* ... */ }

// With IIFE - keeps everything private
(function() {
  var libraryName = 'My Library';
  function doSomething() { /* ... */ }

  // Expose only what's needed
  window.MyLibrary = { doSomething };
})();
```

### Async IIFE
```javascript
(async function() {
  try {
    const response = await fetch('https://api.example.com/data');
    const data = await response.json();
    console.log(data);
  } catch (error) {
    console.error('Error:', error);
  }
})();

// Arrow function version
(async () => {
  const result = await someAsyncOperation();
  console.log(result);
})();
```

### IIFE in Loops
```javascript
// Fix the classic closure problem
for (var i = 0; i < 5; i++) {
  (function(j) {
    setTimeout(function() {
      console.log(j);
    }, 100);
  })(i);
}
// Output: 0, 1, 2, 3, 4
```

### IIFE for Module Pattern
```javascript
const Counter = (function() {
  let count = 0;

  function increment() { count++; }
  function decrement() { count--; }
  function getCount() { return count; }

  return { increment, decrement, getCount };
})();

Counter.increment();
Counter.increment();
console.log(Counter.getCount()); // Output: 2
```

### jQuery-style IIFE
```javascript
(function($) {
  // Use $ without conflicts
  $(document).ready(function() {
    console.log('jQuery ready');
  });
})(jQuery);
```

---

## Common Use Cases

| Use Case | Description |
|----------|-------------|
| **Namespace Isolation** | Prevent global variable pollution |
| **Initialization Code** | Run setup code once on page load |
| **Private Variables** | Create private scope for variables |
| **Module Pattern** | Implement module system (pre-ES6) |
| **Loop Closures** | Fix closure issues in loops |
| **Async Operations** | Execute async code immediately |
| **Libraries** | Wrap library code to avoid conflicts |

---

## Common Mistakes

### 1. Missing Parentheses
```javascript
// WRONG - This is a function declaration, not an IIFE
function() {
  console.log('This will cause a syntax error');
}();

// CORRECT
(function() {
  console.log('This works');
})();
```

### 2. Arrow Function IIFE Parentheses
```javascript
// WRONG - Syntax error
() => {
  console.log('Error');
}();

// CORRECT - Wrap in parentheses
(() => {
  console.log('Works');
})();
```

### 3. Confusing IIFE with Function Call
```javascript
// This is NOT an IIFE - it's calling a function named 'functionName'
functionName();

// This IS an IIFE - function expression invoked immediately
(function() {
  // ...
})();
```

---

## Quick Revision Summary

- IIFE is a function that runs immediately after it is defined
- Created using `(function() { })()` or `(() => { })()`
- Creates a new scope to avoid polluting the global namespace
- Useful for initialization, privacy, and module patterns
- Async IIFE allows using `await` at the top level
- Always wrap function expressions in parentheses before invoking

---

## Related Topics

- [[function]] - Function declarations and expressions
- [[Function-Scope-and-Closures]] - How IIFE creates isolated scopes
- [[JavaScript]] - JavaScript language overview
- [[let]] - Block scoping alternatives to IIFE
- [[Local-Storage]] - Storing data outside IIFE scope
