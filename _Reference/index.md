# JavaScript Tutorial Index

## Welcome

Welcome to the JavaScript Tutorial! This index provides a comprehensive guide to learning JavaScript from fundamentals to advanced concepts.

---

## Tutorial Contents

### Fundamentals
| Topic | Description |
|-------|-------------|
| [[JavaScript]] | Overview of the JavaScript language |
| [[function]] | Functions, declarations, expressions, and arrow functions |
| [[let]] | Block-scoped variable declarations with `let` |
| [[Logical-Operators]] | AND, OR, NOT, and logical operators |

### Functions & Scope
| Topic | Description |
|-------|-------------|
| [[function]] | Function declarations, expressions, and arrow functions |
| [[Function-Scope-and-Closures]] | Understanding scope chains and closures |
| [[IIFE]] | Immediately Invoked Function Expressions |

### Data & Storage
| Topic | Description |
|-------|-------------|
| [[Local-Storage]] | Web Storage API for client-side data persistence |
| [[Length]] | Array and String length properties |

### Documentation
| Topic | Description |
|-------|-------------|
| [[JSDoc]] | Documenting JavaScript code with JSDoc comments |

---

## Learning Path

### Beginner Level
1. [[JavaScript]] - Start with the language overview
2. [[function]] - Learn function basics
3. [[let]] - Understand variable scoping
4. [[Logical-Operators]] - Master conditional logic

### Intermediate Level
5. [[Function-Scope-and-Closures]] - Deep dive into closures
6. [[IIFE]] - Learn immediate function execution
7. [[Length]] - Work with arrays and strings
8. [[Local-Storage]] - Persist data in the browser

### Advanced Level
9. [[JSDoc]] - Document your code professionally

---

## Quick Reference

### Variable Declarations
```javascript
var x = 10;    // Function scoped (avoid in modern JS)
let y = 20;    // Block scoped
const z = 30;  // Block scoped, cannot reassign
```

### Function Types
```javascript
function regular() {}       // Declaration (hoisted)
const expr = function() {}  // Expression (not hoisted)
const arrow = () => {}      // Arrow function (no own this)
```

### Logical Operators
```javascript
true && false   // AND: false
true || false   // OR: true
!true           // NOT: false
```

---

## Common Patterns

### Closure Pattern
```javascript
function createCounter() {
  let count = 0;
  return {
    increment: () => ++count,
    getCount: () => count
  };
}
```

### Module Pattern (IIFE)
```javascript
const Module = (function() {
  const private = 'secret';
  return {
    public: () => private
  };
})();
```

### LocalStorage Pattern
```javascript
// Save
localStorage.setItem('key', JSON.stringify(data));

// Load
const data = JSON.parse(localStorage.getItem('key'));
```

---

## Related Topics

- [[JavaScript]] - Language overview
- [[function]] - Function fundamentals
- [[Function-Scope-and-Closures]] - Scope and closures
- [[IIFE]] - Immediately invoked functions
- [[let]] - Block scoping
- [[Local-Storage]] - Client-side storage
- [[Length]] - Array/String length
- [[Logical-Operators]] - Logical operations
- [[JSDoc]] - Code documentation
