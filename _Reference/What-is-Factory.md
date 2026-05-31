# What-is-Factory

## Definition

Factory functions **create and return objects**.

## Example

```javascript
function createUser(name, age) {
    return {
        name,
        age,
        greet() {
            return `Hello, ${this.name}!`;
        }
    };
}

const user = createUser("John", 30);
```

## Quick Revision

- Factory = function creating objects
- No `new` keyword needed
- More flexible than classes
- Use for: object creation

---

## Related Topics

- [[What-is-Factory]] - [[What-is-Factory|Factory]]
- [[What-is-Factory]] - [[What-is-Factory|Factory functions]]
- [[Factory-Functions]] - [[Factory-Functions|Factory functions]]
- [[Factory-Pattern]] - [[Factory-Pattern|Factory pattern]]
