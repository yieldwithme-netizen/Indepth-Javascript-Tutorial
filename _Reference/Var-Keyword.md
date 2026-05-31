# Var Keyword

## Definition

`var` is the **original way** to declare variables in JavaScript (pre-ES6).

## Characteristics

```javascript
// Function scoped (NOT block scoped)
function test() {
    if (true) {
        var x = 10;
    }
    console.log(x); // 10 (accessible!)
}

// Hoisted
console.log(name); // undefined
var name = "John";

// Can redeclare
var x = 10;
var x = 20; // No error
```

## Why Avoid var

```javascript
// ❌ Problem: leaks out of blocks
for (var i = 0; i < 5; i++) {}
console.log(i); // 5 (leaked!)

// ❌ Problem: hoisting confusion
console.log(x); // undefined
var x = 10;

// ✅ Use let/const instead
for (let i = 0; i < 5; i++) {}
console.log(i); // ReferenceError
```

## Quick Revision

- `var` = old way
- Function scoped (not block)
- Hoisted to top
- Can redeclare
- Never use in modern code

---

## Related Topics

- [[Var-Keyword]] - [[Var-Keyword|var keyword]]
- [[Var]] - [[Var|var]]
- [[Declare-Var]] - [[Declare-Var|var]]
- [[What-is-Hoisting]] - [[What-is-Hoisting|Hoisting]]
- [[What-is-Scope]] - [[What-is-Scope|Scope]]
