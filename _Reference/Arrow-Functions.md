# Arrow-Functions

## Definition

Arrow functions provide a **shorter syntax** for writing functions introduced in ES6.

## Syntax

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

## this in Arrow Functions

```javascript
// Arrow function: no own this
const person = {
    name: "John",
    greet: () => {
        return `Hello, ${this.name}!`; // undefined
    }
};

// Regular function: own this
const person = {
    name: "John",
    greet() {
        return `Hello, ${this.name}!`; // "John"
    }
};
```

## Quick Revision

- Arrow: `() => {}` syntax
- Shorter than regular functions
- No own `this`
- No `arguments` object
- Use for callbacks, avoid for methods

---

## Related Topics

- [[What-is-Arrow]] - [[What-is-Arrow|Arrow functions]]
- [[Arrow Functions]] - [[Arrow Functions|Arrow functions]]
- [[What-is-Function]] - [[What-is-Function|Functions]]
- [[What-is-This]] - [[What-is-This|this keyword]]
