# Object.freeze

## Definition

`Object.freeze()` makes an object **immutable** - properties cannot be added, removed, or modified.

## Basic Usage

```javascript
const obj = {
    name: "John",
    age: 30
};

Object.freeze(obj);

obj.name = "Jane"; // Silently fails (or throws in strict mode)
obj.city = "NYC"; // Silently fails
delete obj.name; // Silently fails

console.log(obj); // { name: "John", age: 30 }
```

## Shallow Freeze

```javascript
const obj = {
    name: "John",
    address: {
        city: "NYC"
    }
};

Object.freeze(obj);

obj.name = "Jane"; // Fails
obj.address.city = "Boston"; // Works! (shallow freeze)
```

## Deep Freeze

```javascript
function deepFreeze(obj) {
    Object.freeze(obj);
    for (const key of Object.keys(obj)) {
        if (typeof obj[key] === 'object' && obj[key] !== null) {
            deepFreeze(obj[key]);
        }
    }
    return obj;
}

const obj = {
    name: "John",
    address: { city: "NYC" }
};

deepFreeze(obj);
obj.address.city = "Boston"; // Fails
```

## Quick Revision

- `Object.freeze()` makes object immutable
- Shallow freeze (nested objects mutable)
- Use deep freeze for full immutability
- Cannot add, remove, or modify properties
- Use for: constants, config objects

---

## Related Topics

- [[What-is-Immutability]] - [[What-is-Immutability|Immutability]]
- [[mutable]] - [[mutable|Mutable vs immutable]]
- [[What-is-Object]] - [[What-is-Object|Objects]]
- [[Create-Object]] - [[Create-Object|Creating objects]]
