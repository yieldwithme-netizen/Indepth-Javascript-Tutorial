# How to Use Constructors

This guide covers practical patterns for using constructors effectively in JavaScript classes, including validation, computed properties, and initialization strategies.

## Basic Usage

The `constructor()` method is called with the `new` keyword and initializes object properties:

```javascript
class Car {
  constructor(make, model, year) {
    this.make = make;
    this.model = model;
    this.year = year;
    this.isRunning = false;
  }
}

const myCar = new Car("Toyota", "Camry", 2024);
console.log(myCar.make);    // Output: Toyota
console.log(myCar.isRunning); // Output: false
```

## Constructor with Validation

Use the constructor to validate data before assigning it:

```javascript
class BankAccount {
  constructor(owner, balance = 0) {
    if (typeof owner !== "string" || owner.length === 0) {
      throw new Error("Owner name must be a non-empty string");
    }
    if (balance < 0) {
      throw new Error("Initial balance cannot be negative");
    }

    this.owner = owner;
    this.balance = balance;
    this.createdAt = new Date();
  }
}

const account = new BankAccount("Alice", 1000);
console.log(account.balance); // Output: 1000
```

## Constructor with Default Values

Provide sensible defaults for optional parameters:

```javascript
class Task {
  constructor(title, priority = "medium", completed = false) {
    this.title = title;
    this.priority = priority;
    this.completed = completed;
    this.id = Date.now();
  }
}

const task1 = new Task("Write documentation");
console.log(task1.priority);  // Output: medium
console.log(task1.completed); // Output: false
```

## Constructor with Computed Properties

Calculate derived values during initialization:

```javascript
class Employee {
  constructor(firstName, lastName, hourlyRate, hoursWorked) {
    this.firstName = firstName;
    this.lastName = lastName;
    this.hourlyRate = hourlyRate;
    this.hoursWorked = hoursWorked;
    this.fullName = `${firstName} ${lastName}`;
    this.weeklyPay = hourlyRate * hoursWorked;
  }
}

const emp = new Employee("John", "Doe", 25, 40);
console.log(emp.fullName);  // Output: John Doe
console.log(emp.weeklyPay); // Output: 1000
```

## Constructor with Object Initialization

Accept objects for more flexible initialization:

```javascript
class Config {
  constructor({ host = "localhost", port = 3000, debug = false } = {}) {
    this.host = host;
    this.port = port;
    this.debug = debug;
  }
}

const devConfig = new Config({ debug: true, port: 5000 });
const prodConfig = new Config({ host: "api.production.com", port: 443 });
```

## Constructor Chaining Pattern

One constructor calling another within the same class using factory methods:

```javascript
class Point {
  constructor(x, y) {
    this.x = x;
    this.y = y;
  }

  static fromPolar(r, theta) {
    return new Point(
      r * Math.cos(theta),
      r * Math.sin(theta)
    );
  }
}

const cartesian = new Point(3, 4);
const polar = Point.fromPolar(5, Math.PI / 4);
```

## Constructor with Private Fields

Use private fields for encapsulation:

```javascript
class SecureData {
  #encryptionKey;

  constructor(data, key) {
    this.#encryptionKey = key;
    this.encryptedData = this.#encrypt(data);
  }

  #encrypt(data) {
    return btoa(data + this.#encryptionKey);
  }
}
```

## Common Mistakes

1. **Not validating input types**
   ```javascript
   // Bad
   class User {
     constructor(name) {
       this.name = name; // No validation
     }
   }

   // Better
   class User {
     constructor(name) {
       if (typeof name !== "string") {
         throw new TypeError("Name must be a string");
       }
       this.name = name;
     }
   }
   ```

2. **Calling constructor methods before initializing all properties**
   ```javascript
   class Bad {
     constructor() {
       this.doSomething(); // Method may rely on uninitialized properties
       this.value = 10;
     }
   }
   ```

3. **Forgetting to initialize derived/computed properties in constructor**

## Common Use Cases

- Setting up database connections
- Initializing UI components with configuration
- Creating data models with validation
- Setting default state for Redux/Vuex stores
- Initializing class instances from API responses

## Related Topics

- [[What-is-Constructor]]
- [[What-is-Super]]
- [[Call-Parent]]
- [[What-is-Private]]
- [[What-is-Static]]

## Quick Revision

| Pattern | Description |
|---------|-------------|
| Basic | `constructor(a, b) { this.a = a; }` |
| Default params | `constructor(name = "default")` |
| Validation | Check input before assignment |
| Computed values | `this.fullName = first + last` |
| Object params | `constructor({ key = val } = {})` |
| Factory method | Static method returning new instance |
| Private fields | `#fieldName` for encapsulation |
