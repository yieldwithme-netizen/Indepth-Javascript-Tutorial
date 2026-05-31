# What is Default Parameter?

## Definition

Default parameters set **fallback values** when arguments are not provided.

## Basic Syntax

```javascript
function greet(name = "Guest") {
    return `Hello, ${name}!`;
}

greet("John"); // "Hello, John!"
greet();       // "Hello, Guest!"
```

## Multiple Defaults

```javascript
function createUser(name = "Anonymous", age = 0, active = true) {
    return { name, age, active };
}

createUser(); // { name: "Anonymous", age: 0, active: true }
createUser("John"); // { name: "John", age: 0, active: true }
```

## Default with Expressions

```javascript
function greet(name = getDefaultName()) {
    return `Hello, ${name}!`;
}

function getDefaultName() {
    return "Guest";
}
```

## Default vs Optional

```javascript
// With defaults (cleaner)
function greet(name = "Guest") {
    return `Hello, ${name}!`;
}

// With manual check (verbose)
function greet(name) {
    name = name || "Guest";
    return `Hello, ${name}!`;
}
```

## Quick Revision

- Default parameters: `param = value`
- Used when argument is undefined
- Can be expressions or function calls
- Cleaner than manual checking
- Works with destructuring

---

## Related Topics

- [[What-is-DefaultParam]] - Default parameters overview
- [[Set-Defaults]] - Setting defaults
- [[What-is-Parameter]] - Parameters
- [[What-is-RestParam]] - Rest parameters
- [[What-is-Function]] - Functions
