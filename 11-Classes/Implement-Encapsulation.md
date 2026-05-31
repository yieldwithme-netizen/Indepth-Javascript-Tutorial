# How to Implement Encapsulation in JavaScript

## Definition

Encapsulation implementation involves restricting access to object internals and providing controlled access through public methods. JavaScript offers multiple approaches: private fields (`#`), closures, WeakMap, and getter/setter patterns.

## Implementation Methods

| Method | ES Version | Browser Support |
|--------|------------|-----------------|
| Private Fields (`#`) | ES2022 | Modern browsers |
| Closures | ES5+ | All browsers |
| WeakMap | ES6+ | All modern browsers |
| Getter/Setter | ES5+ | All browsers |
| Convention (`_`) | Any | All (no enforcement) |

## Code Examples

### Private Fields (ES2022+)

```javascript
class User {
  #id;
  #name;
  #email;
  #password;

  constructor(id, name, email, password) {
    this.#id = id;
    this.#name = name;
    this.#email = email;
    this.#password = this.#hash(password);
  }

  #hash(value) {
    return btoa(value); // Simple encoding (use proper hashing in production)
  }

  #verify(input) {
    return this.#hash(input) === this.#password;
  }

  get id() {
    return this.#id;
  }

  get name() {
    return this.#name;
  }

  set name(value) {
    if (typeof value !== 'string' || value.length < 2) {
      throw new Error('Name must be at least 2 characters');
    }
    this.#name = value;
  }

  get email() {
    return this.#email;
  }

  set email(value) {
    if (!/^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(value)) {
      throw new Error('Invalid email format');
    }
    this.#email = value;
  }

  authenticate(password) {
    if (!this.#verify(password)) {
      throw new Error('Authentication failed');
    }
    return true;
  }

  toJSON() {
    return {
      id: this.#id,
      name: this.#name,
      email: this.#email
    };
  }
}

const user = new User(1, 'John', 'john@example.com', 'secret123');
console.log(user.name); // "John"
user.name = 'Jane';
console.log(user.name); // "Jane"
// user.#password; // SyntaxError: Private field
```

### Closure-Based Encapsulation

```javascript
function createProduct(id, name, price) {
  let _stock = 0;
  let _price = price;

  const product = {
    get id() {
      return id;
    },
    get name() {
      return name;
    },
    get price() {
      return _price;
    },
    set price(value) {
      if (value < 0) throw new Error('Price cannot be negative');
      _price = value;
    },
    get stock() {
      return _stock;
    },
    addToStock(quantity) {
      if (quantity <= 0) throw new Error('Quantity must be positive');
      _stock += quantity;
      return this;
    },
    removeFromStock(quantity) {
      if (quantity <= 0) throw new Error('Quantity must be positive');
      if (quantity > _stock) throw new Error('Insufficient stock');
      _stock -= quantity;
      return this;
    },
    inStock() {
      return _stock > 0;
    },
    getInfo() {
      return {
        id,
        name,
        price: _price,
        stock: _stock
      };
    }
  };

  return product;
}

const laptop = createProduct(1, 'Laptop', 999);
laptop.addToStock(10).removeFromStock(3);
console.log(laptop.stock); // 7
console.log(laptop.price); // 999
// laptop._stock; // undefined
```

### WeakMap-Based Encapsulation

```javascript
const _private = new WeakMap();

class PaymentProcessor {
  constructor(apiKey) {
    _private.set(this, {
      apiKey,
      transactions: [],
      isInitialized: false
    });
  }

  initialize() {
    const priv = _private.get(this);
    if (priv.isInitialized) {
      throw new Error('Already initialized');
    }
    priv.isInitialized = true;
    return this;
  }

  charge(amount, description) {
    const priv = _private.get(this);
    if (!priv.isInitialized) {
      throw new Error('Processor not initialized');
    }

    const transaction = {
      id: Date.now(),
      amount,
      description,
      timestamp: new Date()
    };

    priv.transactions.push(transaction);
    return transaction;
  }

  getTransactions() {
    return [..._private.get(this).transactions];
  }

  getTotal() {
    return _private.get(this).transactions.reduce(
      (sum, t) => sum + t.amount, 0
    );
  }
}

const processor = new PaymentProcessor('api_key_123');
processor.initialize();
processor.charge(99.99, 'Product purchase');
processor.charge(49.99, 'Service fee');

console.log(processor.getTotal()); // 149.98
console.log(processor.getTransactions().length); // 2
// processor.apiKey; // undefined
```

