# WeakSet - Use Cases and Practical Examples

## Definition

A `WeakSet` is a collection of objects where each object is held weakly. Unlike a regular [[Set]], `WeakSet` does not prevent garbage collection of its entries and cannot be iterated over.

```javascript
const weakSet = new WeakSet();
```

## Key Characteristics

- **Only objects** — primitives are not allowed
- **Not iterable** — no `.forEach()`, no spread operator
- **Weak references** — entries are automatically removed when objects are garbage collected
- **No size property** — prevents enumeration

## Common Use Cases

### 1. Tracking Visited DOM Elements

```javascript
const visited = new WeakSet();

function markAsVisited(element) {
  visited.add(element);
  element.classList.add('visited');
}

function hasBeenVisited(element) {
  return visited.has(element);
}

document.querySelectorAll('.link').forEach(link => {
  link.addEventListener('click', () => {
    markAsVisited(link);
    console.log('Link visited:', link.textContent);
  });
});

// When elements are removed from DOM, WeakSet entries are cleaned up
```

### 2. Preventing Duplicate Object Processing

```javascript
const processedObjects = new WeakSet();

function processObject(obj) {
  if (processedObjects.has(obj)) {
    console.log('Already processed:', obj.id);
    return;
  }
  
  // Process the object
  console.log('Processing:', obj.id);
  obj.status = 'done';
  processedObjects.add(obj);
}

const item = { id: 1, status: 'pending' };
processObject(item);  // Processing: 1
processObject(item);  // Already processed: 1
```

### 3. Private Member Detection

```javascript
const privateMembers = new WeakSet();

class BankAccount {
  #balance; // Private field
  
  constructor(owner, balance) {
    this.owner = owner;
    this.#balance = balance;
    privateMembers.add(this);
  }
  
  static isPrivateInstance(obj) {
    return privateMembers.has(obj);
  }
}

const account = new BankAccount('Alice', 10000);
console.log(BankAccount.isPrivateInstance(account)); // true
```

### 4. Object Pool Management

```javascript
const activeObjects = new WeakSet();

class ObjectPool {
  constructor() {
    this.pool = [];
  }
  
  acquire(obj) {
    activeObjects.add(obj);
    return obj;
  }
  
  release(obj) {
    activeObjects.delete(obj);
  }
  
  isActive(obj) {
    return activeObjects.has(obj);
  }
}

const pool = new ObjectPool();
const connection = { id: 1 };
pool.acquire(connection);
console.log(pool.isActive(connection)); // true
pool.release(connection);
console.log(pool.isActive(connection)); // false
```

### 5. Event Handler Cleanup

```javascript
const handlers = new WeakSet();

function bindCleanup(element, event, handler) {
  element.addEventListener(event, handler);
  handlers.add(element);
}

function unbindAll(element) {
  // WeakSet doesn't support iteration, so track handlers separately
  element.removeEventListener('click', handlers.get(element));
  handlers.delete(element);
}

const card = document.querySelector('.card');
bindCleanup(card, 'click', () => console.log('clicked'));
```

## Common Mistakes

```javascript
// ❌ Wrong: Using primitives
const ws = new WeakSet();
ws.add(42);         // TypeError
ws.add('string');   // TypeError
ws.add(true);       // TypeError

// ✅ Correct: Use objects
ws.add({});
ws.add([]);

// ❌ Wrong: Trying to iterate
for (const item of new WeakSet()) {}  // TypeError
console.log([...new WeakSet()]);      // TypeError

// ❌ Wrong: Checking size
console.log(new WeakSet().size);      // undefined

// ✅ Correct: Use .has() and .size check via workaround
ws.has(obj);  // true/false
```

## Quick Revision Summary

| Feature | WeakSet | [[Set]] |
|---------|---------|---------|
| Value types | Objects only | Any type |
| Iterability | Not iterable | Iterable |
| Size property | No | Yes |
| Garbage collection | Automatic cleanup | Manual deletion needed |
| Use case | Object tracking, caching | Unique value collections |

## Related Topics

- [[Set]] - Iterable collection of unique values
- [[WeakMap]] - Key-value pairs with weak references
- [[Map]] - Iterable key-value collection
- [[GarbageCollection]] - Automatic memory management
- [[DataStructures]] - Choosing the right collection type
