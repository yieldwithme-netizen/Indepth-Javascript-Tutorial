# WeakMap

A WeakMap is a collection of key-value pairs where keys must be objects and values can be any type. Keys are held weakly, allowing garbage collection when no other references exist.

## Basic Usage

```javascript
const weakmap = new WeakMap();

const obj1 = { name: 'Alice' };
const obj2 = { name: 'Bob' };

weakmap.set(obj1, 'value1');
weakmap.set(obj2, 'value2');

console.log(weakmap.get(obj1)); // 'value1'
console.log(weakmap.has(obj2)); // true

weakmap.delete(obj1);
console.log(weakmap.has(obj1)); // false
```

## Key Characteristics

```javascript
// Keys must be objects
const weakmap = new WeakMap();
// weakmap.set('string', 'value'); // Error: Invalid value used as weak map key

// No size property
// console.log(weakmap.size); // undefined

// Not iterable
// for (const [key, value] of weakmap) {} // Error
```

## Private Data Pattern

```javascript
const privateData = new WeakMap();

class User {
  constructor(name, password) {
    this.name = name;
    privateData.set(this, { password });
  }

  checkPassword(password) {
    return privateData.get(this).password === password;
  }
}

const user = new User('Alice', 'secret123');
console.log(user.name); // 'Alice'
console.log(user.checkPassword('secret123')); // true
// Password is not accessible from outside
```

## Caching Pattern

```javascript
const cache = new WeakMap();

function processObject(obj) {
  if (cache.has(obj)) {
    return cache.get(obj);
  }

  const result = expensiveComputation(obj);
  cache.set(obj, result);
  return result;
}

function expensiveComputation(obj) {
  // Simulate heavy computation
  return { processed: true, data: obj };
}

const obj = { data: 123 };
console.log(processObject(obj)); // Computed result
console.log(processObject(obj)); // Cached result
```

## DOM Element Metadata

```javascript
const elementData = new WeakMap();

function attachMetadata(element, metadata) {
  elementData.set(element, metadata);
}

function getMetadata(element) {
  return elementData.get(element);
}

const button = document.querySelector('button');
attachMetadata(button, { clicks: 0, lastClicked: null });

// Metadata is garbage collected when element is removed
```

## WeakMap vs Map

```javascript
// WeakMap: keys must be objects, no iteration
const weakmap = new WeakMap();
weakmap.set({}, 'value');

// Map: any key type, iterable
const map = new Map();
map.set('string', 'value');
map.set(123, 'value');
map.set({}, 'value');

for (const [key, value] of map) {
  console.log(key, value);
}
```

## Common Use Cases

- Private data storage
- Caching computed results
- Storing metadata for objects
- Avoiding memory leaks with DOM references
- Object relationship tracking

## Common Mistakes

- Using non-object keys
- Trying to iterate over WeakMap
- Expecting `size` property
- Not understanding weak references
- Using WeakMap when Map is needed

## Related Topics

- [[Map]]
- [[WeakSet]]
- [[Garbage Collection]]
- [[Private Variables]]
- [[Caching]]

## Quick Revision

- WeakMap keys must be objects
- Keys are weakly referenced (garbage collected)
- Not iterable, no `size` property
- Methods: `set`, `get`, `has`, `delete`
- Useful for private data and caching
- Prevents memory leaks with object references
