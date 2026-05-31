# Factory Pattern

## Definition

Factory pattern provides **flexible object creation** without using `new`.

## Basic Example

```javascript
function createUser(type) {
    if (type === 'admin') {
        return { role: 'admin', permissions: ['read', 'write', 'delete'] };
    }
    return { role: 'user', permissions: ['read'] };
}

const admin = createUser('admin');
const user = createUser('user');
```

## Quick Revision

- Factory = function that creates objects
- No `new` keyword needed
- More flexible than constructors
- Use for: object creation, encapsulation

---

## Related Topics

- [[What-is-Factory]] - [[What-is-Factory|Factory]]
- [[Factory-Pattern]] - [[Factory-Pattern|Factory pattern]]
- [[Factory-Functions]] - [[Factory-Functions|Factory functions]]
- [[What-is-Constructor]] - [[What-is-Constructor|Constructors]]
