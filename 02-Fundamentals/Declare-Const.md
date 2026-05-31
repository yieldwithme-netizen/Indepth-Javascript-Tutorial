# How to Declare Variables with const

## Basic Syntax

```javascript
// Declaration + Assignment (required!)
const name = "John";

// const requires initialization
const x; // SyntaxError!
```

## const Characteristics

```javascript
// 1. Block scoped
function test() {
    if (true) {
        const x = 10;
    }
    console.log(x); // ReferenceError!
}

// 2. Cannot reassign
const PI = 3.14;
PI = 3.14159; // TypeError!

// 3. But objects/arrays can be mutated
const arr = [1, 2, 3];
arr.push(4); // ✅ OK
console.log(arr); // [1, 2, 3, 4]

const obj = { name: "John" };
obj.name = "Jane"; // ✅ OK
obj = {}; // TypeError!
```

## When to Use const

```javascript
// ✅ Use const for:
const API_URL = "https://api.example.com";
const MAX_SIZE = 100;
const PI = 3.14159;

// ✅ Use const for arrays/objects (reference not reassigned)
const users = [];
const config = {};

// ❌ Don't use const for:
let count = 0; // Will be reassigned
let isValid = false; // Will change
```

## Quick Revision

- `const` = constant (cannot reassign)
- Must initialize when declaring
- Block scoped like `let`
- Objects/arrays can be mutated (properties change)
- Default choice for all variables

---

## Related Topics

- [[What-is-Variable]] - [[What-is-Variable|Variables]] overview
- [[Declare-Var]] - [[Declare-Var|var]] keyword
- [[Declare-Let]] - [[Declare-Let|let]] keyword
- [[What-is-Hoisting]] - [[What-is-Hoisting|Hoisting]]
- [[What-is-Scope]] - [[What-is-Scope|Scope]]
