# How to Declare Variables with let

## Basic Syntax

```javascript
// Declaration
let name;

// Declaration + Assignment
let name = "John";

// Multiple declarations
let x, y, z;
let a = 1, b = 2, c = 3;
```

## let Characteristics

```javascript
// 1. Block scoped
function test() {
    if (true) {
        let x = 10;
    }
    console.log(x); // ReferenceError!
}

// 2. NOT hoisted (temporal dead zone)
console.log(name); // ReferenceError!
let name = "John";

// 3. Cannot redeclare
let x = 10;
let x = 20; // SyntaxError!
```

## Block Scope

```javascript
// let is block scoped
for (let i = 0; i < 5; i++) {
    // i is only accessible here
}
console.log(i); // ReferenceError!

// Perfect for loops
for (let i = 0; i < 10; i++) {
    setTimeout(() => console.log(i), 1000);
}
// Output: 0, 1, 2, 3, 4, 5, 6, 7, 8, 9
```

## Reassignment

```javascript
let count = 0;
count = count + 1; // ✅ OK
count += 1;        // ✅ OK
count++;           // ✅ OK
```

## Quick Revision

- `let` = modern variable declaration
- Block scoped (only accessible in `{}`)
- Cannot redeclare in same scope
- Use when value needs to change
- Default choice for mutable variables

---

## Related Topics

- [[What-is-Variable]] - [[What-is-Variable|Variables]] overview
- [[Declare-Var]] - [[Declare-Var|var]] keyword
- [[Declare-Const]] - [[Declare-Const|const]] keyword
- [[What-is-Hoisting]] - [[What-is-Hoisting|Hoisting]]
- [[What-is-Scope]] - [[What-is-Scope|Scope]]
