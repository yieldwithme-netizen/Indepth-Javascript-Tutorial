# Iteration Protocols

## Definition

Iteration protocols define **how objects are iterated**.

## Iterable Protocol

```javascript
class Range {
    [Symbol.iterator]() {
        let current = this.start;
        return {
            next() {
                if (current <= this.end) {
                    return { value: current++, done: false };
                }
                return { done: true };
            }
        };
    }
}
```

## Quick Revision

- Iterable: has `[Symbol.iterator]`
- Iterator: has `next()` method
- Enables `for...of`

---

## Related Topics

- [[What-is-Iterator]] - [[What-is-Iterator|Iterator]]
- [[What-is-Iterator]] - [[What-is-Iterator|Iteration protocols]]
- [[Symbol-Iteration]] - [[Symbol-Iteration|Symbol.iterator]]
