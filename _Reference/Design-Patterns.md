# Design Patterns in JavaScript

## Definition

Design patterns are reusable solutions to common problems in software design. They are templates or blueprints that help developers solve specific issues efficiently. In JavaScript, design patterns help write maintainable, scalable, and organized code.

## Types of Design Patterns

### 1. Singleton Pattern

Ensures only one instance of a class exists.

```javascript
class Database {
  constructor() {
    if (Database.instance) {
      return Database.instance;
    }
    this.connection = null;
    Database.instance = this;
  }

  connect() {
    this.connection = 'Connected to database';
    return this;
  }

  query(sql) {
    return `Executing: ${sql}`;
  }
}

const db1 = new Database();
const db2 = new Database();

console.log(db1 === db2); // true - same instance
db1.connect();
console.log(db2.connection); // "Connected to database"
```

### 2. Factory Pattern

Creates objects without specifying the exact class.

```javascript
class User {
  constructor(name, role) {
    this.name = name;
    this.role = role;
  }
}

class UserFactory {
  static createUser(name, role) {
    switch (role) {
      case 'admin':
        return new User(name, 'admin');
      case 'editor':
        return new User(name, 'editor');
      case 'viewer':
        return new User(name, 'viewer');
      default:
        throw new Error('Unknown role');
    }
  }
}

const admin = UserFactory.createUser('Alice', 'admin');
const editor = UserFactory.createUser('Bob', 'editor');
console.log(admin.role); // "admin"
console.log(editor.role); // "editor"
```

### 3. Observer Pattern

Defines a subscription mechanism to notify multiple objects about events.

```javascript
class EventEmitter {
  constructor() {
    this.events = {};
  }

  on(event, callback) {
    if (!this.events[event]) {
      this.events[event] = [];
    }
    this.events[event].push(callback);
    return () => this.off(event, callback);
  }

  off(event, callback) {
    if (!this.events[event]) return;
    this.events[event] = this.events[event].filter(cb => cb !== callback);
  }

  emit(event, data) {
    if (!this.events[event]) return;
    this.events[event].forEach(callback => callback(data));
  }
}

// Usage
const emitter = new EventEmitter();
const unsubscribe = emitter.on('message', (data) => {
  console.log('Received:', data);
});

emitter.emit('message', 'Hello!'); // "Received: Hello!"
unsubscribe(); // Stop listening
```

### 4. Module Pattern

Encapsulates code into private and public scopes.

```javascript
const ShoppingCart = (() => {
  let items = []; // Private

  return {
    addItem(item) {
      items.push(item);
      console.log(`Added ${item.name}`);
    },
    getItems() {
      return [...items]; // Return copy
    },
    getTotal() {
      return items.reduce((sum, item) => sum + item.price, 0);
    }
  };
})();

shoppingCart.addItem({ name: 'Laptop', price: 999 });
console.log(shoppingCart.getTotal()); // 999
```

### 5. Prototype Pattern

Creates new objects based on an existing object.

```javascript
const carPrototype = {
  start() {
    console.log(`${this.model} engine started`);
  },
  stop() {
    console.log(`${this.model} engine stopped`);
  }
};

function createCar(model, color) {
  const car = Object.create(carPrototype);
  car.model = model;
  car.color = color;
  return car;
}

const car1 = createCar('Toyota', 'Red');
const car2 = createCar('Honda', 'Blue');

car1.start(); // "Toyota engine started"
car2.start(); // "Honda engine started"
```

### 6. Decorator Pattern

Dynamically adds behavior to objects.

```javascript
class Coffee {
  cost() {
    return 5;
  }
  description() {
    return 'Simple coffee';
  }
}

function withMilk(coffee) {
  const cost = coffee.cost();
  const desc = coffee.description();
  coffee.cost = () => cost + 2;
  coffee.description = () => desc + ', milk';
  return coffee;
}

function withSugar(coffee) {
  const cost = coffee.cost();
  const desc = coffee.description();
  coffee.cost = () => cost + 1;
  coffee.description = () => desc + ', sugar';
  return coffee;
}

let myCoffee = new Coffee();
myCoffee = withMilk(myCoffee);
myCoffee = withSugar(myCoffee);

console.log(myCoffee.description()); // "Simple coffee, milk, sugar"
console.log(myCoffee.cost()); // 8
```

### 7. Strategy Pattern

Defines a family of algorithms and makes them interchangeable.

```javascript
const strategies = {
  add: (a, b) => a + b,
  subtract: (a, b) => a - b,
  multiply: (a, b) => a * b
};

function calculate(strategy, a, b) {
  return strategies[strategy](a, b);
}

console.log(calculate('add', 5, 3)); // 8
console.log(calculate('multiply', 5, 3)); // 15
```

## Common Use Cases

### Event Handling

```javascript
// Observer pattern for event handling
class EventBus {
  static instance = null;

  static getInstance() {
    if (!EventBus.instance) {
      EventBus.instance = new EventBus();
    }
    return EventBus.instance;
  }
}

// Using native events (Observer pattern)
document.addEventListener('click', (e) => {
  console.log('Clicked at:', e.clientX, e.clientY);
});
```

### State Management

```javascript
// Module pattern for state management
const createState = (initialState) => {
  let state = { ...initialState };
  const listeners = [];

  return {
    getState: () => ({ ...state }),
    setState: (newState) => {
      state = { ...state, ...newState };
      listeners.forEach(listener => listener(state));
    },
    subscribe: (listener) => {
      listeners.push(listener);
      return () => {
        const index = listeners.indexOf(listener);
        listeners.splice(index, 1);
      };
    }
  };
};

const counter = createState({ count: 0 });
counter.subscribe(state => console.log('Count:', state.count));
counter.setState({ count: 1 }); // "Count: 1"
```

## Common Mistakes

### 1. Overusing Singleton

```javascript
// BAD: Hard to test and maintain
class BadSingleton {
  constructor() {
    if (BadSingleton.instance) return BadSingleton.instance;
    BadSingleton.instance = this;
    this.data = {};
  }
}

// BETTER: Use dependency injection
class BetterService {
  constructor(database) {
    this.db = database;
  }
}
```

### 2. Creating Tight Coupling

```javascript
// BAD: Direct dependency
class OrderProcessor {
  process(order) {
    const payment = new PayPalPayment(); // Tight coupling
    payment.charge(order.total);
  }
}

// BETTER: Use dependency injection
class OrderProcessor {
  constructor(paymentService) {
    this.payment = paymentService;
  }
  process(order) {
    this.payment.charge(order.total);
  }
}
```

### 3. Ignoring JavaScript's Built-in Patterns

```javascript
// BAD: Custom observer when events exist
class CustomEvent {
  // ... custom implementation
}

// BETTER: Use built-in EventTarget
const emitter = new EventTarget();
emitter.addEventListener('custom', handler);
emitter.dispatchEvent(new CustomEvent('custom', { detail: data }));
```

## Quick Revision Summary

- **Singleton**: One instance only
- **Factory**: Create objects without specifying class
- **Observer**: Subscribe/notify mechanism
- **Module**: Encapsulate private/public scope
- **Prototype**: Create objects from existing ones
- **Decorator**: Add behavior dynamically
- **Strategy**: Interchangeable algorithms

## Related Topics

- [[Singleton-Pattern]] - Deep dive into Singleton
- [[Factory-Pattern]] - Factory pattern variations
- [[Observer-Pattern]] - Event-driven architecture
- [[Module-Pattern]] - JavaScript modules
- [[OOP-Concepts]] - Object-oriented programming
- [[SOLID-Principles]] - Design principles
- [[Dependency-Injection]] - Inversion of control
