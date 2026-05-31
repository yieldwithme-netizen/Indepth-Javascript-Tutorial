# Error Object

## Definition

Error object contains **error information**.

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

## Quick Revision

- Error object: name, message, stack
- Different error types
- Use try/catch to handle

---

## Related Topics

- [[What-is-Error]] - [[What-is-Error|Errors]]
- [[Error-Object]] - [[Error-Object|Error object]]
- [[What-is-TryCatch]] - [[What-is-TryCatch|try-catch]]
- [[What-is-ErrorTypes]] - [[What-is-ErrorTypes|Error types]]
