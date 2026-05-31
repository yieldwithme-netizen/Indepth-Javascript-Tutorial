# Try Statement

## Definition

The `try` statement **handles errors** by wrapping code that might fail.

## Basic Syntax

```javascript
try {
    // Code that might fail
    riskyOperation();
} catch (error) {
    // Handle error
    console.log(error.message);
} finally {
    // Always runs
    cleanup();
}
```

## Try-Catch

```javascript
try {
    const data = JSON.parse(invalidJSON);
} catch (error) {
    console.log("Parse error:", error.message);
}
```

## Try-Catch-Finally

```javascript
try {
    openFile();
    readFile();
} catch (error) {
    console.log("Error:", error.message);
} finally {
    closeFile(); // always runs
}
```

## Quick Revision

- `try` wraps risky code
- `catch` handles errors
- `finally` always runs
- Can re-throw with `throw error`

---

## Related Topics

- [[What-is-TryCatch]] - [[What-is-TryCatch|try-catch]]
- [[Try-Statement]] - [[Try-Statement|try statement]]
- [[Use-TryCatch]] - [[Use-TryCatch|Using try-catch]]
- [[What-is-Error]] - [[What-is-Error|Errors]]
