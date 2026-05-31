# Hoisting

## Definition

Hoisting moves **declarations to the top** of their scope during compilation.

## Variable Hoisting

```javascript
console.log(x); // undefined (not error!)
var x = 10;

// Interpreted as:
var x;
console.log(x); // undefined
x = 10;
```

## let/const Hoisting

```javascript
console.log(y); // ReferenceError!
let y = 10;

// let/const are in TDZ until declaration
```

## Function Hoisting

```javascript
greet(); // Works!
function greet() {
    console.log("Hello");
}

sayHi(); // Error!
var sayHi = function() {};
```

## Quick Revision

- `var` → undefined
- `let/const` → ReferenceError (TDZ)
- Function declarations fully hoisted
- Function expressions NOT hoisted

---

## Related Topics

- [[What-is-Hoisting]] - [[What-is-Hoisting|Hoisting]]
- [[Hoisting]] - [[Hoisting|Hoisting]]
- [[Temporal-Dead-Zone]] - [[Temporal-Dead-Zone|TDZ]]
- [[Declare-Var]] - [[Declare-Var|var]]
