# Error Object

## Definition

The Error object contains **information about an error**.

## Properties

```javascript
try {
    riskyOperation();
} catch (error) {
    console.log(error.name);    // "Error"
    console.log(error.message); // "Something went wrong"
    console.log(error.stack);   // Stack trace
}
```

## Error Types

| Type | Description |
|------|-------------|
| Error | Generic error |
| TypeError | Wrong type |
| ReferenceError | Undeclared variable |
| SyntaxError | Invalid syntax |
| RangeError | Out of range |

## Quick Revision

- Error object has name, message, stack
- Different error types for different issues
- Use try/catch to handle

---

## Related Topics

- [[What-is-Error]] - [[What-is-Error|Errors]]
- [[Error-Object]] - [[Error-Object|Error object]]
- [[What-is-ErrorTypes]] - [[What-is-ErrorTypes|Error types]]
- [[What-is-TryCatch]] - [[What-is-TryCatch|try-catch]]
