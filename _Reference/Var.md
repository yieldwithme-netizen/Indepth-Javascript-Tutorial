# The `var` Keyword

## Definition
`var` is the original variable declaration keyword in JavaScript. It declares a function-scoped or globally-scoped variable, optionally initializing it with a value. In modern JavaScript, `var` is largely replaced by `let` and `const`.

## Declaration and Initialization

```javascript
// Declaration
var name;

// Declaration with initialization
var name = "John";

// Multiple declarations
var x = 1, y = 2, z = 3;

// Reassignment
var count = 0;
count = 1;  // OK

// Redeclaration
var count = 0;
var count = 1;  // OK (unique to var)
```

## Function Scope

```javascript
function example() {
  var x = 10;

  if (true) {
    var y = 20;  // Same scope as x
    console.log(x);  // 10
    console.log(y);  // 20
  }

  console.log(x);  // 10
  console.log(y);  // 20 (still accessible!)
}

example();
```

## Global Scope

```javascript
// var at global scope creates window property
var globalVar = "I'm global";

console.log(window.globalVar);  // "I'm global"
console.log(globalThis.globalVar);  // "I'm global"

// let/const do NOT create window properties
let globalLet = "I'm global";
console.log(window.globalLet);  // undefined
```

## Hoisting Behavior

```javascript
// Variables are hoisted (moved to top)
console.log(name);  // undefined (not ReferenceError)
var name = "John";

// Equivalent to:
var name;
console.log(name);  // undefined
name = "John";

// Functions are also hoisted
greet();  // Works!
function greet() {
  console.log("Hello!");
}

// But function expressions are not
// sayGoodbye();  // ReferenceError
var sayGoodbye = function() {
  console.log("Goodbye!");
};
```

## Loop Behavior

```javascript
// Problem with var in loops
for (var i = 0; i < 3; i++) {
  setTimeout(() => {
    console.log(i);
  }, 1000);
}
// Output: 3, 3, 3 (not 0, 1, 2)

// Because var is function-scoped, all callbacks share same i

// Solution with let
for (let j = 0; j < 3; j++) {
  setTimeout(() => {
    console.log(j);
  }, 1000);
}
// Output: 0, 1, 2
```

## Historical Context

```javascript
// Before ES6, var was the only way to declare variables
// This led to common issues:

// 1. Accidental globals
function createCounter() {
  count = 0;  // Creates global variable!
  return {
    increment: () => count++,
    getCount: () => count
  };
}

// 2. Unexpected scope
var x = "global";
function test() {
  if (true) {
    var x = "local";  // Shadows global x
  }
  console.log(x);  // "local" (surprising!)
}

// 3. Loop variable issues
var buttons = document.querySelectorAll('button');
for (var i = 0; i < buttons.length; i++) {
  buttons[i].addEventListener('click', function() {
    console.log(i);  // Always shows buttons.length
  });
}
```

## var vs let vs const

| Feature | var | let | const |
|---------|-----|-----|-------|
| Scope | Function | Block | Block |
| Hoisting | Yes | Yes (TDZ) | Yes (TDZ) |
| Redeclaration | Yes | No | No |
| Reassignment | Yes | Yes | No |
| Global property | Yes | No | No |

## When to Use var

```javascript
// Legacy code compatibility
function legacyFunction() {
  var result = calculateSomething();
  return result;
}

// When you need function scope
function processData() {
  var cache = {};

  if (condition) {
    var temp = getTempData();
    cache.temp = temp;
  }

  return cache;
}
```

## Common Use Cases
- Legacy codebases
- Function-scoped variables
- When you need redeclaration
- Compatibility with older browsers

## Common Mistakes

| Mistake | Solution |
|---------|----------|
| Using var in modern code | Use let/const instead |
| Not understanding scope | Learn block vs function scope |
| Accidental globals | Always declare variables |
| Loop variable capture | Use let in for loops |

## Quick Revision Summary
- `var` is function-scoped, not block-scoped
- Variables are hoisted and initialized to undefined
- Redeclaration is allowed
- Creates global properties at global scope
- Avoid in modern JavaScript; use let/const instead
- Still valid but considered legacy

## Related Topics
- [[Variables]]
- [[Scope]]
- [[Hoisting]]
- [[Temporal-Dead-Zone]]
- [[Block-Scope]]
- [[Function-Scope]]
- [[let-keyword]]
- [[const-keyword]]
