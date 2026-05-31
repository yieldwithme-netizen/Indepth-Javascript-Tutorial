# Throw Statement

## Definition

The `throw` statement **creates and raises a custom error**.

## Basic Syntax

```javascript
throw "Error occurred";
throw 42;
throw true;
throw { message: "Error", code: 404 };
```

## Throwing Errors

```javascript
function divide(a, b) {
    if (b === 0) {
        throw new Error("Cannot divide by zero");
    }
    return a / b;
}

try {
    divide(10, 0);
} catch (error) {
    console.log(error.message); // "Cannot divide by zero"
}
```

## Throwing Custom Errors

```javascript
class ValidationError extends Error {
    constructor(message) {
        super(message);
        this.name = "ValidationError";
    }
}

function validateAge(age) {
    if (age < 0 || age > 150) {
        throw new ValidationError("Invalid age");
    }
}

try {
    validateAge(-5);
} catch (error) {
    if (error instanceof ValidationError) {
        console.log(error.message); // "Invalid age"
    }
}
```

## Re-throwing

```javascript
function processUser(user) {
    try {
        validateUser(user);
        saveUser(user);
    } catch (error) {
        console.log("Error processing user:", error.message);
        throw error; // re-throw
    }
}
```

## Quick Revision

- `throw` creates custom errors
- Can throw any value
- Use `throw new Error()` for error objects
- Re-throw with `throw error`
- Use try/catch to handle

---

## Related Topics

- [[What-is-Error]] - [[What-is-Error|Errors]]
- [[What-is-TryCatch]] - [[What-is-TryCatch|try-catch]]
- [[Use-TryCatch]] - [[Use-TryCatch|Using try-catch]]
- [[Throw-Errors]] - [[Throw-Errors|Throwing errors]]
- [[What-is-CustomError]] - [[What-is-CustomError|Custom errors]]
