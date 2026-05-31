# WeakMap

## Definition

WeakMap stores **key-value pairs with weak references** to keys.

## Basic Usage

```javascript
const weakMap = new WeakMap();
let obj = { name: "John" };
weakMap.set(obj, "data");
weakMap.get(obj); // "data"
obj = null; // key eligible for garbage collection
```

## Quick Revision

- Keys must be objects
- Allows garbage collection
- Use for: private data, caching

---

## Related Topics

- [[What-is-WeakMap]] - [[What-is-WeakMap|WeakMap]]
- [[WeakMap]] - [[WeakMap|WeakMap]]
- [[Use-WeakMap]] - [[Use-WeakMap|Using WeakMap]]
