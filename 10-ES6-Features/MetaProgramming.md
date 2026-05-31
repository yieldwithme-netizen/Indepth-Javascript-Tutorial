# MetaProgramming

## Definition

Metaprogramming **manipulates code** as data.

## Features

```javascript
// Reflect
Reflect.get(obj, 'name');
Reflect.set(obj, 'name', 'John');

// Proxy
const handler = {
    get(target, prop) {
        return prop in target ? target[prop] : 'Default';
    }
};
const proxy = new Proxy(obj, handler);

// Symbol
const sym = Symbol('id');
```

## Quick Revision

- Reflect: manipulate objects
- Proxy: intercept operations
- Symbol: unique identifiers
- Use for: validation, logging

---

## Related Topics

- [[What-is-Proxy]] - [[What-is-Proxy|Proxy]]
- [[What-is-Symbol]] - [[What-is-Symbol|Symbol]]
- [[MetaProgramming]] - [[MetaProgramming|Metaprogramming]]
- [[What-is-Reflect]] - [[What-is-Reflect|Reflect]]
