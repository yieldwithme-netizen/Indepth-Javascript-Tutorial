# What is Functional Programming?

## Definition

Functional programming is a **programming paradigm** that treats computation as evaluation of mathematical functions.

## Core Principles

### 1. Pure Functions

```javascript
// Pure (same input → same output)
function add(a, b) {
    return a + b;
}

// Impure (side effect)
let count = 0;
function increment() {
    return ++count; // modifies external state
}
```

### 2. Immutability

```javascript
// Mutable (bad)
const arr = [1, 2, 3];
arr.push(4);

// Immutable (good)
const arr = [1, 2, 3];
const newArr = [...arr, 4];
```

### 3. First-Class Functions

```javascript
// Functions as values
const greet = (name) => `Hello, ${name}!`;

// Passed as arguments
[1, 2, 3].map((n) => n * 2);

// Returned from functions
function createMultiplier(factor) {
    return (n) => n * factor;
}
```

### 4. Higher-Order Functions

```javascript
// Takes function as argument
function repeat(fn, times) {
    for (let i = 0; i < times; i++) {
        fn(i);
    }
}

// Returns function
function compose(f, g) {
    return (x) => f(g(x));
}
```

## Quick Revision

- FP = functions as building blocks
- Pure functions: no side effects
- Immutability: don't change data
- Higher-order functions: take/return functions
- Use: map, filter, reduce, compose

---

## Related Topics

- [[What-is-FP]] - FP overview
- [[What-is-Pure]] - Pure functions
- [[What-is-Immutability]] - Immutability
- [[What-is-HOF]] - Higher-order functions
- [[What-is-Currying]] - Currying
- [[What-is-Composition]] - Composition
