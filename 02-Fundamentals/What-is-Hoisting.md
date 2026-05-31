# What is Hoisting?

## Definition

Hoisting is JavaScript's behavior of **moving declarations to the top** of their [[What-is-Variable|scope]] during compilation, before code execution.

## How Hoisting Works

```javascript
// Variable hoisting
console.log(x); // undefined (not error!)
var x = 10;

// This is interpreted as:
// var x;           (declaration hoisted)
// console.log(x);  (undefined)
// x = 10;          (assignment stays)
```

## Hoisting Rules

### var - Hoisted and Initialized

```javascript
console.log(name); // undefined
var name = "John";

// Equivalent to:
var name;
console.log(name); // undefined
name = "John";
```

### let/const - Hoisted but NOT Initialized

```javascript
console.log(age); // ReferenceError: Cannot access before initialization
let age = 30;

// let/const are in "temporal dead zone" until declaration
```

### Functions - Fully Hoisted

```javascript
// Function declaration - fully hoisted
greet(); // ✅ Works! "Hello"

function greet() {
    console.log("Hello");
}

// Function expression - NOT hoisted
sayHi(); // ❌ TypeError: sayHi is not a function

var sayHi = function() {
    console.log("Hi");
};
```

## Temporal Dead Zone (TDZ)

```javascript
// TDZ starts at beginning of scope
// Ends at declaration

{
    // TDZ starts here
    console.log(x); // ReferenceError
    // TDZ ends here
    let x = 10;
}
```

## Hoisting Summary

| Type | Hoisted? | Initialized? | Example |
|------|----------|--------------|---------|
| `var` | ✅ | ✅ (undefined) | `var x` |
| `let` | ✅ | ❌ (TDZ) | `let x` |
| `const` | ✅ | ❌ (TDZ) | `const x` |
| Function declaration | ✅ | ✅ | `function x() {}` |
| Function expression | ❌ | ❌ | `var x = function() {}` |
| Arrow function | ❌ | ❌ | `var x = () => {}` |

## Quick Revision

- Hoisting = declarations moved to top
- `var` → undefined, `let/const` → ReferenceError
- Function declarations fully hoisted
- Function expressions NOT hoisted
- Always declare variables at top of scope

---

## Related Topics

- [[What-is-Variable]] - Variables
- [[Declare-Var]] - var keyword
- [[Declare-Let]] - let keyword
- [[Declare-Const]] - const keyword
- [[What-is-Scope]] - Scope concept