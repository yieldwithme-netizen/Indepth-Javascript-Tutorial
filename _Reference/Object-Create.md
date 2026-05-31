# Object.create

## Definition

`Object.create()` creates a **new object with specified prototype**.

## Basic Usage

```javascript
const proto = {
    greet() {
        return `Hello, ${this.name}!`;
    }
};

const obj = Object.create(proto);
obj.name = "John";
console.log(obj.greet()); // "Hello, John!"
```

## With Property Descriptors

```javascript
const obj = Object.create(null, {
    name: {
        value: "John",
        writable: true,
        enumerable: true,
        configurable: true
    }
});
```

## Quick Revision

- `Object.create(proto)` creates object with prototype
- Use for inheritance
- Can add property descriptors
- Use for: prototypal inheritance

---

## Related Topics

- [[What-is-ObjectCreate]] - [[What-is-ObjectCreate|Object.create]]
- [[Object-Create]] - [[Object-Create|Object.create]]
- [[What-is-Prototype]] - [[What-is-Prototype|Prototypes]]
- [[What-is-ProtoInheritance]] - [[What-is-ProtoInheritance|Prototypal inheritance]]
