# Reflect - Default Object Operations API

## Definition

`Reflect` is a built-in object that provides methods for interceptable JavaScript operations. It mirrors the methods on the `Proxy` handler, allowing you to call default behavior for operations and build reusable decorators.

```javascript
Reflect.get(target, property);
Reflect.set(target, property, value);
```

## Why Use Reflect?

1. **Consistency** — Matches Proxy trap signatures exactly
2. **Return values** — `set` returns boolean (success/failure)
3. **Proper receiver handling** — Passes `this` correctly
4. **Safer property access** — Avoids prototype chain issues

## All Reflect Methods

| Method | Description |
|--------|-------------|
| `Reflect.apply()` | Call a function with arguments |
| `Reflect.construct()` | Call a constructor with `new` |
| `Reflect.defineProperty()` | Define a property |
| `Reflect.deleteProperty()` | Delete a property |
| `Reflect.get()` | Get a property value |
| `Reflect.getOwnPropertyDescriptor()` | Get property descriptor |
| `Reflect.getPrototypeOf()` | Get prototype |
| `Reflect.has()` | Check if property exists (`in` operator) |
| `Reflect.isExtensible()` | Check if object is extensible |
| `Reflect.ownKeys()` | Get all own property keys |
| `Reflect.preventExtensions()` | Prevent adding properties |
| `Reflect.set()` | Set a property value |
| `Reflect.setPrototypeOf()` | Set prototype |

## Common Use Cases

### 1. Safe Function Application

```javascript
// Using Reflect.apply instead of Function.prototype.apply
function greet(name, punctuation) {
  return `Hello, ${name}${punctuation}`;
}

// Traditional
greet.apply(null, ['World', '!']);

// With Reflect (more explicit)
Reflect.apply(greet, null, ['World', '!']); // "Hello, World!"
```

### 2. Constructor Interception

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }
}

// Using Reflect.construct to call constructor without new
const dog = Reflect.construct(Animal, ['Rex']);
console.log(dog.name); // 'Rex'
console.log(dog instanceof Animal); // true
```

### 3. Property Operations with Proper Handling

```javascript
const obj = { _x: 10 };

// Safe property access
const value = Reflect.get(obj, '_x'); // 10

// With receiver (important for inheritance)
class Base {
  get x() { return this._x; }
}

class Child extends Base {
  constructor() {
    super();
    this._x = 20;
  }
}

const child = new Child();
console.log(Reflect.get(child, 'x', child)); // 20 (uses child's context)
```

### 4. Boolean Set Operations

```javascript
const obj = {};

// Reflect.set returns boolean indicating success
const success = Reflect.set(obj, 'name', 'Alice');
console.log(success); // true

// Prevent extensions
Reflect.preventExtensions(obj);
const failed = Reflect.set(obj, 'newProp', 'value');
console.log(failed); // false (can't add to non-extensible)
```

### 5. Complete Proxy with Reflect

```javascript
function createObservable(obj, onChange) {
  return new Proxy(obj, {
    get(target, prop, receiver) {
      const value = Reflect.get(target, prop, receiver);
      if (typeof value === 'object' && value !== null) {
        return createObservable(value, onChange);
      }
      return value;
    },
    set(target, prop, value, receiver) {
      const oldValue = target[prop];
      const success = Reflect.set(target, prop, value, receiver);
      if (success && oldValue !== value) {
        onChange(prop, value, oldValue);
      }
      return success;
    }
  });
}

const data = createObservable({ count: 0 }, (prop, newVal, oldVal) => {
  console.log(`${prop}: ${oldVal} → ${newVal}`);
});

data.count = 1;  // "count: 0 → 1"
data.count = 5;  // "count: 1 → 5"
```

### 6. Property Descriptor Manipulation

```javascript
const obj = {};

// Define with full control
Reflect.defineProperty(obj, 'name', {
  value: 'Alice',
  writable: false,
  enumerable: true,
  configurable: false
});

console.log(Reflect.getOwnPropertyDescriptor(obj, 'name'));
// { value: 'Alice', writable: false, enumerable: true, configurable: false }

// Check ownership
console.log(Reflect.ownKeys(obj)); // ['name']
console.log(Reflect.has(obj, 'name')); // true
```

## Common Mistakes

```javascript
// ❌ Wrong: Ignoring receiver in get/set
const base = {
  get x() { return this._x; },
  _x: 10
};

const child = Object.create(base);
child._x = 20;

// Using Reflect.get without receiver
console.log(Reflect.get(child, 'x')); // 10 (uses base's this)

// ✅ Correct: Pass receiver
console.log(Reflect.get(child, 'x', child)); // 20

// ❌ Wrong: Using apply without proper context
const fn = function() { return this.name; };
const context = { name: 'Alice' };

// Using Function.prototype.apply
fn.apply(context); // 'Alice'

// ✅ Correct: Reflect.apply is clearer
Reflect.apply(fn, context, []); // 'Alice'
```

## Reflect vs Function.prototype

```javascript
// Function.prototype methods
const fn = (a, b) => a + b;
const result1 = fn.apply(null, [1, 2]);

// Reflect methods
const result2 = Reflect.apply(fn, null, [1, 2]);

// Both return 3, but Reflect is more explicit and
// matches Proxy trap signatures exactly
```

## Quick Revision Summary

- `Reflect` provides methods for interceptable JavaScript operations
- Directly mirrors [[Proxy]] handler traps
- Always use `Reflect` inside Proxy traps for default behavior
- Returns boolean for set/delete operations
- Properly handles receiver/this context

## Related Topics

- [[Proxy]] - Intercept object operations (uses Reflect)
- [[Symbols]] - `Symbol.toPrimitive` and reflection
- [[ObjectDefineProperty]] - Property descriptor manipulation
- [[GettersSetters]] - Property accessor patterns
- [[MetaProgramming]] - Advanced reflection patterns
