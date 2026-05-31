# Use WeakMap

## Definition

WeakMap stores **key-value pairs with weak references** to keys.

## Basic Usage

```javascript
const weakMap = new WeakMap();

let obj = { name: "John" };
weakMap.set(obj, "some data");

console.log(weakMap.get(obj)); // "some data"

obj = null; // key is now eligible for garbage collection
```

## Use Cases

```javascript
// Private data
class User {
    constructor(name) {
        this._name = name;
    }
    
    get name() {
        return userMap.get(this);
    }
}

const userMap = new WeakMap();

// Caching
const cache = new WeakMap();

function process(obj) {
    if (cache.has(obj)) {
        return cache.get(obj);
    }
    const result = expensiveOperation(obj);
    cache.set(obj, result);
    return result;
}
```

## Quick Revision

- WeakMap: key-value with weak refs
- Keys must be objects
- Allows garbage collection
- Use for: private data, caching

---

## Related Topics

- [[What-is-WeakMap]] - [[What-is-WeakMap|WeakMap]]
- [[Use-WeakMap]] - [[Use-WeakMap|Using WeakMap]]
- [[WeakMap]] - [[WeakMap|WeakMap]]
- [[What-is-Map]] - [[What-is-MapES6|Map]]
