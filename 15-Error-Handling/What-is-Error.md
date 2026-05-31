# What is an Error?

## Definition

An error is an **object thrown when something goes wrong** in code.

## Error Types

| Error | When |
|-------|------|
| ReferenceError | Undeclared variable |
| TypeError | Wrong type operation |
| SyntaxError | Invalid syntax |
| RangeError | Value out of range |
| URIError | Invalid URI |
| EvalError | Invalid eval() |

## Try-Catch-Finally

```javascript
try {
    // Code that might fail
    const result = riskyOperation();
} catch (error) {
    // Handle error
    console.log(error.message);
} finally {
    // Always runs
    cleanup();
}
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

## Custom Errors

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
```

## Quick Revision

- Error = object with message, name, stack
- Types: Reference, Type, Syntax, Range
- Use try/catch/finally
- Throw custom errors with `throw`
- Extend Error class for custom errors

---

## Related Topics

- [[What-is-Error]] - Errors overview
- [[What-is-ErrorTypes]] - Error types
- [[What-is-TryCatch]] - Try-catch
- [[Use-TryCatch]] - Using try-catch
- [[Throw-Errors]] - Throwing errors
- [[What-is-CustomError]] - Custom errors
