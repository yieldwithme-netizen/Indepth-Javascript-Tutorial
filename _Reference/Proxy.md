# Proxy - Interceptors and Meta-Programming

## Definition

A `Proxy` wraps an object and intercepts fundamental operations like property access, assignment, enumeration, and function invocation. It enables custom behavior for default operations and is the foundation for many modern JavaScript patterns.

```javascript
const proxy = new Proxy(target, handler);
```

## Handler Trap Methods

| Trap | Intercepts |
|------|------------|
| `get` | Property access |
| `set` | Property assignment |
| `has` | `in` operator |
| `deleteProperty` | `delete` operator |
| `apply` | Function calls |
| `construct` | `new` operator |

## Common Use Cases

### 1. Data Validation

```javascript
const validator = {
  set(target, prop, value) {
    if (prop === 'age') {
      if (typeof value !== 'number' || value < 0 || value > 150) {
        throw new TypeError('Invalid age');
      }
    }
    target[prop] = value;
    return true;
  }
};

const person = new Proxy({}, validator);
person.age = 25;      // OK
person.age = -5;      // TypeError: Invalid age
person.age = 'old';   // TypeError: Invalid age
```

### 2. Default Values

```javascript
function createDefault(defaults) {
  return new Proxy({}, {
    get(target, prop) {
      return prop in target ? target[prop] : defaults[prop];
    }
  });
}

const settings = createDefault({
  theme: 'dark',
  fontSize: 14,
  language: 'en'
});

console.log(settings.theme);      // 'dark' (from defaults)
settings.theme = 'light';         // Override
console.log(settings.theme);      // 'light'
```

### 3. Private Properties

```javascript
function privateProperties(obj) {
  const privateMap = new WeakMap();
  
  return new Proxy(obj, {
    get(target, prop) {
      if (typeof prop === 'string' && prop.startsWith('_')) {
        return privateMap.get(target)?.[prop];
      }
      return target[prop];
    },
    set(target, prop, value) {
      if (typeof prop === 'string' && prop.startsWith('_')) {
        if (!privateMap.has(target)) privateMap.set(target, {});
        privateMap.get(target)[prop] = value;
        return true;
      }
      target[prop] = value;
      return true;
    }
  });
}

class User {
  constructor(name, password) {
    this.name = name;
    this._password = password; // Becomes private
  }
}

const user = privateProperties(new User('Alice', 'secret'));
console.log(user.name);           // 'Alice'
console.log(user._password);      // undefined (private)
user._password = 'newSecret';     // Stored internally
```

### 4. Negation Trap (`has`)

```javascript
const alwaysTrue = new Proxy({}, {
  has() { return true; }
});

console.log('anything' in alwaysTrue);  // true

const range = new Proxy({ min: 1, max: 100 }, {
  has(target, prop) {
    const num = Number(prop);
    return num >= target.min && num <= target.max;
  }
});

console.log(50 in range);   // true
console.log(150 in range);  // false
```

### 5. Function Interception

```javascript
function createLogger(fn) {
  return new Proxy(fn, {
    apply(target, thisArg, args) {
      console.log(`Calling ${target.name} with`, args);
      const result = Reflect.apply(target, thisArg, args);
      console.log(`Result:`, result);
      return result;
    }
  });
}

const add = createLogger((a, b) => a + b);
add(2, 3);
// Calling add with [2, 3]
// Result: 5
```

## Common Mistakes

```javascript
// ❌ Wrong: Forgetting to return true from set trap
const proxy = new Proxy({}, {
  set(target, prop, value) {
    target[prop] = value;
    // Missing return statement
  }
});

// ✅ Correct: Always return true from set for successful operation
const proxy2 = new Proxy({}, {
  set(target, prop, value) {
    target[prop] = value;
    return true;
  }
});

// ❌ Wrong: Creating infinite recursion
const proxy = new Proxy(obj, {
  get(target, prop) {
    return target[prop]; // This triggers get again!
  }
});

// ✅ Correct: Use Reflect
const proxy = new Proxy(obj, {
  get(target, prop) {
    return Reflect.get(target, prop);
  }
});
```

## Proxy vs Reflect

```javascript
// Proxy and Reflect work together
const proxy = new Proxy(target, {
  get(target, prop, receiver) {
    console.log(`Accessing ${prop}`);
    return Reflect.get(target, prop, receiver);
  }
});

// Reflect provides default behavior for all traps
Reflect.get(target, prop);
Reflect.set(target, prop, value);
Reflect.has(target, prop);
```

## Quick Revision Summary

- `Proxy` wraps objects and intercepts operations
- Handler defines traps for different operations
- `Reflect` provides default behavior for traps
- Use cases: validation, defaults, private data, logging
- Cannot proxy certain built-in objects (e.g., `Date`)

## Related Topics

- [[Reflect]] - Companion API for Proxy traps
- [[Symbols]] - `Symbol.toPrimitive` and proxy behavior
- [[GettersSetters]] - Property accessor alternatives
- [[ObjectDefineProperty]] - Pre-Proxy property control
- [[MetaProgramming]] - Advanced patterns and metaclasses
