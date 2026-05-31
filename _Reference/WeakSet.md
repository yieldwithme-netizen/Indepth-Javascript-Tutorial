# WeakSet

## Definition
WeakSet is a collection of objects where each object exists weakly. It allows garbage collection to occur if there are no other references to the object, preventing memory leaks. Unlike Set, WeakSet only stores objects and is not iterable.

## Creating a WeakSet

```javascript
// Empty WeakSet
const weakSet1 = new WeakSet();

// With initial values
const obj1 = { name: "John" };
const obj2 = { name: "Jane" };
const weakSet2 = new WeakSet([obj1, obj2]);

// WeakSet with objects only
const user = { name: "Admin" };
const weakSet3 = new WeakSet();
weakSet3.add(user);
```

## Methods

### add()
```javascript
const weakSet = new WeakSet();
const obj = { id: 1 };

weakSet.add(obj);
console.log(weakSet.has(obj));  // true

// Adding non-object throws error
// weakSet.add(1);  // TypeError
// weakSet.add("string");  // TypeError
```

### has()
```javascript
const weakSet = new WeakSet();
const obj = { name: "Test" };

weakSet.add(obj);
console.log(weakSet.has(obj));   // true
console.log(weakSet.has({}));    // false (different object)
console.log(weakSet.has(null));  // TypeError
```

### delete()
```javascript
const weakSet = new WeakSet();
const obj = { name: "Test" };

weakSet.add(obj);
weakSet.delete(obj);
console.log(weakSet.has(obj));  // false
```

## Memory Management

```javascript
// WeakSet allows garbage collection
let obj = { data: "important" };
const weakSet = new WeakSet();
weakSet.add(obj);

console.log(weakSet.has(obj));  // true

// Remove reference
obj = null;

// After garbage collection:
// weakSet.has(obj) would be false
// Object can be garbage collected
```

## Practical Use Cases

### 1. Tracking Object Visits
```javascript
function traverseObject(obj, visited = new WeakSet()) {
  if (typeof obj !== 'object' || obj === null) {
    return obj;
  }

  if (visited.has(obj)) {
    return '[Circular Reference]';
  }

  visited.add(obj);

  const result = {};
  for (const key in obj) {
    result[key] = traverseObject(obj[key], visited);
  }

  return result;
}

// Test with circular reference
const data = { name: "John" };
data.self = data;
console.log(traverseObject(data));
// { name: "John", self: "[Circular Reference]" }
```

### 2. Private Data Storage
```javascript
const privateData = new WeakSet();

class User {
  constructor(name, password) {
    this.name = name;
    privateData.set(this, { password });
  }

  checkPassword(password) {
    return privateData.get(this).password === password;
  }

  changePassword(newPassword) {
    privateData.get(this).password = newPassword;
  }
}

const user = new User("John", "secret123");
console.log(user.checkPassword("secret123"));  // true
// user.password is undefined (private)
```

### 3. DOM Element Tracking
```javascript
const processedElements = new WeakSet();

function processElement(element) {
  if (processedElements.has(element)) {
    return;  // Already processed
  }

  // Process element
  element.classList.add('processed');
  processedElements.add(element);
}

// When element is removed from DOM, it can be garbage collected
// WeakSet entry is automatically removed
```

## WeakSet vs Set

| Feature | WeakSet | Set |
|---------|---------|-----|
| Element types | Objects only | Any value |
| Iterable | No | Yes |
| Size property | No | Yes |
| Memory management | Weak references | Strong references |
| Methods | add, has, delete | add, has, delete, forEach, clear |
| Garbage collection | Automatic | Manual |

## Restrictions

```javascript
const weakSet = new WeakSet();

// Only objects allowed
// weakSet.add(1);  // TypeError
// weakSet.add("string");  // TypeError
// weakSet.add(true);  // TypeError
// weakSet.add(null);  // TypeError
// weakSet.add(undefined);  // TypeError

// Not iterable
// for (const item of weakSet) {}  // TypeError
// weakSet.forEach(item => {});  // TypeError

// No size property
// console.log(weakSet.size);  // undefined

// No clear method
// weakSet.clear();  // TypeError
```

## Common Use Cases
- Tracking objects for processing
- Storing private data
- Caching objects
- Preventing memory leaks
- DOM element management
- Object relationship tracking

## Common Mistakes

| Mistake | Solution |
|---------|----------|
| Trying to iterate | Use Set if iteration needed |
| Storing primitives | Only objects allowed |
| Expecting size property | Check with has() instead |
| Not understanding weak refs | Objects may be garbage collected |

## Quick Revision Summary
- WeakSet stores objects with weak references
- Only add, has, delete methods available
- Not iterable, no size property
- Allows garbage collection of unused objects
- Ideal for tracking objects without preventing cleanup
- Use Set when you need iteration or primitive values

## Related Topics
- [[WeakMap]]
- [[Set]]
- [[Map]]
- [[Garbage-Collection]]
- [[Memory-Management]]
- [[Objects]]
