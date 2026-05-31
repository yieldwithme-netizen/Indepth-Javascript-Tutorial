# What is an Arrow Function?

## Definition

An arrow function is a **shorthand syntax** for writing functions introduced in ES6.

## Basic Syntax

```javascript
// Regular function
function greet(name) {
    return `Hello, ${name}!`;
}

// Arrow function
const greet = (name) => `Hello, ${name}!`;

// No parameters
const sayHello = () => "Hello!";

// One parameter (optional parentheses)
const double = x => x * 2;
```

## Arrow Functions vs Regular Functions

```javascript
// Arrow function
const add = (a, b) => a + b;

// Regular function
function add(a, b) {
    return a + b;
}
```

## this in Arrow Functions

```javascript
// Arrow function: no own this
const person = {
    name: "John",
    greet: () => {
        // this refers to outer scope (window)
        return `Hello, ${this.name}!`; // undefined!
    }
};

// Regular function: own this
const person = {
    name: "John",
    greet() {
        // this refers to person
        return `Hello, ${this.name}!`; // "Hello, John!"
    }
};
```

## When to Use

```javascript
// ✅ Use arrow for:
// - Callbacks
const numbers = [1, 2, 3].map(x => x * 2);

// - Short functions
const double = x => x * 2;

// ❌ Don't use arrow for:
// - Object methods
const obj = {
    method() {} // ✅
    method: () => {} // ❌
};

// - Constructors
class Person {} // ✅
const Person = () => {} // ❌
```

## Quick Revision

- Arrow function: `() => {}` syntax
- Shorter than regular functions
- No own `this` (inherits from parent)
- No `arguments` object
- Use for callbacks, avoid for methods/constructors

---

## Related Topics

- [[What-is-Arrow]] - [[What-is-Arrow|Arrow functions]] overview
- [[Write-Arrow]] - [[Write-Arrow|Writing arrow functions]]
- [[What-is-Function]] - [[What-is-Function|Functions]]
- [[What-is-This]] - [[What-is-This|this keyword]]
- [[What-is-Scope]] - [[What-is-Scope|Scope]]
