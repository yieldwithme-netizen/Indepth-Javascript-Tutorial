# Error Handling

## Definition

Error handling **catches and manages errors** that occur during program execution.

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
```

## Custom Errors

```javascript
class ValidationError extends Error {
    constructor(message) {
        super(message);
        this.name = "ValidationError";
    }
}
```

## Quick Revision

- Try-catch handles errors
- `throw` creates custom errors
- `finally` always runs
- Extend Error for custom errors

---

## Related Topics

- [[What-is-Error-Handling]] - [[What-is-Error-Handling|Error handling]]
- [[Error-Handling]] - [[Error-Handling|Error handling]]
- [[What-is-TryCatch]] - [[What-is-TryCatch|try-catch]]
- [[Throw-Errors]] - [[Throw-Errors|Throwing errors]]
