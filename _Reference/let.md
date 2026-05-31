# let Keyword

## Definition

The **`let`** keyword declares a block-scoped local variable in JavaScript. Unlike `var` (which is function-scoped), `let` is scoped to the nearest enclosing block (delimited by `{ }`). Variables declared with `let` can be reassigned but cannot be redeclared in the same scope.

`let` was introduced in ES6 (2015) and is the preferred way to declare variables that will be reassigned.

---

## Syntax

```javascript
let variableName = value;
let name;           // undefined until assigned
let x = 10, y = 20; // multiple declarations
```

---

## Code Examples

### Basic Declaration and Assignment
```javascript
let count = 0;
console.log(count); // Output: 0

count = 10; // Reassignment allowed
console.log(count); // Output: 10
```

### Block Scope
```javascript
if (true) {
  let x = 10;
  console.log(x); // Output: 10
}

// console.log(x); // Error: x is not defined
```

### Loop Scope
```javascript
for (let i = 0; i < 5; i++) {
  console.log(i); // Output: 0, 1, 2, 3, 4
}

// console.log(i); // Error: i is not defined
```

### vs var (Function Scope)
```javascript
// var is function scoped
function varExample() {
  if (true) {
    var x = 10;
  }
  console.log(x); // Output: 10 - var leaks out
}

// let is block scoped
function letExample() {
  if (true) {
    let y = 10;
  }
  // console.log(y); // Error: y is not defined
}
```

### Hoisting Behavior
```javascript
// var is hoisted and initialized
console.log(a); // Output: undefined (no error)
var a = 5;

// let is hoisted but NOT initialized (Temporal Dead Zone)
// console.log(b); // ReferenceError: Cannot access 'b' before initialization
let b = 5;
```

### Cannot Redeclare
```javascript
let x = 10;
// let x = 20;  // SyntaxError: Identifier 'x' has already been declared

x = 20; // Reassignment is fine
console.log(x); // Output: 20
```

### In For Loops (Classic Problem Solved)
```javascript
// var - closure problem
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Output: 3, 3, 3

// let - each iteration has its own scope
for (let i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
// Output: 0, 1, 2
```

### Destructuring with let
```javascript
let [a, b, ...rest] = [1, 2, 3, 4, 5];
console.log(a);    // Output: 1
console.log(b);    // Output: 2
console.log(rest); // Output: [3, 4, 5]

let { name, age } = { name: 'Alice', age: 30 };
console.log(name); // Output: Alice
console.log(age);  // Output: 30
```

### Let with Switch Statements
```javascript
const day = 'Monday';

switch (day) {
  case 'Monday':
    let message = 'Start of the week';
    console.log(message);
    break;
  case 'Friday':
    // let message = 'Almost weekend'; // Error: can't redeclare
    message = 'Almost weekend'; // Reassignment works
    console.log(message);
    break;
}
```

### Let in Closures
```javascript
function createCounters() {
  const counters = [];

  for (let i = 0; i < 5; i++) {
    counters.push(() => i);
  }

  return counters;
}

const counters = createCounters();
console.log(counters[0]()); // Output: 0
console.log(counters[1]()); // Output: 1
console.log(counters[4]()); // Output: 4
```

### Global let vs Window
```javascript
var globalVar = 'var';
let globalLet = 'let';

console.log(window.globalVar); // Output: var
console.log(window.globalLet); // Output: undefined (let doesn't create window property)
```

---

## Common Use Cases

| Use Case | Description |
|----------|-------------|
| **Loop Counters** | Block-scoped iteration variables |
| **Conditional Variables** | Variables only needed in if blocks |
| **Temporary Values** | Variables used in specific code blocks |
| **Closures in Loops** | Each iteration has its own variable |
| **Destructuring** | Extract values from arrays/objects |
| **Reassignable Variables** | Variables that change value |

---

## Common Mistakes

### 1. Using let in Loops Incorrectly
```javascript
// WRONG - Using let with increment in for-of
for (let i of [1, 2, 3]) {
  // This works fine, but be aware of scope
  console.log(i);
}

// CORRECT - Standard usage
for (let i = 0; i < 3; i++) {
  console.log(i);
}
```

### 2. Confusing let with const
```javascript
// let allows reassignment
let x = 10;
x = 20; // Works

// const does not
const y = 10;
// y = 20; // Error: Assignment to constant variable
```

### 3. TDZ (Temporal Dead Zone) Confusion
```javascript
function example() {
  console.log(x); // ReferenceError!
  let x = 10;
}

// TDZ is the period between block entry and declaration
function correct() {
  let x;
  console.log(x); // Output: undefined (after declaration, before assignment)
  x = 10;
}
```

---

## Quick Revision Summary

- `let` declares a block-scoped variable
- Cannot be redeclared in the same scope but can be reassigned
- Not hoisted like `var` — subject to Temporal Dead Zone (TDZ)
- Solves the classic for-loop closure problem
- Does not create properties on the global `window` object
- Use `let` for variables that will be reassigned
- Use `const` for variables that won't be reassigned

---

## Related Topics

- [[function]] - Functions and variable scoping
- [[Function-Scope-and-Closures]] - Scope chains and closures
- [[IIFE]] - IIFE as alternative to let for scoping
- [[JavaScript]] - JavaScript language overview
- [[Logical-Operators]] - Using let in conditional logic
