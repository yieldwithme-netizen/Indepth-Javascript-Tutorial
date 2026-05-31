# Iterators

## Definition

Iterators are objects with a `next()` method that returns `{value, done}`.

## Basic Example

```javascript
const range = {
    start: 1,
    end: 5,
    [Symbol.iterator]() {
        let current = this.start;
        const end = this.end;
        return {
            next() {
                if (current <= end) {
                    return { value: current++, done: false };
                }
                return { done: true };
            }
        };
    }
};

for (const num of range) {
    console.log(num); // 1, 2, 3, 4, 5
}
```

## Quick Revision

- Iterator: `next()` returns `{value, done}`
- Implements Symbol.iterator
- Enables for...of loops
- Use for: custom sequences

---

## Related Topics

- [[What-is-Iterator]] - [[What-is-Iterator|Iterator]]
- [[Iterators]] - [[Iterators|Iterators]]
- [[Create-Iterator]] - [[Create-Iterator|Creating iterators]]
- [[Symbol-Iteration]] - [[Symbol-Iteration|Symbol.iterator]]
