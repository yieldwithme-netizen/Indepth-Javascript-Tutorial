# Iteration Protocols

## Definition

Iteration protocols define **how objects are iterated** in JavaScript.

## Iterable Protocol

```javascript
class Range {
    constructor(start, end) {
        this.start = start;
        this.end = end;
    }
    
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
}

for (const num of new Range(1, 5)) {
    console.log(num); // 1, 2, 3, 4, 5
}
```

## Iterator Protocol

```javascript
// Iterator has next() method
const iterator = {
    next() {
        return { value: 1, done: false };
    }
};
```

## Quick Revision

- Iterable: has `[Symbol.iterator]`
- Iterator: has `next()` returning `{value, done}`
- Enables `for...of`
- Use for: custom iteration

---

## Related Topics

- [[What-is-Iterator]] - [[What-is-Iterator|Iterator]]
- [[Iteration-Protocols]] - [[Iteration-Protocols|Iteration protocols]]
- [[Symbol-Iteration]] - [[Symbol-Iteration|Symbol.iterator]]
- [[Create-Iterator]] - [[Create-Iterator|Creating iterators]]
