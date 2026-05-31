# Arguments Object

## Definition

The arguments object is an **array-like object** containing function arguments.

## Example

```javascript
function sum() {
    let total = 0;
    for (let i = 0; i < arguments.length; i++) {
        total += arguments[i];
    }
    return total;
}

sum(1, 2, 3); // 6
```

## Quick Revision

- `arguments` is array-like
- Has `length` property
- Use `Array.from()` to convert
- Prefer rest parameters `...args`

---

## Related Topics

- [[What-is-RestParam]] - [[What-is-RestParam|Rest parameters]]
- [[Arguments-Object]] - [[Arguments-Object|Arguments object]]
- [[What-is-Function]] - [[What-is-Function|Functions]]
- [[What-is-Parameter]] - [[What-is-Parameter|Parameters]]
