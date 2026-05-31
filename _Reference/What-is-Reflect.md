# What-is-Reflect

## Definition

Reflect provides **methods for object operations**.

## Example

```javascript
const obj = { name: "John" };

Reflect.get(obj, 'name');      // "John"
Reflect.set(obj, 'age', 30);   // true
Reflect.has(obj, 'name');      // true
Reflect.deleteProperty(obj, 'age'); // true
```

## Quick Revision

- Reflect: manipulate objects
- Methods: get, set, has, delete
- Use with Proxy
- Modern alternative to Object methods

---

## Related Topics

- [[What-is-Reflect]] - [[What-is-Reflect|Reflect]]
- [[What-is-Reflect]] - [[What-is-Reflect|Reflect]]
- [[What-is-Proxy]] - [[What-is-Proxy|Proxy]]
- [[MetaProgramming]] - [[MetaProgramming|Metaprogramming]]
