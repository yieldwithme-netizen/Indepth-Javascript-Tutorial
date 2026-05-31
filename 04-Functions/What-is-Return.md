# What is a [[Return]] Statement?

## Definition

The `[[Return]]` statement **sends a value back** from a [[Function]] and exits it.

## Basic Usage

```javascript
function add(a, b) {
    return a + b;
}

const result = add(5, 3); // 8
```

## Multiple [[Return]]s (Early Exit)

```javascript
function getDiscount(price, isMember) {
    if (price < 0) {
        return 0; // early exit
    }
    
    if (isMember) {
        return price * 0.2;
    }
    
    return price * 0.1;
}
```

## Returning [[Object|Objects]]

```javascript
// Implicit return (arrow function)
const createUser = (name, age) => ({ name, age });

// Explicit return
function createUser(name, age) {
    return {
        name: name,
        age: age
    };
}
```

## Returning Nothing

```javascript
// No return statement
function greet(name) {
    console.log(`Hello, ${name}!`);
    // returns undefined implicitly
}

const result = greet("John"); // undefined
```

## Common Mistakes

```javascript
// ❌ Wrong: No return statement
function add(a, b) {
    a + b; // doesn't return anything!
}

// ✅ Right: Use return
function add(a, b) {
    return a + b;
}

// ❌ Wrong: Returning multiple values
function getMinMax(arr) {
    return Math.min(arr), Math.max(arr); // only returns max!
}

// ✅ Right: Return array or object
function getMinMax(arr) {
    return [Math.min(arr), Math.max(arr)];
}
```

## Quick Revision

- `[[Return]]` sends value back from [[Function]]
- Exits [[Function]] immediately
- Without return, [[Function]] returns `undefined`
- Can return any [[Data Type]]
- Use for early exit from functions

---

## Related Topics

- [[What-is-Function]] - Functions
- [[Return-Values]] - Returning values
- [[Write-Recursion]] - Recursive functions
- [[What-is-Parameter]] - Parameters