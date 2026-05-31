# How to Declare Variables with var

## Basic Syntax

```javascript
// Declaration
var name;

// Declaration + Assignment
var name = "John";

// Multiple declarations
var x, y, z;
var a = 1, b = 2, c = 3;
```

## var Characteristics

```javascript
// 1. Function scoped (NOT block scoped)
function test() {
    if (true) {
        var x = 10;
    }
    console.log(x); // 10 (accessible!)
}

// 2. Hoisted (moved to top)
console.log(name); // undefined (not error!)
var name = "John";

// 3. Can redeclare
var x = 10;
var x = 20; // No error!
```

## Why Avoid var

```javascript
// ❌ Problem 1: Function scope leaks
for (var i = 0; i < 5; i++) {}
console.log(i); // 5 (leaked!)

// ❌ Problem 2: Redeclaration
var x = 10;
var x = 20; // No error, but confusing

// ❌ Problem 3: Hoisting confusion
console.log(x); // undefined
var x = 10;
```

## Use let/const Instead

```javascript
// ✅ Better: let (block scoped, reassignable)
let count = 0;
count = count + 1;

// ✅ Better: const (block scoped, not reassignable)
const API_URL = "https://api.example.com";
```

## Quick Revision

- `var` = old way, function scoped
- Hoisted to top of function
- Can redeclare (no error)
- Use `let` or `const` instead
- Never use `var` in modern code

---

## Related Topics

- [[What-is-Variable]] - [[What-is-Variable|Variables]] overview
- [[Declare-Let]] - [[Declare-Let|let]] keyword
- [[Declare-Const]] - [[Declare-Const|const]] keyword
- [[What-is-Hoisting]] - [[What-is-Hoisting|Hoisting]]
- [[What-is-Scope]] - [[What-is-Scope|Scope]]
