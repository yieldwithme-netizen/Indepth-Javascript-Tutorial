# Try Statement

## Definition

`try` statement **handles errors** by wrapping risky code.

## Syntax

```javascript
try {
    riskyOperation();
} catch (error) {
    console.log(error.message);
} finally {
    cleanup();
}
```

## Quick Revision

- `try` wraps risky code
- `catch` handles errors
- `finally` always runs
- Can re-throw with `throw`

---

## Related Topics

- [[What-is-TryCatch]] - [[What-is-TryCatch|try-catch]]
- [[Try-Statement]] - [[Try-Statement|try statement]]
- [[Use-TryCatch]] - [[Use-TryCatch|Using try-catch]]
- [[What-is-Error]] - [[What-is-Error|Errors]]
