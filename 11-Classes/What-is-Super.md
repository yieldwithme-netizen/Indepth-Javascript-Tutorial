# What is `super`?

`super` is a keyword that allows a child class to access and call methods and properties from its parent class. It is essential for working with inheritance in JavaScript classes.

## Definition

`super` can be used in two ways:
1. **`super()`** — Calls the parent class constructor
2. **`super.method()`** — Calls a method from the parent class

## Syntax

```javascript
class Parent {
  constructor(name) {
    this.name = name;
  }

  greet() {
    return `Hello, I'm ${this.name}`;
  }
}

class Child extends Parent {
  constructor(name, age) {
    super(name); // Call parent constructor
    this.age = age;
  }

  greet() {
    const parentGreeting = super.greet(); // Call parent method
    return `${parentGreeting} and I'm ${this.age} years old`;
  }
}
```

## Calling Parent Constructor

```javascript
class Person {
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  fullName() {
    return `${this.firstName} ${this.lastName}`;
  }
}

class Student extends Person {
  constructor(firstName, lastName, grade) {
    super(firstName, lastName); // Pass arguments to parent
    this.grade = grade;
  }
}

const student = new Student("Jane", "Smith", "A");
console.log(student.fullName()); // Output: Jane Smith
console.log(student.grade);      // Output: A
```

## Calling Parent Methods

```javascript
class BaseWidget {
  constructor(id) {
    this.id = id;
    this.isVisible = true;
  }

  show() {
    this.isVisible = true;
    return `Widget ${this.id} is now visible`;
  }

  hide() {
    this.isVisible = false;
    return `Widget ${this.id} is now hidden`;
  }
}

class Modal extends BaseWidget {
  constructor(id, title) {
    super(id);
    this.title = title;
    this.isModal = true;
  }

  show() {
    const message = super.show(); // Get parent's behavior
    return `${message} (Modal: ${this.title})`;
  }

  showOverlay() {
    return super.show() + " with overlay";
  }
}

const modal = new Modal("modal1", "Confirm Dialog");
console.log(modal.show()); // Output: Widget modal1 is now visible (Modal: Confirm Dialog)
```

## Super with Property Access

```javascript
class Animal {
  constructor(name) {
    this._name = name;
  }

  get name() {
    return this._name;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }

  getInfo() {
    // Access parent's getter
    return `${super.name} is a ${this.breed}`;
  }
}

const dog = new Dog("Rex", "German Shepherd");
console.log(dog.getInfo()); // Output: Rex is a German Shepherd
```

## Super with Static Methods

```javascript
class Logger {
  static createLogger(type) {
    return { type, timestamp: new Date() };
  }
}

class FileLogger extends Logger {
  static createLogger(filename) {
    const base = super.createLogger("file"); // Call parent static
    return { ...base, filename };
  }
}

console.log(FileLogger.createLogger("app.log"));
// Output: { type: 'file', timestamp: [Date], filename: 'app.log' }
```

## Super in Different Scenarios

```javascript
class Base {
  constructor() {
    this.baseValue = 10;
  }

  getValue() {
    return this.baseValue;
  }
}

class Derived extends Base {
  constructor() {
    super();
    this.derivedValue = 20;
  }

  getBothValues() {
    return {
      base: super.getValue(),
      derived: this.derivedValue
    };
  }

  getBaseProperty() {
    return super.getValue(); // Call parent method
  }
}
```

## Common Use Cases

- Initializing parent class state
- Extending parent method behavior
- Calling parent cleanup in destructor-like patterns
- Building layered architectures
- Creating middleware chains
- Framework hooks and lifecycle methods

## Common Mistakes

1. **Using `super()` before `this` in constructor**
   ```javascript
   // Wrong
   class Child extends Parent {
     constructor() {
       this.name = "child"; // ReferenceError
       super();             // Too late!
     }
   }
   ```

2. **Calling `super()` without `extends`**
   ```javascript
   // Wrong
   class Standalone {
     constructor() {
       super(); // SyntaxError
     }
   }
   ```

3. **Using `super` to access private fields of parent**
   ```javascript
   class Parent {
     #secret = "hidden";
   }

   class Child extends Parent {
     showSecret() {
       return super.#secret; // SyntaxError - cannot access private fields
     }
   }
   ```

4. **Forgetting to return parent method result**
   ```javascript
   // Bad
   greet() {
     super.greet(); // Discards return value
     // ... additional logic
   }

   // Better
   greet() {
     const result = super.greet(); // Capture return value
     return result + " (extended)";
   }
   ```

## Related Topics

- [[What-is-Constructor]]
- [[Use-Extends]]
- [[Call-Parent]]
- [[What-is-Static]]
- [[Use-GetSet]]

## Quick Revision

| Usage | Purpose | Example |
|-------|---------|---------|
| `super()` | Call parent constructor | `super(name, age)` |
| `super.method()` | Call parent method | `super.toString()` |
| `super.property` | Access parent property | `super.name` |
| `super.getter` | Access parent getter | `super.getName` |
| `super.static()` | Call parent static method | `super.create()` |
| Requirement | Must call `super()` before `this` in constructor |
| Scope | Only works in derived classes |
