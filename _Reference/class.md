# Classes in JavaScript

## Definition

Classes are templates for creating objects that encapsulate data (properties) and behavior (methods). Introduced in ES6, classes provide a cleaner, more intuitive syntax for object-oriented programming in JavaScript, replacing the traditional prototype-based inheritance.

## Syntax

```javascript
class ClassName {
  constructor(parameters) {
    // Initialize properties
  }

  methodName() {
    // Method logic
  }
}

// Creating an instance
const instance = new ClassName();
```

## Basic Example

```javascript
class Person {
  constructor(name, age) {
    this.name = name;
    this.age = age;
  }

  greet() {
    return `Hello, my name is ${this.name} and I'm ${this.age} years old.`;
  }
}

const john = new Person('John', 30);
console.log(john.greet()); // "Hello, my name is John and I'm 30 years old."
```

## Inheritance

```javascript
class Animal {
  constructor(name) {
    this.name = name;
  }

  speak() {
    return `${this.name} makes a sound.`;
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name);
    this.breed = breed;
  }

  speak() {
    return `${this.name} barks!`;
  }

  fetch(item) {
    return `${this.name} fetches the ${item}.`;
  }
}

const rex = new Dog('Rex', 'German Shepherd');
console.log(rex.speak()); // "Rex barks!"
console.log(rex.fetch('ball')); // "Rex fetches the ball."
```

## Getters and Setters

```javascript
class BankAccount {
  constructor(owner, balance) {
    this.owner = owner;
    this._balance = balance; // Convention: underscore for private-like
  }

  get balance() {
    return this._balance;
  }

  set balance(amount) {
    if (amount < 0) {
      throw new Error('Balance cannot be negative');
    }
    this._balance = amount;
  }

  deposit(amount) {
    this.balance += amount;
  }
}

const account = new BankAccount('Alice', 1000);
console.log(account.balance); // 1000
account.deposit(500);
console.log(account.balance); // 1500
```

## Static Methods

```javascript
class MathUtils {
  static add(a, b) {
    return a + b;
  }

  static multiply(a, b) {
    return a * b;
  }
}

// Called on the class, not instances
console.log(MathUtils.add(5, 3)); // 8
```

## Private Fields

```javascript
class Counter {
  #count = 0;

  increment() {
    this.#count++;
  }

  decrement() {
    this.#count--;
  }

  get value() {
    return this.#count;
  }
}

const counter = new Counter();
counter.increment();
counter.increment();
console.log(counter.value); // 2
// console.log(counter.#count); // Error: Private field
```

## Common Use Cases

- **Data models** for database records
- **UI components** in frameworks like React
- **Service classes** for API interactions
- **Game entities** with shared behaviors

## Common Mistakes

```javascript
class MyClass {
  // ❌ Wrong: Forgetting 'new' keyword
  // const obj = MyClass(); // TypeError

  // ✅ Correct
  constructor() {}
}

// ❌ Wrong: Using arrow functions for methods (breaks 'this')
class Wrong {
  greet = () => {
    console.log(this.name); // Works but loses prototype benefits
  };
}

// ✅ Correct: Regular methods
class Correct {
  greet() {
    console.log(this.name);
  }
}
```

## Related Topics

- [[Prototypes]] - The underlying mechanism classes build upon
- [[this-keyword]] - Understanding context in class methods
- [[Inheritance]] - Extending class functionality
- [[Modules]] - Exporting and importing classes
- [[Encapsulation]] - Hiding internal implementation details

## Quick Revision

| Feature | Description |
|---------|-------------|
| `constructor` | Special method for initializing objects |
| `extends` | Creates a child class |
| `super` | Calls parent class constructor/methods |
| `static` | Methods called on class, not instances |
| `#field` | Private field (ES2022+) |
| `get/set` | Accessor properties |

**Key takeaway**: Classes provide syntactic sugar over prototypes, making OOP patterns more accessible and readable in JavaScript.