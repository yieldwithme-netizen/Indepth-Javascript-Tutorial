# Const Keyword

## Definition

The `const` keyword declares a block-scoped constant that cannot be reassigned after initialization. It was introduced in ES6 and is the preferred way to declare values that should not change.

## Basic Syntax

```javascript
const PI = 3.14159;
console.log(PI); // 3.14159

// PI = 3.14; // TypeError: Assignment to constant variable.
```

## Block Scope

`const` is block-scoped, meaning it only exists within the nearest set of curly braces:

```javascript
if (true) {
  const message = "Hello!";
  console.log(message); // "Hello!"
}

// console.log(message); // ReferenceError: message is not defined
```

## Immutability vs Mutability

### `const` Prevents Reassignment, Not Mutation

```javascript
// Object - properties CAN be modified
const user = { name: "Alice", age: 25 };
user.age = 26;           // OK - modifying property
user.email = "a@b.com";  // OK - adding new property
// user = {};             // TypeError - cannot reassign

// Array - elements CAN be modified
const colors = ["red", "green"];
colors.push("blue");     // OK - modifying array
colors[0] = "yellow";    // OK - changing element
// colors = [];           // TypeError - cannot reassign
```

### Making Objects Truly Immutable

```javascript
// Freeze the object
const user = Object.freeze({
  name: "Alice",
  age: 25,
});

user.age = 26;  // Silently fails (or throws in strict mode)
console.log(user); // { name: "Alice", age: 25 }

// Deep freeze
function deepFreeze(obj) {
  Object.freeze(obj);
  Object.keys(obj).forEach((key) => {
    if (typeof obj[key] === "object" && !Object.isFrozen(obj[key])) {
      deepFreeze(obj[key]);
    }
  });
  return obj;
}
```

## Hoisting Behavior

`const` is hoisted but not initialized (Temporal Dead Zone):

```javascript
// console.log(x); // ReferenceError: Cannot access 'x' before initialization
const x = 10;

// This is different from var:
console.log(y); // undefined (var is hoisted and initialized)
var y = 20;
```

## When to Use `const`

### Always Use `const` (Default)

```javascript
// Use const for values that don't change
const API_URL = "https://api.example.com";
const MAX_RETRIES = 3;
const config = { debug: true };

// Use const for function references
const handleClick = (event) => { ... };
const calculateTotal = (items) => { ... };
```

### Use `let` When Reassignment is Needed

```javascript
// Only use let when you need to reassign
let counter = 0;
counter++; // OK

let currentuser = null;
currentUser = getUser(); // OK
```

## Const with Arrays

```javascript
const fruits = ["apple", "banana"];

// These work:
fruits.push("orange");      // Add element
fruits.pop();               // Remove last
fruits[0] = "mango";       // Change element

// This doesn't work:
// fruits = ["new", "array"]; // TypeError
```

## Const with Destructuring

```javascript
// Object destructuring
const { name, age } = { name: "Alice", age: 25 };

// Array destructuring
const [first, second] = [10, 20];

// Renaming
const { name: userName } = { name: "Alice" };

// Default values
const { role = "user" } = {};
```

## Const in Loops

```javascript
// Regular for loop - const works for loop variable (reassigned each iteration)
for (const i = 0; i < 5; i++) {
  console.log(i); // 0, 1, 2, 3, 4
}

// forEach - const works
const items = ["a", "b", "c"];
items.forEach((item) => {
  console.log(item);
});
```

## Common Use Cases

### Configuration Objects

```javascript
const CONFIG = {
  apiUrl: "https://api.example.com",
  timeout: 5000,
  retries: 3,
};
```

### Module Exports

```javascript
const helper = {
  format: (str) => str.trim(),
  capitalize: (str) => str.charAt(0).toUpperCase() + str.slice(1),
};

module.exports = helper;
```

### Function Declarations in Modules

```javascript
const express = require("express");
const router = express.Router();

router.get("/", (req, res) => {
  res.json({ message: "Hello" });
});

module.exports = router;
```

## Common Mistakes

| Mistake | Fix |
|---------|-----|
| Using `const` then trying to reassign | Use `let` if reassignment is needed |
| Thinking `const` makes objects immutable | Use `Object.freeze()` for immutability |
| Using `var` for constants | Always prefer `const` over `var` |
| Not using `const` for function refs | Use `const fn = () => {}` instead of `var` |

## Quick Revision

- `const` declares a block-scoped constant
- Cannot be **reassigned** (but objects/arrays can be **mutated**)
- **Always use `const` by default** - switch to `let` only when reassignment is needed
- **Not hoisted** like `var` (Temporal Dead Zone)
- Use `Object.freeze()` for true immutability
- Perfect for configuration, imports, and function references

## Related Topics

- [[Let-Keyword]]
- [[Var-Keyword]]
- [[Block-Scope]]
- [[Hoisting]]
- [[Object-Freeze]]
- [[Destructuring]]
- [[ES6-Features]]
