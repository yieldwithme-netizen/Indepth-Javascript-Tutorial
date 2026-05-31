# What is `static`?

The `static` keyword defines methods or properties that belong to the class itself, not to instances of the class. Static members are called on the class, not on objects created from the class.

## Definition

Static methods and properties:
- Are called on the class itself (e.g., `ClassName.method()`)
- Cannot be called on instances (e.g., `instance.method()` throws an error)
- Cannot access instance properties via `this`
- Are often used for utility functions, factory methods, and class-level operations

## Syntax

```javascript
class ClassName {
  static methodName() {
    // Method body
  }

  static property = value;
}

ClassName.methodName(); // Valid
ClassName.property;     // Valid
```

## Basic Example

```javascript
class MathUtils {
  static add(a, b) {
    return a + b;
  }

  static multiply(a, b) {
    return a * b;
  }
}

console.log(MathUtils.add(2, 3));      // Output: 5
console.log(MathUtils.multiply(4, 5)); // Output: 20
```

## Static vs Instance Methods

```javascript
class Counter {
  constructor() {
    this.count = 0;
  }

  // Instance method - operates on instance data
  increment() {
    this.count++;
    return this.count;
  }

  // Static method - operates on class-level data
  static getTotalInstances() {
    return Counter.instanceCount || 0;
  }
}

const counter1 = new Counter();
counter1.increment(); // 1
Counter.instanceCount = 1;

const counter2 = new Counter();
counter2.increment(); // 1
Counter.instanceCount = 2;

console.log(Counter.getTotalInstances()); // Output: 2
```

## Static Factory Methods

```javascript
class User {
  constructor(name, email, role) {
    this.name = name;
    this.email = email;
    this.role = role;
  }

  static createAdmin(name, email) {
    return new User(name, email, "admin");
  }

  static createGuest() {
    return new User("Guest", "guest@example.com", "viewer");
  }

  static fromJSON(json) {
    const data = JSON.parse(json);
    return new User(data.name, data.email, data.role);
  }
}

const admin = User.createAdmin("Alice", "alice@example.com");
const guest = User.createGuest();
const user = User.fromJSON('{"name":"Bob","email":"bob@example.com","role":"editor"}');
```

## Static Properties

```javascript
class Configuration {
  static MAX_CONNECTIONS = 10;
  static DEFAULT_TIMEOUT = 5000;
  static instances = new Map();

  constructor(name) {
    this.name = name;
    Configuration.instances.set(name, this);
  }

  static getInstance(name) {
    return Configuration.instances.get(name);
  }
}

console.log(Configuration.MAX_CONNECTIONS); // Output: 10
```

## Static in Inheritance

```javascript
class Animal {
  static create(type) {
    switch (type) {
      case "dog": return new Dog();
      case "cat": return new Cat();
      default: return new Animal();
    }
  }
}

class Dog extends Animal {
  speak() {
    return "Woof!";
  }
}

class Cat extends Animal {
  speak() {
    return "Meow!";
  }
}

const dog = Animal.create("dog");
console.log(dog.speak()); // Output: Woof!
```

## Common Use Cases

- Utility/helper functions (e.g., `Array.isArray()`)
- Factory methods
- Class-level counters or registries
- Constants and configuration
- Data transformation methods
- Validation methods

## Common Mistakes

1. **Trying to call static method on instance**
   ```javascript
   class MyClass {
     static myMethod() {}
   }

   const obj = new MyClass();
   obj.myMethod(); // TypeError: obj.myMethod is not a function
   ```

2. **Using `this` in static methods to access instance data**
   ```javascript
   // Wrong
   class User {
     constructor(name) {
       this.name = name;
     }

     static getName() {
       return this.name; // Undefined - no instance
     }
   }
   ```

3. **Confusing static methods with prototype methods**
   ```javascript
   class MyClass {
     static staticMethod() {} // On class
     protoMethod() {}         // On prototype
   }
   ```

4. **Forgetting static methods cannot use `new this()`**
   ```javascript
   // Wrong
   static create() {
     return new this(); // Works but can be confusing
   }
   ```

## Related Topics

- [[What-is-Constructor]]
- [[Use-Extends]]
- [[What-is-Super]]
- [[What-is-GetSet]]
- [[What-is-Private]]

## Quick Revision

| Feature | Static | Instance |
|---------|--------|----------|
| Called on | Class | Object |
| Access `this` | Class context | Object context |
| Memory | One copy | Per instance |
| Use case | Utilities, factories | Object behavior |
| Syntax | `static method()` | `method()` |
| Access | `Class.method()` | `instance.method()` |
| Can access instance props | No | Yes |
