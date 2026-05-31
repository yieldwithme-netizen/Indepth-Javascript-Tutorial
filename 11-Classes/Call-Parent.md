# How to Call Parent Constructor

This guide covers different patterns for calling parent constructors in JavaScript, including passing arguments, handling optional parameters, and working with complex initialization flows.

## Basic Parent Constructor Call

Always call `super()` as the first statement in the child constructor:

```javascript
class Parent {
  constructor(name) {
    this.name = name;
  }
}

class Child extends Parent {
  constructor(name, age) {
    super(name); // Pass name to parent constructor
    this.age = age;
  }
}

const child = new Child("Alice", 10);
console.log(child.name); // Output: Alice
console.log(child.age);  // Output: 10
```

## Passing Multiple Arguments

```javascript
class Vehicle {
  constructor(make, model, year) {
    this.make = make;
    this.model = model;
    this.year = year;
  }

  getInfo() {
    return `${this.year} ${this.make} ${this.model}`;
  }
}

class Truck extends Vehicle {
  constructor(make, model, year, payload) {
    super(make, model, year); // Pass all parent arguments
    this.payload = payload;
  }

  getInfo() {
    return `${super.getInfo()} - Payload: ${this.payload}kg`;
  }
}

const truck = new Truck("Ford", "F-150", 2024, 1000);
console.log(truck.getInfo());
// Output: 2024 Ford F-150 - Payload: 1000kg
```

## Conditional Parent Constructor Call

Use spread operator or conditional logic for dynamic argument passing:

```javascript
class Config {
  constructor(options = {}) {
    this.host = options.host || "localhost";
    this.port = options.port || 3000;
  }
}

class DevConfig extends Config {
  constructor(devOptions = {}) {
    super({
      ...devOptions,
      host: devOptions.host || "127.0.0.1",
      debug: true
    });
    this.devMode = true;
  }
}

const config = new DevConfig({ port: 5000 });
console.log(config.host); // Output: 127.0.0.1
console.log(config.port); // Output: 5000
console.log(config.devMode); // Output: true
```

## Parent Constructor with Default Values

```javascript
class Database {
  constructor(host = "localhost", port = 5432, dbname = "app") {
    this.host = host;
    this.port = port;
    this.dbname = dbname;
  }
}

class ProductionDB extends Database {
  constructor(dbname, options = {}) {
    super(
      options.host || "prod-db.example.com",
      options.port || 5432,
      dbname
    );
    this.ssl = true;
  }
}
```

## Multiple Level Inheritance

```javascript
class LivingThing {
  constructor(name) {
    this.name = name;
    this.isAlive = true;
  }
}

class Animal extends LivingThing {
  constructor(name, species) {
    super(name); // Call LivingThing constructor
    this.species = species;
  }
}

class Pet extends Animal {
  constructor(name, species, owner) {
    super(name, species); // Call Animal constructor
    this.owner = owner;
  }
}

const pet = new Pet("Buddy", "Dog", "Alice");
console.log(pet.name);    // Output: Buddy
console.log(pet.species); // Output: Dog
console.log(pet.owner);   // Output: Alice
```

## Parent Constructor with Object Spread

```javascript
class Logger {
  constructor(options = {}) {
    this.level = options.level || "info";
    this.prefix = options.prefix || "";
    this.timestamp = options.timestamp !== false;
  }
}

class FileLogger extends Logger {
  constructor(filename, options = {}) {
    super({
      ...options,
      level: options.level || "debug"
    });
    this.filename = filename;
  }
}
```

## Calling Parent Constructor Conditionally

```javascript
class BaseComponent {
  constructor(config = {}) {
    this.enabled = config.enabled !== false;
    this.name = config.name || "unnamed";
  }
}

class ToggleableComponent extends BaseComponent {
  constructor(config = {}) {
    super(config); // Always call super first
    this.toggled = false;
  }

  toggle() {
    this.toggled = !this.toggled;
    return this.toggled;
  }
}
```

## Common Use Cases

- Framework component initialization
- Custom error types with additional context
- Database model inheritance
- Middleware classes
- Service classes extending base services

## Common Mistakes

1. **Not calling super before using `this`**
   ```javascript
   // Wrong
   class Child extends Parent {
     constructor(name) {
       this.name = name; // ReferenceError
       super(name);
     }
   }
   ```

2. **Passing wrong number of arguments to parent**
   ```javascript
   // Wrong - parent expects 3 args
   super(name); // Missing arguments

   // Correct
   super(name, model, year);
   ```

3. **Using `this` before `super()` in constructor**
   ```javascript
   // Wrong
   constructor(name) {
     console.log(this.name); // Cannot access before super
     super(name);
   }
   ```

4. **Calling super multiple times in constructor**
   ```javascript
   // Wrong
   constructor(name) {
     super(name);
     super(name); // SyntaxError
   }
   ```

## Related Topics

- [[What-is-Constructor]]
- [[What-is-Super]]
- [[Use-Extends]]
- [[What-is-Static]]
- [[What-is-Private]]

## Quick Revision

| Pattern | Example |
|---------|---------|
| Basic call | `super(name)` |
| Multiple args | `super(make, model, year)` |
| Object spread | `super({ ...options })` |
| Default values | `super(name \|\| "default")` |
| From child | Always first statement in constructor |
| Return value | Constructor returns nothing |
| Error case | Must use before `this` access |
