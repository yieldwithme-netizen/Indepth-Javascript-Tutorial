# What is a Function [[Expression]]?

## Definition

A function [[Expression]] is a [[Function]] **assigned to a [[Variable]]**.

## Syntax

```javascript
// Named function expression
const greet = function greet(name) {
    return `Hello, ${name}!`;
};

// Anonymous function expression
const greet = function(name) {
    return `Hello, ${name}!`;
};
```

## Function Declaration vs [[Expression]]

```javascript
// Declaration (hoisted)
greet(); // Works
function greet() {
    console.log("Hello");
}

// Expression (NOT hoisted)
sayHi(); // Error!
const sayHi = function() {
    console.log("Hi");
};
```

## [[IIFE]] (Immediately Invoked)

```javascript
// Runs immediately
(function() {
    console.log("Runs immediately!");
})();

// With parameters
(function(name) {
    console.log(`Hello, ${name}!`);
})("John");
```

## Quick Revision

- Function [[Expression]] = [[Function]] assigned to [[Variable]]
- NOT hoisted (must declare before use)
- Can be named or anonymous
- [[IIFE]] = runs immediately
- Often used for [[Callback]]s

---

## Related Topics

- [[What-is-Function]] - Functions
- [[Write-Expression]] - Writing expressions
- [[What-is-IIFE]] - IIFE
- [[Declare-Function]] - Function declarations
- [[What-is-Callback]] - Callbacks