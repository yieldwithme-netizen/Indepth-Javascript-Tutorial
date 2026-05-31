# How to Initialize Variables

## Declaration vs Initialization

```javascript
// Declaration (creates the variable)
let name;
console.log(name); // undefined

// Initialization (assigns a value)
name = "John";
console.log(name); // "John"

// Declaration + Initialization
let age = 30;
console.log(age); // 30
```

## Default Values

```javascript
// Uninitialized variables
let x;
console.log(x); // undefined

// Different types
let str;
console.log(str); // undefined

let num;
console.log(num); // undefined

let bool;
console.log(bool); // undefined
```

## Initialization Patterns

```javascript
// Pattern 1: Declare then assign
let name;
name = "John";

// Pattern 2: Declare and assign
let name = "John";

// Pattern 3: Multiple declarations
let x = 1, y = 2, z = 3;

// Pattern 4: Default values
let count = 0;
let isValid = false;
let name = "Anonymous";
```

## Quick Revision

- Declaration = create variable
- Initialization = assign value
- Uninitialized = `undefined`
- Always initialize before use
- Use meaningful initial values

---

## Related Topics

- [[What-is-Variable]] - [[What-is-Variable|Variables]] overview
- [[Declare-Var]] - [[Declare-Var|var]]
- [[Declare-Let]] - [[Declare-Let|let]]
- [[Declare-Const]] - [[Declare-Const|const]]
- [[What-is-Undefined]] - [[What-is-Undefined|undefined]]