### Encapsulation with Proxy Validation

```javascript
class ValidatedClass {
  #data = {};

  constructor(schema) {
    this.#schema = schema;
  }

  #schema;

  set(key, value) {
    const rule = this.#schema[key];
    if (!rule) {
      throw new Error(`Unknown property: ${key}`);
    }

    if (rule.type && typeof value !== rule.type) {
      throw new Error(`${key} must be of type ${rule.type}`);
    }

    if (rule.validate && !rule.validate(value)) {
      throw new Error(`${key} validation failed`);
    }

    this.#data[key] = value;
    return this;
  }

  get(key) {
    if (!(key in this.#data)) {
      throw new Error(`Property ${key} not set`);
    }
    return this.#data[key];
  }

  getAll() {
    return { ...this.#data };
  }
}

const userSchema = {
  name: { type: 'string', validate: v => v.length >= 2 },
  age: { type: 'number', validate: v => v >= 0 && v <= 150 },
  email: { type: 'string', validate: v => /^[^\s@]+@[^\s@]+\.[^\s@]+$/.test(v) }
};

const validatedUser = new ValidatedClass(userSchema);
validatedUser.set('name', 'John').set('age', 30).set('email', 'john@example.com');
console.log(validatedUser.getAll());
// { name: 'John', age: 30, email: 'john@example.com' }

// validatedUser.set('age', -5); // Error: age validation failed
```

## Common Use Cases

| Use Case | Implementation |
|----------|----------------|
| API Keys | Hide keys, expose auth methods |
| Database Connections | Pool management, connection state |
| Cache Systems | Internal storage, public get/set |
| State Machines | Hidden states, public transitions |
| Form Validators | Validation rules, public validation API |

## Common Mistakes

```javascript
// ❌ Wrong: Using underscore convention only
class BadExample {
  constructor() {
    this._secret = 'hidden'; // Still accessible!
  }
}

const bad = new BadExample();
console.log(bad._secret); // 'hidden' - not actually private

// ❌ Wrong: Exposing internal arrays directly
class BadCollection {
  #items = [];

  getItems() {
    return this.#items; // Returns reference!
  }
}

const badColl = new BadCollection();
const items = badColl.getItems();
items.push('hacked'); // Modifies internal state!

// ✅ Correct: Return copies
class GoodCollection {
  #items = [];

  getItems() {
    return [...this.#items]; // Returns copy
  }

  addItem(item) {
    this.#items.push(item);
    return this;
  }
}

// ❌ Wrong: No validation in setters
class BadConfig {
  #port;
  set port(value) {
    this.#port = value; // No validation
  }
}

// ✅ Correct: Validate before setting
class GoodConfig {
  #port;
  set port(value) {
    if (typeof value !== 'number' || value < 1 || value > 65535) {
      throw new Error('Invalid port number');
    }
    this.#port = value;
  }
  get port() {
    return this.#port;
  }
}
```

## Related Topics

- [[What-is-Encapsulation]] - Encapsulation principles
- [[Create-Class]] - Creating classes in JavaScript
- [[What-is-Polymorphism]] - Polymorphism concepts
- [[Override-Methods]] - Overriding methods
- [[Implement-Chaining]] - Method chaining patterns

## Quick Revision

| Method | Pros | Cons |
|--------|------|------|
| Private Fields | True privacy, ES standard | Limited browser support |
| Closures | Wide support, flexible | Memory overhead |
| WeakMap | No memory leaks, clean | More complex setup |
| Getter/Setter | Simple, readable | Not truly private |
| Convention | Universal | No enforcement |
