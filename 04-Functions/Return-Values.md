# How to Return Values

## Basic Return

```javascript
// Return a value
function add(a, b) {
    return a + b;
}

const result = add(5, 3); // 8
```

## Early Return

```javascript
// Return early if condition met
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

## Returning Objects

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

- `return` sends value back from function
- Exits function immediately
- Without return, function returns `undefined`
- Can return any data type
- Use for early exit from functions

---

## Related Topics

- [[What-is-Return]] - [[What-is-Return|Return]] overview
- [[What-is-Function]] - [[What-is-Function|Functions]]
- [[Write-Recursion]] - [[Write-Recursion|Recursion]]
- [[What-is-Parameter]] - [[What-is-Parameter|Parameters]]
