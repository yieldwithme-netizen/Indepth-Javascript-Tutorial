# Partial Application

## Definition

Partial application **fixes some arguments** of a function, returning a new function with fewer parameters.

## Basic Syntax

```javascript
// Without partial application
function add(a, b) {
    return a + b;
}
add(5, 3); // 8

// With partial application (using bind)
const add5 = add.bind(null, 5);
add5(3); // 8
add5(10); // 15
```

## Custom Partial Function

```javascript
function partial(fn, ...args1) {
    return function(...args2) {
        return fn(...args1, ...args2);
    };
}

function add(a, b, c) {
    return a + b + c;
}

const add5 = partial(add, 5);
add5(3, 2); // 10

const add5and3 = partial(add, 5, 3);
add5and3(2); // 10
```

## Partial vs Currying

```javascript
// Partial: fix some args
const add5 = partial(add, 5);
add5(3, 2); // 10

// Currying: one arg at a time
const add = (a) => (b) => (c) => a + b + c;
add(5)(3)(2); // 10
```

## Quick Revision

- Partial application fixes some arguments
- Use `bind()` or custom function
- Returns new function
- More flexible than currying
- Use for: function specialization

---

## Related Topics

- [[What-is-Partial]] - [[What-is-Partial|Partial application]]
- [[Partial Application]] - [[Partial Application|Partial application]]
- [[What-is-Currying]] - [[What-is-Currying|Currying]]
- [[What-is-HOF]] - [[What-is-HOF|Higher-order functions]]
