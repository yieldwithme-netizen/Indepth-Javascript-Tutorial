# What is let and const?

## Definition

- `let` declares a **reassignable** variable with block scope
- `const` declares a **non-reassignable** variable with block scope

## let

```javascript
let name = "John";
name = "Jane"; // ✅ OK

let age;
age = 30; // ✅ OK (can declare then assign)

// Block scoped
if (true) {
    let x = 10;
}
console.log(x); // ReferenceError!
```

## const

```javascript
const PI = 3.14;
PI = 3.14159; // ❌ TypeError!

// Must initialize
const x; // ❌ SyntaxError!

// Object properties can change
const person = { name: "John" };
person.name = "Jane"; // ✅ OK (not reassigning)
person = {};          // ❌ TypeError!
```

## When to Use Each

```javascript
// Use const by default
const name = "John";
const API_URL = "https://api.example.com";

// Use let when reassigning
let count = 0;
count = count + 1;

// Never use var
```

## Quick Revision

- `let`: reassignable, block scoped
- `const`: non-reassignable, block scoped
- Default to `const`, use `let` when needed
- Never use `var`
- Both are block scoped

---

## Related Topics

- [[What-is-LetConst]] - let/const overview
- [[Use-LetConst]] - Using let/const
- [[What-is-Variable]] - Variables
- [[Declare-Let]] - let
- [[Declare-Const]] - const
