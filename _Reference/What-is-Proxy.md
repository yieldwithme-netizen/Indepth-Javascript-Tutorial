# What-is-Proxy

## Definition

Proxy **intercepts operations** on objects.

## Example

```javascript
const handler = {
    get(target, prop) {
        return prop in target ? target[prop] : 'Default';
    }
};

const proxy = new Proxy({ name: "John" }, handler);
console.log(proxy.name);    // "John"
console.log(proxy.unknown); // "Default"
```

## Quick Revision

- Proxy intercepts object operations
- Handlers: get, set, delete
- Use for: validation, logging, defaults

---

## Related Topics

- [[What-is-Proxy]] - [[What-is-Proxy|Proxy]]
- [[What-is-Proxy]] - [[What-is-Proxy|Proxy]]
- [[What-is-Reflect]] - [[What-is-Reflect|Reflect]]
- [[MetaProgramming]] - [[MetaProgramming|Metaprogramming]]
