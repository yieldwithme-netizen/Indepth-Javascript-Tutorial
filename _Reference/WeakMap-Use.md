# WeakMap - Use Cases and Practical Examples

## Definition

A `WeakMap` is a collection of key-value pairs where keys must be objects (or non-registered symbols) and values can be any type. Unlike regular [[Map]], `WeakMap` keys are held weakly — meaning if there are no other references to the key object, the garbage collector can reclaim it and remove the entry.

```javascript
const weakMap = new WeakMap();
```

## Key Characteristics

- **Keys must be objects** (not primitives)
- **Not iterable** — no `.keys()`, `.values()`, `.entries()`, or `.size`
- **Weak references** — entries are automatically cleaned up when keys are garbage collected
- **No enumeration** — prevents memory leaks and information leakage

## Common Use Cases

### 1. Private Data Storage

```javascript
const privateData = new WeakMap();

class User {
  constructor(name, password) {
    this.name = name;
    privateData.set(this, { password });
  }

  checkPassword(pwd) {
    return privateData.get(this).password === pwd;
  }
}

const user = new User('Alice', 'secret123');
console.log(user.name);           // Alice
console.log(user.password);       // undefined (private!)
console.log(user.checkPassword('secret123')); // true
```

### 2. Caching Expensive Computation

```javascript
const cache = new WeakMap();

function processHeavyObject(obj) {
  if (cache.has(obj)) {
    return cache.get(obj);
  }
  const result = expensiveCalculation(obj);
  cache.set(obj, result);
  return result;
}

function expensiveCalculation(obj) {
  // Simulate heavy computation
  return Object.keys(obj).length * 1000;
}

const data = { a: 1, b: 2, c: 3 };
console.log(processHeavyObject(data)); // 3000 (computed)
console.log(processHeavyObject(data)); // 3000 (cached)
```

### 3. DOM Element Metadata

```javascript
const elementData = new WeakMap();

function addClickHandler(element, handler) {
  elementData.set(element, { handler, clicks: 0 });
  element.addEventListener('click', () => {
    const data = elementData.get(element);
    data.clicks++;
    handler(data.clicks);
  });
}

const btn = document.querySelector('#myButton');
addClickHandler(btn, (count) => console.log(`Clicked ${count} times`));
// When btn is removed from DOM, WeakMap entry is garbage collected
```

### 4. Tracking Object State

```javascript
const objectState = new WeakMap();

function trackObject(obj, state) {
  objectState.set(obj, state);
}

function getObjectState(obj) {
  return objectState.get(obj) || 'unknown';
}

const order1 = { id: 1, total: 100 };
trackObject(order1, 'pending');
console.log(getObjectState(order1)); // 'pending'
```

## Common Mistakes

```javascript
// ❌ Wrong: Using primitive keys
const wm = new WeakMap();
wm.set('key', 'value'); // TypeError: Invalid value used as weak map key

// ✅ Correct: Use object keys
wm.set({}, 'value');

// ❌ Wrong: Trying to iterate over WeakMap
for (const [key, value] of new WeakMap()) {} // TypeError

// ✅ Correct: Use .has() and .get() instead
wm.has(obj);  // true/false
wm.get(obj);  // value
```

## Quick Revision Summary

| Feature | WeakMap | [[Map]] |
|---------|---------|---------|
| Key types | Objects only | Any type |
| Iterability | Not iterable | Iterable |
| Size property | No | Yes |
| Garbage collection | Automatic cleanup | Manual removal needed |
| Use case | Private data, caching | General-purpose collections |

## Related Topics

- [[Map]] - Iterable key-value collection
- [[Set]] - Collection of unique values
- [[WeakSet]] - Weak references to objects
- [[Symbols]] - Can also be used as WeakMap keys
- [[GarbageCollection]] - How WeakMap interacts with GC
